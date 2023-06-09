# 利伯拉🚀con✨ Fastlane✨本土反应应用

> 原文：<https://dev.to/javymb/libera-aplicaciones-de-react-native-con-fastlane-9g8>

使用 React Native (JavaScript)创建应用程序就像童话一样一切都是粉红色直到发行新版的，特别是如果你不熟悉 iOS 或 Android 的母语。
向 App Store 和 Play Store 商店提交申请的过程，往往是一个令人沮丧、缓慢或绝望的经历虽然有很多文件，但并不总是很清楚或只是错过了许多重要的步骤，因为还有很多事情要做。

那就是他来救援的时候！
本文将向您介绍如何将 Android 应用程序的发射过程自动化。∞最常见的任务，如证书、应用程序编译、Beta 分发等。

Fastlane 是 iOS 和 Android 开发人员的一个工具，可帮助他们自动执行繁琐的任务，如生成屏幕截图、处理证书和启动应用程序。
真实证词:

> ∞我开始使用，我的生活改变了，现在我有更多的时间在书桌上喝杯咖啡放松一下，同时 Fastlane 负责一切。🙇

## 开始

安装前，请确保已安装最新的 Xcode 命令行工具，然后安装

`brew cask install fastlane;`
安装后，在您的本机根级反应项目内创建 fast lane/∞文件夹。然后，在此目录中创建一个名为 Fastfile 的文件(不带扩展名，仅 Fastlane)。

Fastfile 文件是我们将对车道(lanes)进行编码的位置。车道包含一组动作，您可以同步执行这些动作以自动化流程。操作是执行任务的函数。

让我们从这个基本模板 *Fastfile* 开始，如您所见，有一个 before_all 链接，它基本上执行状态检查，有三个动作，以确保它处于最后一个分支(branch)中，状态是干净的。

```
fastlane_version '2.53.1'

before_all do
  ensure_git_branch
  ensure_git_status_clean
  git_pull
end

platform :ios do
   # iOS Lanes
end

platform :android do
  # Android Lanes
end 
```

Enter fullscreen mode Exit fullscreen mode

## 认证

当您要启动一个新的应用程序时，这一切都是完美的，直到您必须签署并控制应用程序证书。

### iOS🍎

签署你的代码的最好方法是使用 match 在将 march 整合到「车道」(lane)之前，必须先执行下列步骤:

1-使用 Nuke 删除现有配置文件和证书。

2-通过 init 命令启动 match 配置。

`Fastlane match init`
3-在 ios 平台上创建使用配对的车道。

```
desc 'Fetch certificates and provisioning profiles'
  lane :certificates do
  match(app_identifier: 'com.app.bundle', type: 'development', readonly: true)
  match(app_identifier: 'com.app.bundle', type: 'appstore', readonly: true)
End 
```

Enter fullscreen mode Exit fullscreen mode

之后，您可以使用 fastlane ios 证书命令或使用证书作为其他车道上的功能。Match 会自动将配置文件和证书保存到您的 OS X 密钥(Keychain)中。

### 安卓🤖

在发布模式下使用“汇编”任务编译 Android 应用程序时，应用程序将自动签名。但你首先需要生成或预先生成签名密钥并将其添加到项目中，不用担心，可以查阅本 Facebook 指南了解如何做到这一点。

## 汇编(构建)

### iOS🍎

为了生成签名构建，我们将创建一个使用以前创建的证书存储库的车道，并使用 gym 快速轻松地构建我们的应用程序。过程结束时，我们会增加编译编号，把我们的申请送到【beta 测试 T2]服务

```
desc 'Build the iOS application.'
private_lane :build do
  certificates
  increment_build_number(xcodeproj: './ios/name.xcodeproj')
  gym(scheme: 'name', project: './ios/name.xcodeproj')
end 
```

Enter fullscreen mode Exit fullscreen mode

### 安卓🤖

为了生成签名的. apk，我们将创建编译车道。如您所见，我们正在使用 grad 操作，以清理工程并装备发射版本，任务为[grad](https://docs.gradle.org/current/userguide/userguide.html)。

```
desc 'Build the Android application.'
private_lane :build do
  gradle(task: 'clean', project_dir: 'android/')
  gradle(task: 'assemble', build_type: 'Release', project_dir: 'android/')
end 
```

Enter fullscreen mode Exit fullscreen mode

然后自动增加*版本代码*，将*汇编租赁*与这个小任务连接起来。

## 分配

### iOS🍎

testflight 是 IOs beta 测试时的前进之路。虽然开发者门户有点混乱，但它确实运行得很好。使用 pilot，我们可以管理我们的 testflight 编译。

beta 车道将使用编译车道向 pilot 提供一个签名的. ipa，然后将元素发送到 git，并通过增加编译编号来驱动所做的更改，最后将本地编译上载到 testflight。

```
desc 'Ship to Testflight.'
  lane :beta do
    build
    pilot
    commit_version_bump(message: 'Bump build', xcodeproj: './ios/name.xcodeproj')
    push_to_git_remote
end 
```

Enter fullscreen mode Exit fullscreen mode

### 安卓🤖

Android 使用 playstore 共享 beta 版。fastlane 也能实现自动化！

Android 的 beta 车道与 iOS 几乎相同，它使用编译车道生成已签名的. apk，确认版本代码的更改，使用 supply 以 beta 版形式促进 playstore 的本地编译。

```
desc 'Ship to Playstore Beta.'
lane :beta do
    build
    supply(track: 'beta', track_promote_to: 'beta')
    git_commit(path: ['./android/gradle.properties'], message: 'Bump versionCode')
    push_to_git_remote
end 
```

Enter fullscreen mode Exit fullscreen mode

你的 Fastfile 应该看起来很像下面显示的

```
fastlane_version '2.53.1'

before_all do
  ensure_git_branch
  ensure_git_status_clean
  git_pull
end

platform :ios do
   # iOS Lanes
  desc 'Fetch certificates and provisioning profiles'
  lane :certificates do
    match(app_identifier: 'com.app.bundle', type: 'development', readonly: true)
    match(app_identifier: 'com.app.bundle', type: 'appstore', readonly: true)
  end
  desc 'Build the iOS application.'
  private_lane :build do
    certificates
    increment_build_number(xcodeproj: './ios/name.xcodeproj')
    gym(scheme: 'name', project: './ios/name.xcodeproj')
  end
  desc 'Ship to Testflight.'
  lane :beta do
    build
    pilot
    commit_version_bump(message: 'Bump build', xcodeproj: './ios/name.xcodeproj')
    push_to_git_remote
  end
end

platform :android do
  # Android Lanes
  desc 'Build the Android application.'
  private_lane :build do
    gradle(task: 'clean', project_dir: 'android/')
    gradle(task: 'assemble', build_type: 'Release', project_dir: 'android/')
  end
  desc 'Ship to Playstore Beta.'
  lane :beta do
      build
      supply(track: 'beta', track_promote_to: 'beta')
      git_commit(path: ['./android/gradle.properties'], message: 'Bump versionCode')
      push_to_git_remote
  end
end 
```

Enter fullscreen mode Exit fullscreen mode

**谢谢**
Felix Krause，Carlos Cuesta

为我美丽的安曼徒争取惊人的支持！
你需要帮助吗？
给我留言，我很乐意帮你！

哈维尔·穆尼奥斯·巴里奥斯
([@ jaymb](https://dev.to/javymb))|推特