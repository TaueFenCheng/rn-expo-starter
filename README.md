# rn-expo-starter

**English** | [简体中文](#简体中文)

A cross-platform starter application built with **Expo SDK 57**, **React Native 0.86**, **TypeScript**, and **Expo Router**. It targets **iOS and Android**, with Expo web support.

## Features

- Expo managed workflow with Continuous Native Generation (CNG)
- File-based routing and starter tab screens with Expo Router
- TypeScript and Expo-compatible dependencies
- Starter assets, app icon, splash screen, and platform configuration
- iOS and Android run scripts

## Requirements

- Node.js LTS and npm
- **iOS:** macOS and a compatible Xcode for Simulator or local iOS builds. Expo Go on a physical iPhone works for supported features.
- **Android:** Android Studio and an emulator, or a physical Android device with Expo Go. Android development also works on Windows and Linux.
- An Expo account is only required for EAS cloud builds, submissions, and updates.

See the [Expo SDK 57 documentation](https://docs.expo.dev/versions/v57.0.0/) for compatible tools and APIs.

## Getting started

```bash
npm install
npx expo start
```

Press `i` in the Expo CLI to open the iOS Simulator, or `a` to open an Android emulator. You can also scan the QR code with Expo Go (your phone and development computer must be able to reach each other over the network).

```bash
npm run ios       # Start Metro and open the iOS Simulator (macOS + Xcode)
npm run android   # Start Metro and open an Android emulator
npm run web       # Start the web target
```

For native libraries not included in Expo Go, create a [development build](https://docs.expo.dev/develop/development-builds/introduction/).

## Project structure

```text
app/                  Expo Router routes, layouts, and starter screens
  (tabs)/             Tab navigator and tab screens
components/           Reusable UI components
constants/            Shared constants and theme values
assets/               App icon, splash screen, and static assets
app.json              Expo app identity and native platform configuration
```

## Development commands

```bash
npx expo start                 # Start Metro
npx expo start --clear         # Clear Metro cache and start
npx expo install <package>     # Install an SDK-compatible package
npx expo install --check       # Check Expo dependency compatibility
npx expo-doctor                # Diagnose project configuration
npx tsc --noEmit               # Type-check TypeScript
```

Use `npx expo install` for Expo and React Native ecosystem dependencies so Expo selects SDK-compatible versions.

## Build and publish to app stores

EAS Build can produce signed iOS and Android binaries in the cloud, so local Xcode and Android build tools are not required for cloud builds. Store developer accounts, signing, store metadata, and review approval are still required.

### 1. Configure the app identity

Before the first store build, set permanent, globally unique identifiers in `app.json` (`ios.bundleIdentifier` and `android.package`). The example below uses placeholders; replace them with your own values:

```json
{
  "expo": {
    "name": "Your App Name",
    "slug": "your-app-slug",
    "ios": { "bundleIdentifier": "com.yourcompany.yourapp" },
    "android": { "package": "com.yourcompany.yourapp" }
  }
}
```

Prepare production icons and splash assets, version/build numbers, privacy disclosures, and permission descriptions. Keep bundle/package identifiers stable after publishing.

### 2. Configure EAS

```bash
npm install --global eas-cli
eas login
eas build:configure
```

Review the generated `eas.json`. EAS can manage signing credentials; never commit credentials, private keys, or service-account files.

### 3. Build production binaries

```bash
eas build --platform ios --profile production
eas build --platform android --profile production
```

The iOS artifact can be submitted to App Store Connect/TestFlight. Android production builds normally produce an `.aab` for Google Play. Check the current SDK 57 build image and store requirements before release.

### 4. Test and submit

Install and test production builds on real iOS and Android devices, then submit:

```bash
eas submit --platform ios --profile production
eas submit --platform android --profile production
```

Public distribution requires an **Apple Developer Program** membership and a **Google Play Console** developer account. Complete each store's listing, screenshots, age rating, privacy/data-safety forms, export-compliance questions, and review submission. Uploading a build does not publish it automatically; each store controls review and release.

Official guides: [EAS Build](https://docs.expo.dev/build/introduction/), [EAS Submit](https://docs.expo.dev/submit/introduction/), [iOS deployment](https://docs.expo.dev/submit/ios/), and [Android deployment](https://docs.expo.dev/submit/android/).

### Optional: over-the-air updates

EAS Update can deliver compatible JavaScript and asset updates. It cannot replace a native binary when native code, native dependencies, or runtime compatibility changes. Configure and test channels/runtime versions before production use; do not use OTA to bypass app-store review for changes that require review.

## Native project generation

The `ios/` and `android/` directories are intentionally not committed. Expo app config and config plugins are the source of truth; generate native projects when needed with `npx expo prebuild`. Direct edits to generated projects may be lost when regenerating them, so use config plugins for repeatable native configuration.

## License

MIT. See [LICENSE](./LICENSE).

---

# 简体中文

这是一个基于 **Expo SDK 57**、**React Native 0.86**、**TypeScript** 和 **Expo Router** 的跨平台移动应用起步模板，目标平台为 **iOS 和 Android**，同时支持 Expo Web。

## 项目包含

- Expo 托管工作流与持续原生生成（CNG）
- 基于 Expo Router 的文件路由和 Tab 示例页面
- TypeScript 配置及与 Expo SDK 兼容的依赖
- 应用图标、启动页等基础资源和平台配置
- iOS 与 Android 启动脚本

## 环境要求

- Node.js LTS 和 npm
- **iOS：** 使用模拟器或本地构建需要 macOS 和兼容版本的 Xcode。实体 iPhone 可通过 Expo Go 运行其支持的功能。
- **Android：** 安装 Android Studio 和模拟器，或使用装有 Expo Go 的实体 Android 设备。也可在 Windows/Linux 上开发 Android。
- 只有使用 EAS 云构建、提交或 OTA 更新时才需要 Expo 账号。

工具链和 API 兼容信息请参考 [Expo SDK 57 文档](https://docs.expo.dev/versions/v57.0.0/)。

## 快速开始

```bash
npm install
npx expo start
```

在 Expo CLI 中按 `i` 打开 iOS 模拟器，按 `a` 打开 Android 模拟器；也可以用 Expo Go 扫描二维码（手机和开发电脑需要能够通过网络互相访问）。

```bash
npm run ios       # 启动 Metro 并打开 iOS 模拟器（需要 macOS + Xcode）
npm run android   # 启动 Metro 并打开 Android 模拟器
npm run web       # 启动 Web 目标
```

若使用 Expo Go 不包含的原生库，需要创建[开发构建](https://docs.expo.dev/develop/development-builds/introduction/)。

## 项目结构

```text
app/                  Expo Router 路由、布局和示例页面
  (tabs)/             Tab 导航及示例页面
components/           可复用 UI 组件
constants/            共享常量和主题值
assets/               应用图标、启动页及静态资源
app.json              Expo 应用标识和原生平台配置
```

## 常用开发命令

```bash
npx expo start                 # 启动 Metro
npx expo start --clear         # 清除 Metro 缓存并启动
npx expo install <package>     # 安装与当前 SDK 兼容的依赖
npx expo install --check       # 检查 Expo 依赖兼容性
npx expo-doctor                # 检查项目配置
npx tsc --noEmit               # TypeScript 类型检查
```

安装 Expo / React Native 生态依赖时优先使用 `npx expo install`，由 Expo 选择与当前 SDK 兼容的版本。

## 构建并发布到应用市场

可以使用 EAS Build 在云端构建并签名 iOS 和 Android 安装包，因此云构建不要求本机安装 Xcode 或 Android 构建工具。但发布仍需要相应的开发者账号、签名、商店资料以及通过平台审核。

### 1. 配置应用身份

首次商店构建前，在 `app.json` 中设置永久且全局唯一的 `ios.bundleIdentifier` 和 `android.package`。下面是占位示例，发布前必须替换：

```json
{
  "expo": {
    "name": "你的应用名称",
    "slug": "your-app-slug",
    "ios": { "bundleIdentifier": "com.yourcompany.yourapp" },
    "android": { "package": "com.yourcompany.yourapp" }
  }
}
```

同时准备正式版图标和启动页、版本号/构建号、隐私说明及功能所需的权限描述。首次发布后应保持 bundle ID / package name 不变。

### 2. 配置 EAS

```bash
npm install --global eas-cli
eas login
eas build:configure
```

检查生成的 `eas.json`。EAS 可以代管签名凭据；不要把签名凭据、私钥或服务账号文件提交到 Git。

### 3. 构建生产包

```bash
eas build --platform ios --profile production
eas build --platform android --profile production
```

iOS 构建产物可提交到 App Store Connect/TestFlight；Android 生产构建通常生成上传 Google Play 的 `.aab`。发布前确认当前 SDK 57 对应的 EAS 构建镜像及商店要求。

### 4. 测试并提交

先在真实 iOS 和 Android 设备上安装并测试生产构建，再提交：

```bash
eas submit --platform ios --profile production
eas submit --platform android --profile production
```

公开发布需要加入 **Apple Developer Program**，并注册 **Google Play Console** 开发者账号。还要分别完成商店介绍、截图、年龄分级、隐私/数据安全表单、出口合规问卷及审核提交。上传构建包不等于自动上架，最终审核和发布时间由应用商店决定。

官方文档：[EAS Build](https://docs.expo.dev/build/introduction/)、[EAS Submit](https://docs.expo.dev/submit/introduction/)、[iOS 发布](https://docs.expo.dev/submit/ios/)、[Android 发布](https://docs.expo.dev/submit/android/)。

### 可选：OTA 热更新

EAS Update 可发布兼容的 JavaScript 和静态资源更新。如果原生代码、原生依赖或运行时兼容性发生变化，则需要重新构建原生安装包。正式使用 OTA 前应配置并测试渠道/运行时版本；需要商店审核的变更不能用 OTA 绕过审核。

## 原生项目生成

`ios/` 和 `android/` 目录默认不提交到仓库。Expo app config 与 config plugin 是原生配置的主要来源；需要时可用 `npx expo prebuild` 生成原生项目。直接修改生成目录的内容可能在重新生成时丢失，应优先使用 config plugin 保存可重复的配置。

## 许可证

MIT，详见 [LICENSE](./LICENSE)。
