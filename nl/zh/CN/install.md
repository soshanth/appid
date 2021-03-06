---
copyright:
  years: 2017, 2018
lastupdated: "2018-01-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}

# 配置 SDK
{: #configuring}


## 设置 Android SDK
{: #android-setup}

使用 {{site.data.keyword.appid_short}} 客户端 SDK 构建 Android 应用程序，初始化该 SDK，认证用户，然后对受保护和不受保护的资源发起请求。
{:shortdesc}


### 开始之前

您需要以下信息：
  * {{site.data.keyword.appid_short_notm}} 服务的实例。
  * 您的租户标识。在服务仪表板的**服务凭证**选项卡中，单击**查看凭证**。您的租户标识会显示在 **TenantID** 字段中。这是用于初始化应用程序的唯一标识。
  * 您所在的 {{site.data.keyword.Bluemix}} 区域。您可以通过查看 UI 来找到您所在的区域。此值用于初始化应用程序。
    <table> <caption> 表 1. {{site.data.keyword.Bluemix_notm}} 区域及对应的 SDK 值</caption>
    <tr>
      <th> Bluemix 区域</th>
      <th> SDK 值</th>
    </tr>
    <tr>
      <td> 美国南部</td>
      <td> AppID.REGION_US_SOUTH</td>
    </tr>
    <tr>
      <td> 悉尼</td>
      <td> AppID.REGION_SYDNEY</td>
    </tr>
    <tr>
      <td> 英国</td>
      <td> AppID.REGION_UK</td>
    </tr>
  </table>

  * <a href="https://developers.google.com/web/tools/setup/" target="_blank">Android Studio 项目 <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a>，设置为使用 Gradle。

### 安装客户端 SDK

1. 创建 Android Studio 项目或打开现有项目。
2. 将 JitPack 存储库添加到根 `build.gradle` 文件。

  ```gradle
    allprojects {
	    repositories {
		    ...
		    maven { url 'https://jitpack.io' }
	    }
    }
  ```
  {: codeblock}

3. 打开应用程序的 `build.gradle` 文件。

    **注：**请确保打开应用程序的这一文件，而不是项目的 `build.gradle` 文件。
4. 找到该文件的 dependencies 部分，并为 {{site.data.keyword.appid_short_notm}} 客户端 SDK 添加编译依赖项。

  ```gradle
   dependencies {
       compile group: 'com.github.ibm-cloud-security:appid-clientsdk-android:1.+'
   }
  ```
  {: codeblock}

5. 找到 `defaultConfig` 部分，并添加以下代码行。

  ```gradle
  defaultConfig {
  ...
  manifestPlaceholders = ['appIdRedirectScheme': android.defaultConfig.applicationId]
  }
  ```
  {: codeblock}

6. 使用 Gradle 同步项目。单击**工具 ** > **Android ** > **使用 Gradle 文件同步项目**。

### 初始化客户端 SDK

通过将上下文、租户标识和区域参数传递到 initialize 方法来初始化客户端 SDK。在 Android 应用程序中，通常会将初始化代码放置在主活动的 onCreate 方法中，但这不是强制性的。

  ```java
  AppID.getInstance().initialize(getApplicationContext(), <tenantId>, AppID.REGION_UK);
  ```
  {: codeblock}

1. 将 *tenantId* 替换为您的服务 tenantId。
2. 将 *AppID.REGION_UK* 替换为您所在的 {{site.data.keyword.Bluemix_notm}} 区域。

有关更多信息，请参阅 <a href="https://github.com/ibm-cloud-security/appid-clientsdk-android" target="_blank">{{site.data.keyword.appid_short_notm}} Android GitHub 存储库 <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a>。

## 设置 iOS Swift SDK
{: #ios-setup}

使用 {{site.data.keyword.appid_short}} 客户端 SDK 构建 Swift 应用程序，初始化该 SDK，认证用户，然后对受保护和不受保护的资源发起请求。
{:shortdesc}


### 开始之前

您需要以下信息：
  * {{site.data.keyword.appid_short_notm}} 的实例。
  * 您的租户标识。在服务仪表板的**服务凭证**选项卡中，单击**查看凭证**。您的租户标识会显示在 **TenantID** 字段中。这是用于初始化应用程序的唯一标识。
  * 您所在的 {{site.data.keyword.Bluemix_notm}} 区域。您可以通过查看 UI 来找到您所在的区域。此值用于初始化应用程序。
    <table> <caption> 表 1. {{site.data.keyword.Bluemix_notm}} 区域及对应的 SDK 值</caption>
    <tr>
      <th> Bluemix 区域</th>
      <th> SDK 值</th>
    </tr>
    <tr>
      <td> 美国南部</td>
      <td> AppID.REGION_US_SOUTH</td>
    </tr>
    <tr>
      <td> 悉尼</td>
      <td> AppID.REGION_SYDNEY</td>
    </tr>
    <tr>
      <td> 英国</td>
      <td> AppID.REGION_UK</td>
    </tr>
  </table>

  * Xcode 项目（V8.1 或更高版本）。
  * CocoaPods（V1.1.0 或更高版本）。


### 安装客户端 SDK

{{site.data.keyword.appid_short_notm}} 客户端 SDK 通过 CocoaPods 进行分发；CocoaPods 是用于 Swift 和 Objective-C Cocoa 项目的依赖项管理器。CocoaPods 会下载工件，并将其提供给项目使用。

1. 创建 Xcode 项目或打开现有项目。
2. 在项目的目录中打开或创建 podfile。
3. 在项目的目标下，添加“BluemixAppID”pod 的依赖项。确保 `use_frameworks!` 命令也位于目标下。

  例如：


  ```swift
  target '<yourTarget>' do
     use_frameworks!
     pod 'BluemixAppID'
  end
  ```
  {: codeblock}

4. 要下载 `BluemixAppID` 依赖项，请运行以下命令。

  ```swift
  pod install --repo-update
  ```
  {: codeblock}

6. 打开 Xcode 项目并启用密钥链共享。在**项目设置**下，单击**功能 > 密钥链共享**。
7. 在**项目设置 > 信息 > URL 类型**下，添加 **URL 类型**。使用以下值填充**标识**文本框和 **URL 方案**文本框：`$(PRODUCT_BUNDLE_IDENTIFIER)`


### 初始化客户端 SDK

1. 将以下 import 语句添加到 `AppDelegate.swift` 文件中。

  ```swift
  import BluemixAppID
  ```
  {: codeblock}

2. 通过将租户标识和区域参数传递到 initialize 方法来初始化客户端 SDK。在 Swift 应用程序中，通常会将初始化代码放置在 AppDelegate 的 `application:didFinishLaunchingWithOptions` 方法中，但这不是强制性的。

  ```swift
  AppID.sharedInstance.initialize(tenantId: <tenantId>, bluemixRegion: AppID.Region_UK)
  ```
  {: codeblock}

  * 将 *tenantId* 替换为服务的租户标识。
  * 将 AppID.REGION_UK 替换为您所在的 {{site.data.keyword.appid_short_notm}} 区域。

3. 将以下代码添加到 AppDelegate 文件中。

  ```swift
  func application(_ application: UIApplication, open url: URL, options :[UIApplicationOpenURLOptionsKey : Any]) -> Bool {
          return AppID.sharedInstance.application(application, open: url, options: options)
      }
  ```
  {: codeblock}

有关更多信息，请参阅 <a href="https://github.com/ibm-cloud-security/appid-clientsdk-swift" target="_blank">{{site.data.keyword.appid_short_notm}} iOS GitHub 存储库 <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a>。

## 设置 Node.js SDK
{: #nodejs-setup}

### 开始之前

* 熟悉如何在 {{site.data.keyword.Bluemix_notm}} 上开发 Node.js 应用程序。
* {{site.data.keyword.appid_short_notm}} 服务器 SDK 要求 Node.js 服务器通过 <a href="http://expressjs.com/" target="_blank">Express 框架 <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a> 进行实施。

**注**：其他框架会使用 `Express` 框架，例如 LoopBack。可以将 {{site.data.keyword.appid_short_notm}} 服务器 SDK 与其中任意框架一起使用。


### 安装服务器 SDK


1. 使用命令行打开包含 Node.js 应用程序的目录。
2. 运行以下命令。

  ```
  npm install -save express
  npm install -save passport
  npm install -save bluemix-appid
  ```
  {: codeblock}

有关更多信息，请参阅 <a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs" target="_blank">{{site.data.keyword.appid_short_notm}} Node.js GitHub 存储库 <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a>。

## 设置 Swift SDK
{: #swift-setup}

### 开始之前

* 熟悉如何在 {{site.data.keyword.Bluemix}} 上开发 Swift 应用程序。
* 安装 Swift 3.0.2
* 安装 Kitura 1.6


### 安装 SDK

1. 打开 Swift 应用程序目录中的 `Package.swift` 文件，并添加 `appid-serversdk-swift` 依赖项。例如：


  ```swift
  import PackageDescription

  let package = Package(
      dependencies: [
          .Package(url: "https://github.com/ibm-cloud-security/appid-serversdk-swift.git", majorVersion: 1)
      ]
  )
  ```
  {: codeblock}

有关更多信息，请参阅 <a href="https://github.com/ibm-cloud-security/appid-serversdk-swift" target="_blank">{{site.data.keyword.appid_short_notm}} Swift GitHub 存储库 <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a>。
