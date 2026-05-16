# Progress

最后更新时间：2026-05-16

## 当前目标

为当前 Tauri v2 项目补齐本机构建环境，并验证以下平台的打包链路：

- macOS
- Linux（通过 GitHub Actions）
- Windows（通过 GitHub Actions）
- Android
- iOS（无签名）

## 已完成

### 项目与 CI

- 已初始化 Tauri v2 项目骨架
- 已配置跨平台 GitHub Actions 工作流：
  - [release.yml](/Users/aaa/Downloads/dev/lessbuild/.github/workflows/release.yml)
- 已补充本地构建脚本：
  - [package.json](/Users/aaa/Downloads/dev/lessbuild/package.json)
- 已更新项目说明：
  - [README.md](/Users/aaa/Downloads/dev/lessbuild/README.md)

### 本机环境

- 已安装 Xcode，当前版本：
  - `Xcode 26.0.1`
- 已安装 Rust：
  - `rustc 1.95.0`
  - `cargo 1.95.0`
  - `rustup 1.29.0`
- 已安装 Java：
  - `openjdk@17`
- 已安装 CocoaPods
- 已安装 Android command line tools
- 已安装：
  - `xcodegen`
  - `libimobiledevice`

### Rust targets

以下 targets 已安装：

- `aarch64-apple-darwin`
- `x86_64-apple-darwin`
- `aarch64-apple-ios`
- `aarch64-apple-ios-sim`
- `x86_64-apple-ios`
- `aarch64-linux-android`
- `armv7-linux-androideabi`
- `i686-linux-android`
- `x86_64-linux-android`

### Shell 环境变量

以下环境已写入 [~/.zshrc](/Users/aaa/.zshrc:1)：

- Rust:
  - `source "$HOME/.cargo/env"`
- Java:
  - `JAVA_HOME=/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home`
- Android:
  - `ANDROID_HOME=/opt/homebrew/share/android-commandlinetools`
  - `ANDROID_SDK_ROOT=$ANDROID_HOME`
  - `NDK_HOME=$ANDROID_HOME/ndk/27.0.12077973`
  - `ANDROID_NDK_HOME=$NDK_HOME`

## 已验证

### 前端

- `npm run build` 已通过

### Tauri / Apple

- `npx tauri info` 可正常识别：
  - Xcode
  - Rust toolchain
  - Node / npm
  - Tauri CLI
- `npm run tauri:ios:init` 已成功
- 已生成 Xcode 工程：
  - [lessbuild.xcodeproj](/Users/aaa/Downloads/dev/lessbuild/src-tauri/gen/apple/lessbuild.xcodeproj)

## 当前未完成

### Android

Android 相关大包仍在后台下载中：

- `platform-tools`
- `platforms;android-35`
- `build-tools;35.0.0`
- `ndk;27.0.12077973`

当前后台进程：

- `sdkmanager --install platform-tools platforms;android-35 build-tools;35.0.0 ndk;27.0.12077973`

### iOS

iOS 构建仍差 Xcode 平台内容下载完成。

当前后台进程：

- `xcodebuild -downloadPlatform iOS`

已知现象：

- `npm run tauri:ios:build` 当前返回：
  - `iOS platform not installed`

这通常意味着 Xcode 的 iOS platform/runtime 内容尚未完全安装完成。

## 下次继续时先做什么

先开新终端并加载环境：

```bash
source ~/.zshrc
```

先检查后台下载是否结束：

```bash
pgrep -fl 'sdkmanager|xcodebuild -downloadPlatform' || true
```

如果后台下载都结束了，按下面顺序继续：

```bash
cd /Users/aaa/Downloads/dev/lessbuild
source ~/.zshrc

npm run tauri:android:init
npm run tauri:android:build

npm run tauri:ios:build
```

## 如果 Android 仍有问题

重新检查 SDK 包：

```bash
source ~/.zshrc
sdkmanager --list_installed
```

必要时重装：

```bash
source ~/.zshrc
sdkmanager --install "platform-tools" "platforms;android-35" "build-tools;35.0.0" "ndk;27.0.12077973"
```

## 如果 iOS 仍有问题

优先检查：

```bash
xcodebuild -showsdks
xcrun --sdk iphoneos --show-sdk-path
```

必要时继续执行：

```bash
xcodebuild -downloadPlatform iOS
```

## 关键文件

- [PROGRESS.md](/Users/aaa/Downloads/dev/lessbuild/PROGRESS.md)
- [README.md](/Users/aaa/Downloads/dev/lessbuild/README.md)
- [package.json](/Users/aaa/Downloads/dev/lessbuild/package.json)
- [release.yml](/Users/aaa/Downloads/dev/lessbuild/.github/workflows/release.yml)
- [tauri.conf.json](/Users/aaa/Downloads/dev/lessbuild/src-tauri/tauri.conf.json)
