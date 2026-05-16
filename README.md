# lessbuild

一个基于 Tauri v2 的跨平台打包模板，内置 GitHub Actions，可为以下平台生成安装包或构建产物：

- Linux
- macOS
- Windows
- Android
- iOS（无签名）

## 已实现

- 桌面端矩阵构建：
  - `ubuntu-22.04`
  - `macos-latest`
  - `windows-latest`
- Android 自动初始化后打包：
  - `APK`
  - `AAB`
  - `split-per-abi`
- iOS 自动初始化后打包：
  - `--no-sign`
- 所有构建产物自动上传为 GitHub Actions artifacts

工作流文件：

- [release.yml](/Users/aaa/Downloads/dev/lessbuild/.github/workflows/release.yml)

## 触发方式

支持两种触发方式：

- 手动触发：GitHub Actions -> `Release`
- Tag 触发：推送 `v*` 标签，例如 `v0.1.0`

## 本地命令

```bash
npm install

# 桌面端
npm run tauri:desktop:build

# Android
npm run tauri:android:init
npm run tauri:android:build

# iOS（无签名）
npm run tauri:ios:init
npm run tauri:ios:build
```

## GitHub Actions 说明

### Desktop

桌面端使用 Tauri v2 官方 `tauri-action` 进行构建，并上传 workflow artifacts。

### Android

Android job 会在 CI 中完成：

- 安装 Java 17
- 安装 Android SDK / Build Tools / NDK
- 安装 Rust Android targets
- 执行 `tauri android init --ci`
- 构建 `APK` 和 `AAB`

### iOS

iOS job 会在 CI 中完成：

- 安装 Rust iOS targets
- 安装 CocoaPods
- 执行 `tauri ios init --ci`
- 执行 `tauri ios build --no-sign`

注意：

- 无签名 iOS 产物主要用于 CI 验证或后续二次签名。
- 未签名的 iOS 包通常不能直接分发到真实用户设备。

## 不需要的 Secrets

当前方案默认不做以下事情，因此不需要额外 secrets：

- iOS 签名
- macOS 签名 / notarization
- Windows 代码签名
- App Store / Play Store 自动发布

如果后续要补签名发布，可以在此基础上继续扩展。
