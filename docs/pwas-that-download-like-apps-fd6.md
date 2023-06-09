# 下载像🗜️这样的应用程序

> 原文：<https://dev.to/samthor/pwas-that-download-like-apps-fd6>

这是今天的一篇短文。(通过写那个，会让它成真的！)它也更像是一篇简短的开发者日志，而不是有一个特定的点😌

渐进式网络应用是当今所有现代浏览器都支持的东西。通过阅读这个网站，你正在使用一个:如果你断开互联网连接，但加载 [dev.to](https://dev.to) ，你会得到一个可爱的离线页面，你可以在那里涂鸦。🖌️🎨🎊

为了构建服务工作者，PWA 的核心部分，您可能想要使用[工具箱](https://developers.google.com/web/tools/workbox/)。但是如果..你不知道吗？🤔

# 山姆的 Patented^网站安装模式

我将创建一个几乎空无一物的网站来“安装”完整的体验，而不是你通常使用的 PWA 方法——编写一些页面和资源，编写一个 SW，然后*缓存*这些相同的页面和资源。

这种完整的体验实际上将是一个存放在其他地方的`.tar`文件。我们来安装吧！🔜🖥️

## 创建一个实际站点

所以，要做到这一点，你需要一个真正的网站。创建一个名为`app.tar`的文件，包含它的资源:`index.html`、样式等。

## 寄存器软件

在我们的前台页面`index.html`中，我们像平常一样注册我们的软件:

```
<script>
if (!('serviceWorker' in navigator)) {
  throw new Error('unsupported');
}
navigator.serviceWorker.register('/sw.js').then((reg) => {
  console.info('registered');
  // TODO
});
</script> 
```

Enter fullscreen mode Exit fullscreen mode

我们只需要这个文件和下面的`sw.js`是由 HTTP 服务器服务的*真正的*。

## 安装处理器

在`sw.js`里面，我们可以这样做:

```
self.addEventListener('install', (ev) => {
  const p = (async() => {
    const response = await fetch('app.tar');
    const buffer = await response.arrayBuffer();

    const cache = await caches.open('app');
    const ops = [];
    untar(buffer, (file) => {
      if (file.name.endsWith('/')) {
        return;  // directory, ignore
      }
      const p = cache.put(file.name, new Response(file.buffer));
      ops.push(p);
    });
    await Promise.all(ops);
    // untar is a modified version of https://github.com/InvokIT/js-untar
  })();
  ev.waitUntil(p);
}); 
```

Enter fullscreen mode Exit fullscreen mode

太好了！我们现在已经下载了`app.tar`并将其内容安装到我们的缓存中。它可以包含我们喜欢的任何内容，并且不需要映射到你*实际上*通过 HTTP 提供的文件。

## 获取处理程序

我差点忘了。我们需要使用`sw.js` :
中的样板文件从缓存中提供服务

```
self.addEventListener('fetch', (ev) => {
  const p = (async() => {
    // TODO: make requests for '/index.html' match '/'
    const response = await caches.match(ev.request, {ignoreSearch: true});
    return response || fetch(ev.request);
  })();
  ev.respondWith(p);
}); 
```

Enter fullscreen mode Exit fullscreen mode

(这对于几乎所有有软件的站点来说都是一样的。)

# 不要在家里尝试这个

这主要是一个实验，看看从一个`.tar`文件安装一个网站是否可行。是的，它是！现在你也可以在网上享受安装应用程序的完整体验了。🙄

这里有一个演示！

Twelve