# 一个明星诞生了⭐

> 原文：<https://dev.to/matricar/a-star-is-born-544k>

### 新建域

最近我得到了一个新的。空间域名，这样我就可以用一个新的名字和外观重新启动我的网站。买了 [eldin.space](https://eldin.space) 。

### 新建站点

很久以来，我一直想对我的个人网站进行一次大修。我也不想只是重做。我想在建造它的时候学习一些新的东西。我对学习前端 JavaScript 框架感兴趣已经有一段时间了，Vue.js 引起了我的注意。它的模板语法和组件结构对我来说很容易理解。他们出色的文档让我觉得我可以学到很多关于当前前端框架的知识，而不会感到头痛。我已经和 Vue.js 一起工作了几个月，现在我想建立我的网站和博客。

### 我那里有什么？

*   主题(暗/亮)
*   响应能力(手机/平板电脑/台式机)
*   动画(一点点)
*   博客(与 Storyblok 一起)
*   简历，作品集，联系方式。

### Vuetify.js

让 wesite 变得好看有很多选择，从手写你的 css，使用 css 框架，或者因为我已经使用 Vue.js，使用 Vue 组件库，它有增加功能的额外好处。有各种各样的 vue 组件库，其中最有前途的是 Vuetify.js，它实现了谷歌的材料设计，看起来几乎是 android 手机中的原生。

### Animate.css

酷炫的动画和巧妙的微交互现在风靡一时。然而，大多数开发人员都知道不断编写和重写 CSS 动画是多么漫长和乏味。它会让你感觉像是在重新发明轮子。所以我用了 Animate.css，有了 Animate.css，你可以很容易的动画任何元素。

### 用 Storyblok 写博客

story blok+view . js =完美匹配

Storyblok 非常强大，并且提供了我所期望的 CMS 的所有基本特性。此外，可以通过添加自定义字段类型插件来扩展其功能，这些插件基本上是常规的 Vue.js 组件。定价惊人。我使用免费计划，它为我工作。

### 视图目标

尽管开发人员可能会忽略它，但 SEO 仍然是任何网站或 web 应用程序的重要组成部分。不容易被搜索引擎索引或优化不佳的应用程序和网站最终会隐藏在一页又一页的搜索结果后面。我不希望这种情况发生在我的项目上，所以我使用 vue-meta。vue-meta 是一个 Vue.js 插件，允许你管理应用程序的元数据，就像 react-helmet 对 react 所做的那样。然而，不是将数据设置为传递给专有组件的道具，而是使用 metaInfo 属性将其作为组件数据的一部分导出。当在深度嵌套的组件上设置这些属性时，它们将巧妙地覆盖其父组件的元数据信息，从而为每个顶级视图启用自定义信息，并将元数据直接耦合到深度嵌套的子组件，以获得更易于维护的代码。

### 用 Netlify 托管

虽然我已经开始建立网站，但我仍然需要一个地方来托管它。我用 Netlify。部署 JAMStack 站点最棒的部分是 git 触发的持续部署部分。每当我用 git push 更新我在 GitHub 上的存储库时，这个站点就会用我最近提交的更改进行重建。以这种方式部署更新会使这个过程自动化很多，因为我不需要自己编写构建命令，也不需要通过 FTP 手动发送 dist 文件夹中的更新文件。Netlify 还允许我用我买的域名建立一个自定义域名。

#### 🖖万岁，繁荣昌盛