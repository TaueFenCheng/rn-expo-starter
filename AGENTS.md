# Agent instructions | Agent 指引

This repository is a cross-platform **Expo SDK 57 / React Native 0.86 / TypeScript** starter targeting iOS and Android, with Expo web support. Prioritize mobile-first UX, accessibility, performance, security, and cross-platform correctness.

本仓库是基于 **Expo SDK 57 / React Native 0.86 / TypeScript** 的跨平台起步项目，支持 iOS、Android 和 Expo Web。开发时优先保证移动端体验、无障碍、性能、安全性及跨平台一致性。

## Project facts | 项目信息

- Package manager: npm (`package-lock.json` is committed). Use npm for scripts and lockfile changes.
- 包管理器：npm（仓库提交了 `package-lock.json`），运行脚本和变更 lockfile 时使用 npm。
- Routing: Expo Router. Routes are under the repository-root `app/` directory (not `src/app/`); `_layout.tsx` files define navigators.
- 路由：Expo Router。路由位于仓库根目录 `app/`（不是 `src/app/`），`_layout.tsx` 定义导航器。
- Shared UI and non-route code live in `components/`; app-wide constants live in `constants/`.
- 可复用 UI 和非路由代码放在 `components/`；应用级常量放在 `constants/`。
- The project uses Expo's managed workflow / Continuous Native Generation (CNG). `ios/` and `android/` are generated and ignored by Git unless the project deliberately changes its native-project strategy.
- 项目使用 Expo 托管工作流 / Continuous Native Generation（CNG）。除非项目明确改为原生工程管理，否则 `ios/` 和 `android/` 是生成目录并由 Git 忽略。
- Check `package.json` for exact versions. Expo SDK APIs and native dependencies change between SDK releases.
- 准确版本以 `package.json` 为准；Expo SDK API 和原生依赖可能随 SDK 版本变化。

## Documentation and compatibility | 文档与兼容性

Before changing Expo, EAS, or React Native APIs or adding native dependencies:

修改 Expo、EAS、React Native API 或添加原生依赖前：

1. Check the installed Expo SDK in `package.json`.
   检查 `package.json` 中安装的 Expo SDK 版本。
2. Read the matching versioned docs: `https://docs.expo.dev/versions/v57.0.0/` (update this link when upgrading SDK).
   阅读对应版本文档：`https://docs.expo.dev/versions/v57.0.0/`（升级 SDK 后同步更新链接）。
3. Follow current task-specific official docs; for broad Expo research, use `https://docs.expo.dev/llms.txt` and follow its links.
   查阅与任务相关的官方文档；广泛了解 Expo 时可从 `https://docs.expo.dev/llms.txt` 开始并沿链接阅读。
4. Install Expo / React Native ecosystem packages with `npx expo install <package>` so SDK-compatible versions are selected.
   使用 `npx expo install <package>` 安装 Expo / React Native 生态包，以选择兼容当前 SDK 的版本。

Do not assume the latest online docs apply to this project's SDK. Verify compatibility before upgrades.

不要假设最新版在线文档适用于本项目；升级依赖前先核实兼容性。

## Development commands | 开发命令

```bash
npm install
npx expo start
npx expo start --clear
npm run ios                 # requires macOS + Xcode for Simulator / 需要 macOS + Xcode 模拟器
npm run android             # requires Android Studio/emulator / 需要 Android Studio/模拟器
npm run web
npx expo install --check
npx expo-doctor
npx tsc --noEmit
```

Run the applicable checks (at minimum Expo Doctor and TypeScript) before declaring code changes complete. Test important UI and native behavior on both iOS and Android; simulators do not cover every real-device condition.

完成代码变更前运行适用检查（至少 Expo Doctor 和 TypeScript 检查）。重要 UI 和原生行为应在 iOS、Android 上验证；模拟器不能覆盖所有真机情况。

## React Native implementation rules | React Native 实现规范

- Use React Native primitives (`View`, `Text`, `Pressable`, `Image`, `FlatList`, etc.), not DOM elements or browser-only APIs, in native screens.
- 原生页面使用 React Native 组件（`View`、`Text`、`Pressable`、`Image`、`FlatList` 等），不要使用 DOM 元素或仅限浏览器的 API。
- Use Expo Router for navigation. Routes are files in root `app/`; `_layout.tsx` defines Stack/Tab layouts. Put reusable components, hooks, services, and utilities outside `app/`.
- 使用 Expo Router 导航。路由文件位于根目录 `app/`，`_layout.tsx` 定义 Stack/Tab；可复用组件、hooks、services 和 utilities 放在 `app/` 之外。
- Account for safe areas, keyboard behavior, touch targets, accessibility labels, platform conventions, app lifecycle, permissions, offline/weak network, and iOS/Android differences.
- 处理安全区域、键盘行为、触控区域、无障碍标签、平台交互规范、应用生命周期、权限、离线/弱网及 iOS/Android 差异。
- Use platform-specific files (`.ios.tsx`, `.android.tsx`) or `Platform.select` only where behavior genuinely differs.
- 仅在行为确有差异时使用平台文件（`.ios.tsx`、`.android.tsx`）或 `Platform.select`。
- Prefer virtualized lists for long collections, avoid unnecessary render work, and verify performance on a physical device for demanding screens.
- 长列表优先使用虚拟列表，避免不必要的渲染；高负载页面应在真机上验证性能。
- Store credentials and secrets securely; never commit signing keys, service-account files, private keys, or real secrets. Use SecureStore for sensitive device-side credentials where appropriate.
- 安全保存凭据和密钥；禁止提交签名密钥、服务账号文件、私钥或真实 secret。设备端敏感凭据可按需使用 SecureStore。
- Expo Go contains only its bundled native modules. For a library requiring native code, use a development build; verify Expo compatibility and rebuild the native app after native dependency/config changes.
- Expo Go 仅包含其内置原生模块。依赖原生代码的库需要开发构建；核实 Expo 兼容性，并在原生依赖/配置变更后重新构建 App。

## Native generation and configuration | 原生工程生成与配置

- Configure app identity, permissions, icons, splash screen, URL schemes, and supported native options in `app.json` / app config and config plugins.
- 在 `app.json` / app config 和 config plugins 中配置应用标识、权限、图标、启动页、URL scheme 及支持的原生选项。
- Do not hand-create or hand-edit generated `ios/` and `android/` projects as the source of truth. Use config plugins or intentional, documented native-project changes; remember that `npx expo prebuild --clean` regenerates and can erase direct native edits.
- 不要把手动创建/编辑的 `ios/`、`android/` 生成工程作为配置事实来源。使用 config plugins 或有明确文档记录的原生工程改动；`npx expo prebuild --clean` 会重新生成工程并可能清除直接修改。
- Local iOS Simulator/native builds require a macOS and Xcode version supported by the current Expo SDK. Cloud EAS builds use the configured/compatible Expo build image, but local builds use the host toolchain.
- 本地 iOS 模拟器/原生构建要求 macOS 和当前 Expo SDK 支持的 Xcode 版本。EAS 云构建使用配置/兼容的构建镜像，本地构建则使用当前电脑的工具链。

## Store-release workflow | 应用商店发布流程

When preparing a release, do not publish without explicit user approval. Guide the user through these steps:

准备发布时，未经用户明确批准不得提交或发布。按以下步骤指导用户：

1. Set permanent unique `ios.bundleIdentifier` and `android.package` values in `app.json`; configure app name, slug, icons, splash assets, version/build numbers, permissions, and production environment variables. Never change store identifiers after first publication without a migration plan.
   在 `app.json` 中设置永久且唯一的 `ios.bundleIdentifier` 和 `android.package`；配置应用名称、slug、图标、启动资源、版本/构建号、权限和生产环境变量。首次发布后，除非有迁移方案，否则不要更改商店标识。
2. Configure EAS (`npx eas-cli@latest login`, `npx eas-cli@latest build:configure`) and review `eas.json`. Use EAS-managed signing credentials where appropriate; do not expose credentials in Git or logs.
   配置 EAS（`npx eas-cli@latest login`、`npx eas-cli@latest build:configure`）并检查 `eas.json`。可酌情使用 EAS 托管签名凭据；不得在 Git 或日志中暴露凭据。
3. Build and test production-profile binaries on real iOS and Android devices:
   在真实 iOS 和 Android 设备上构建并测试 production profile：
   `npx eas-cli@latest build --platform ios --profile production`
   `npx eas-cli@latest build --platform android --profile production`
4. Submit builds only after user approval and after confirming store accounts/credentials:
   仅在用户批准且确认商店账号/凭据后提交构建：
   `npx eas-cli@latest submit --platform ios --profile production`
   `npx eas-cli@latest submit --platform android --profile production`
5. Explain that Apple Developer Program and Google Play Console accounts are needed for public store distribution. Store listings, screenshots, privacy disclosures, data-safety/age-rating forms, export-compliance questions, and human review are separate requirements. Uploading does not itself release an app publicly.
   说明公开发布需要 Apple Developer Program 和 Google Play Console 开发者账号。商店介绍、截图、隐私披露、数据安全/年龄分级表单、出口合规问卷和人工审核都需要单独完成。上传构建包并不等于 App 已公开上架。
6. OTA updates (EAS Update) are only for compatible JS/assets changes. Native code, native dependency, or runtime changes require a new binary. Never use OTA to bypass mandatory app-store review.
   OTA（EAS Update）仅用于兼容的 JS/资源更新。原生代码、原生依赖或 runtime 变化需要重新构建二进制包。不得通过 OTA 绕过必须进行的应用商店审核。

Reference official docs: [EAS Build](https://docs.expo.dev/build/introduction/), [EAS Submit](https://docs.expo.dev/submit/introduction/), [iOS deployment](https://docs.expo.dev/submit/ios/), [Android deployment](https://docs.expo.dev/submit/android/).

官方文档：[EAS Build](https://docs.expo.dev/build/introduction/)、[EAS Submit](https://docs.expo.dev/submit/introduction/)、[iOS 发布](https://docs.expo.dev/submit/ios/)、[Android 发布](https://docs.expo.dev/submit/android/)。
