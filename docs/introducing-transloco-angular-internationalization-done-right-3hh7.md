# 🚀Transloco 简介:角度国际化进展顺利

> 原文：<https://dev.to/netanelbasal/introducing-transloco-angular-internationalization-done-right-3hh7>

最初发表于[netbasal.com](https://netbasal.com/introducing-transloco-angular-internationalization-done-right-54710337630c)

有一段时间，我一直在考虑创建一个 Angular i18n 库，它包含了我脑海中的一些概念。

几周前，我很无聊😀，所以我决定是时候不畏艰险，继续创建一个健壮的库了，里面装满了我期望从这样一个库中获得的所有特性。

我招募了沙哈尔·卡扎兹和伊泰·奥德来完成这项任务；我们一起在工作日的晚上努力工作，这就是 Transloco 的诞生。

我还创建了 ng-neat 组织来保存我所有的开源库。目前它只持有 Transloco，但我也计划转移[旁观者](https://github.com/NetanelBasal/spectator)、 [ngx-until-destroy](https://github.com/NetanelBasal/ngx-take-until-destroy) 、ngx-content-loader 以及我将来创建的任何开源 Angular 库。

让我们看看 Transloco 提供了什么。

使用 Angular CLI 安装库:

```
ng add @ngneat/transloco 
```

[![](img/4d3b4b28ddd113790f17a8da7eac4603.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--YzGZMBA3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://cdn-images-1.medium.com/max/2600/1%2ARnSX-ybFkKsUYEawHDWJHQ.gif)

作为安装过程的一部分，您会遇到一些问题；一旦你回答了这些问题，所有你需要的东西都会自动为你生成。让我们仔细看看生成的文件。

首先，Transloco 为请求的翻译创建样板文件:

assets/i18n/en.json

```
{  "hello":  "transloco en",  "dynamic":  "transloco {{value}}"  } 
```

assets/i18n/es.json

```
{  "hello":  "transloco es",  "dynamic":  "transloco {{value}}"  } 
```

接下来，它将`TranslocoModule`注入到`AppModule`中，并为您设置一些默认选项:

```
import { TRANSLOCO_CONFIG, TranslocoModule } from '@ngneat/transloco';
import { HttpClientModule } from '@angular/common/http';
import { httpLoader } from './loaders/http.loader';
import { environment } from '../environments/environment';

@NgModule({
  imports: [TranslocoModule, HttpClientModule],
  providers: [
    httpLoader
    {
      provide: TRANSLOCO_CONFIG,
      useValue: {
        prodMode: environment.production,
        listenToLangChange: true,
        defaultLang: 'en'
      }
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule {} 
```

让我们解释一下每个配置选项:

*   `listenToLangChange`:订阅语言更改事件，允许您更改活动语言。在不允许用户在运行时更改语言的应用程序中，这是不需要的(例如，从下拉菜单中)，因此在这些情况下，通过将其设置为 false，您可以通过渲染视图一次并取消订阅语言更改事件(默认为 false)来节省内存
*   `defaultLang`:设置默认语言
*   `fallbackLang`:设置默认语言作为备用语言
*   `failedRetries`:如果加载失败，Transloco 应该重试加载翻译文件多少次(默认为 2 次)

它还将 httpLoader 注入到`AppModule`提供者:

```
import { HttpClient } from '@angular/common/http';
import { Translation, TRANSLOCO_LOADER, TranslocoLoader } from '@ngneat/transloco';
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class HttpLoader implements TranslocoLoader {
  constructor(private http: HttpClient) {}

  getTranslation(langPath: string) {
    return this.http.get<Translation>(`/assets/i18n/${langPath}.json`);
  }
}

export const httpLoader = { provide: TRANSLOCO_LOADER, useClass: HttpLoader }; 
```

`HttpLoader`是一个实现`TranslocoLoader`接口的类。它负责指导 transloco 如何加载翻译文件。它使用 Angular HTTP 客户端根据给定的路径获取文件。

## 模板中的翻译

Transloco 提供了三种翻译模板的方法:

### 使用结构化指令

这是推荐的方法。它简洁高效，因为它为每个模板创建一个订阅:

```
<ng-container *transloco="let t">
  <ul>
    <li>{{ t.home }}</li>
    <li>{{ t.alert | translocoParams: { value: dynamic } }}</li>
  </ul>
</ng-container> 
```

### 使用属性指令

```
<ul>
  <li><span transloco="home"></span></li>
  <li>
    <span transloco="alert" [translocoParams]="{ value: dynamic }"></span>
  </li>
  <li><span [transloco]="key"></span></li>
</ul> 
```

### 使用管道

```
<span>{{ 'home' | transloco }}</span> 
<span>{{ 'alert' | transloco: { value: dynamic } }}</span> 
```

## 程序翻译

有时您可能需要翻译组件或服务中的一个键。为此，您可以注入 TranslocoService 并使用它的 translate 方法:

```
export class AppComponent {
  constructor(private service: TranslocoService) {}

  ngOnInit() {
    this.service.translate('hello');
    this.service.translate('hello', { value: 'world' });
    this.service.translate(['hello', 'key']);
    this.service.translate('hello', params, 'en');
    this.service.translate<T>(translate => translate.someKey);
  }
} 
```

## 😱但是等等，还有更多！

这只是 Transloco 的一个尝试，它还支持:

*   😴惰性装载
*   😍多种语言
*   🔥多次回落
*   🤓测试
*   🦊完全可定制

您可以在我们的官方自述文件中找到您需要的所有内容和示例。

## 从 ngx 迁移-翻译

Transloco 提供了一个 schematics 命令来帮助您完成迁移过程。查看比较[表](https://github.com/ngneat/transloco/#comparison-to-other-libraries)了解更多信息。

翻译愉快，别忘了通过主演来表达你的欣赏！

⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️⭐️