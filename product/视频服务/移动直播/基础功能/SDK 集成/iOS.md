本文主要介绍如何快速地将腾讯云移动直播 LiteAVSDK（iOS）集成到您的项目中，按照如下步骤进行配置，就可以完成 SDK 的集成工作。下面以 [全功能专业版](https://cloud.tencent.com/document/product/454/7873) 为例：

## 开发环境要求
- Xcode 9.0+。
- iOS 9.0 以上的 iPhone 或者 iPad 真机。
- 项目已配置有效的开发者签名。

## 集成 LiteAV SDK
您可以选择使用 CocoaPods 自动加载的方式，或者先下载 SDK，再将其导入到您当前的工程项目中。

### CocoaPods
#### 1. 安装 CocoaPods
在终端窗口中输入如下命令（需要提前在 Mac 中安装 Ruby 环境）：
```
sudo gem install cocoapods
```

#### 2. 创建 Podfile 文件
进入项目所在路径，输入以下命令行之后项目路径下会出现一个 Podfile 文件。
```
pod init
```

#### 3. 编辑 Podfile 文件
编辑 Podfile 文件，有如下有两种设置方式：
-  方式一：使用腾讯云 LiteAV SDK 的 podspec 文件路径。

```
  platform :ios, '8.0'
  
  target 'App' do
  pod 'TXLiteAVSDK_Professional', :podspec => 'http://pod-1252463788.cosgz.myqcloud.com/liteavsdkspec/TXLiteAVSDK_Professional.podspec'
  end
```

-  方式二：使用 CocoaPod 官方源，支持选择版本号。

```
   platform :ios, '8.0'
   source 'https://github.com/CocoaPods/Specs.git'
   
   target 'App' do
   pod 'TXLiteAVSDK_Professional'
   end
```

#### 4. 更新并安装 SDK
在终端窗口中输入如下命令以更新本地库文件，并安装 LiteAV SDK：
```
pod install
```
或使用以下命令更新本地库版本：
```
pod update
```

pod 命令执行完后，会生成集成了 SDK 的 `.xcworkspace` 后缀的工程文件，双击打开即可。


### 手动集成
1. 下载 [LiveAVSDK ](https://cloud.tencent.com/document/product/454/7873) ，下载完成后进行解压。

2. 打开您的 Xcode 工程项目，选择要运行的 target , 选中 **Build Phases** 项。
![](https://main.qcloudimg.com/raw/d78299d12be0f6c3255eabec91941e7a.jpg)

3. 单击 **Link Binary with Libraries** 项展开，单击底下的“+”添加依赖库。
![](https://main.qcloudimg.com/raw/c3d1a562a7259c668336f24f0525c2db.jpg)

4. 依次添加所下载的 `TXLiteAVSDK_Professional.framework` 及其所需依赖库 :
```
libz.tbd
libc++.tbd
libresolv.tbd
libsqlite3.tbd
Accelerate.framework
```
![](https://main.qcloudimg.com/raw/3ec85750893f4866fe634a5d8dc6d059.jpg)

## 授权摄像头和麦克风使用权限
使用 SDK 的音视频功能，需要授权麦克风和摄像头的使用权限。在 App 的 Info.plist 中添加以下两项，分别对应麦克风和摄像头在系统弹出授权对话框时的提示信息。
- **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语。
- **Privacy - Camera Usage Description**，并填入摄像头使用目的提示语。

![](https://main.qcloudimg.com/raw/a924a0e1e7e7d0451dbd49cf97650dd2.jpg)

## 在工程中引入 SDK
项目代码中使用 SDK 有两种方式：
- 方式一： 在项目需要使用 SDK API 的文件里，添加模块引用。
```
@import TXLiteAVSDK_Professional;
```

- 方式二：在项目需要使用 SDK API 的文件里，引入具体的头文件。
```
#import "TXLiteAVSDK_Professional/TXLiteAVSDK.h"
```

## 给 SDK 配置 license 授权

单击 [License 申请](https://console.cloud.tencent.com/live/license) 获取测试用 license，您会获得两个字符串：一个字符串是 licenseURL，另一个字符串是解密 key。

在您的 App 调用 LiteAVSDK 的相关功能之前（建议在 `- [AppDelegate application:didFinishLaunchingWithOptions:]` 中）进行如下设置：

```objc
@import TXLiteAVSDK_Professional;
@implementation AppDelegate
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString * const licenceURL = @"<获取到的licenseUrl>";
    NSString * const licenceKey = @"<获取到的key>";
		
    //TXLiveBase 位于 "TXLiveBase.h" 头文件中
    [TXLiveBase setLicenceURL:licenceURL key:licenceKey]; 
    NSLog(@"SDK Version = %@", [TXLiveBase getSDKVersionStr]);
}
@end
```

## 常见问题
### 1. LiteAV SDK 是否支持后台运行？
支持，如需要进入后台仍然运行相关功能，可选中当前工程项目，在 **Capabilities** 下设置  **Background Modes** 为 **ON**，并勾选 **Audio，AirPlay and Picture in Picture** ，如下图所示：
![](https://main.qcloudimg.com/raw/ee8a9e445c6af84b5d1cec3869ed7a3a.jpg)

### 2. LiteAV SDK 是否支持开启 BitCode？
支持 Bitcode， App 项目可根据具体需要，在 **Build Settings** 下启用或禁用 Bitcode，如下图所示：
![](https://main.qcloudimg.com/raw/74326dbed0f09ce016101bb7396986bd.jpg)


