---
layout: post
title: "Reactnative 项目开发（最佳？）实践"
date: 2025-08-14
tags: ["React Native", "我的技术写作成长之路", "Android", "React.js", "前端", "HarmonyOS"]
---
## 目标

- 使用[react-native-boilerplate](https://thecodingmachine.github.io/react-native-boilerplate/docs/installation)快速搭建脚手架- 接入图片上传组件[@baronha/react-native-multiple-image-picker](https://nitrogenzlab.github.io/react-native-multiple-image-picker/getting-started)- 模拟器调试- 真机调试- 代码调试、网络调试- 发布 TestFlight- app图标更换- 启动页面设置
## 搭建脚手架

### 环境要求

RN 最新版已经是`0.81`，但截止到现在2025/08/14最流行的依然是`0.78`，并不是最新的就好，自己学习和工作建议还是选择`0.78`，因为目前 100% 的第三方组件库依然是使用 `0.78`，使用大家都在使用的版本，可以减少很多问题。

至于 ReactJS 的版本，0.78 支持 react 19，请参考[博客](https://reactnative.dev/blog/2025/02/19/react-native-0.78)

Nodejs 版本官方要求是 18+，建议使用 LTS 版本，我使用的是 20，使用 nvm 管理版本。

使用 yarn 1.22 进行依赖安装。

![](/assets/images/7538282563052634121-1.jpg)

建议使用 Mac 电脑，可以开发 IOS 和 Android，否则就必须绕一些弯路才能开发 IOS。

### 搭建脚手架

如果想快速开发，建议使用模板来创建功能，自我学习就从官方的文档手搓脚手架，但生产讲究快速迭代，手搓是禁止的，会徒增许多工作量。

建议使用[react-native-boilerplate](https://github.com/thecodingmachine/react-native-boilerplate)来创建工程，按照[官方文档](https://thecodingmachine.github.io/react-native-boilerplate/docs/installation)来创建，大部分业务碰到的问题，它都涉及到了。

创建项目

```shell
npx @react-native-community/cli@latest init 应用名称 --template @thecodingmachine/react-native-boilerplate
```

按步骤来操作，建议启用 Typescript

```shell
📘 Using typescript ? (Y/n)
```

创建完成后，使用 vscode 打开项目

![](/assets/images/7538282563052634121-2.jpg)

打开 package.json，和其他的 node 项目一样，配置了常用的 scripts。

- 在根目录运行 yarn install 安装依赖。- 运行 yarn pod-install 安装CocoaPods依赖（仅 IOS）- 运行 yarn android启动 安卓 应用- 运行 yarn ios 启动 ios 应用
## 苹果开发者账号（Mac 电脑独享）

假设你已经有了苹果开发者账号，并在 xcode 中登录。

使用 XCode 打开项目下的的`.xcworkspace`后缀的文件，注意项目下还有一个`.xcodeproj`也是可以打开的，但这个文件打开后，是没办法运行项目的，报很多错。

所以一定要正确的打开`.xcworkspace`，双击它或者在 Xcode 的菜单中打开它。

按照图示打开项目后，分别按步骤1、2、3、4 打开签名设置。

![](/assets/images/7538282563052634121-3.jpg)

在第 5 步的位置选择你的开发团队。

第 5、6 是设置开发环境的配置、7、8 是发布环境的配置。

特别注意，这里是 ReactNative 项目，因为 Bundle Identifier 中使用到了变量， 在这里修改，变量会发生变化，虽然没有什么问题，但避免在不同 xcode 版本中出现问题，建议在`/项目名称/ios/项目名称/Info.plist`中使用纯文本的方式修改。

![](/assets/images/7538282563052634121-4.jpg)

变化前
`com.chenbitao.reactjs.native.example.${PRODUCT-NAME-rfc1034identifier}`
变化后
`com.chenbitao.reactjs.native.example.--PRODUCT-NAME-rfc1034identifier`

![](/assets/images/7538282563052634121-5.jpg)

当有效证书配置，前面的步骤`yarn install`、`yarn pod-install`都不报错，这时候可以编译运行到模拟器或真机了。

### 添加 IOS 模拟器

Xcode 中默认只会显示最新版的模拟器，但我们要兼容到 ios15，所以必须要手动添加模拟器，这期间要求网络连接优秀，下载要好几个 G 呢。中断了要重新来，所以不要用手机热点，使用正常的网络连接可以。

![](/assets/images/7538282563052634121-6.jpg)

![](/assets/images/7538282563052634121-7.jpg)

![](/assets/images/7538282563052634121-8.jpg)

![](/assets/images/7538282563052634121-9.jpg)

### 显示模拟器

但添加了模拟器，xcode 中并没有显示我需要的手机和系统版本，需要手动设置一下模拟器使用显示。

![](/assets/images/7538282563052634121-10.jpg)

![](/assets/images/7538282563052634121-11.jpg)

设置完成后，就可以在 Xcode 上选择运行模拟器了，这里选择了什么，会强制安装到这个模拟器上，并不是以你打开的为准。

而在 reactnative 项目根目录使用`yarn ios`则是为默认打开你打开的模拟器里，但我们依然要使用 Xcode 来打开项目，来解决一些编译问题和 ios 上的开发、设置（Launch 页面、应用图标）、检查编译产物是否正确。

开发项目

```shell
% yarn ios
yarn run v1.22.22
$ react-native run-ios
info Found Xcode workspace "MyReactnativeApp.xcworkspace"
info Found booted iPhone 13 Pro Max
info Building (using "xcodebuild -workspace MyReactnativeApp.xcworkspace -configuration Debug -scheme MyReactnativeApp -destination id=EBF8C13B-EB53-4DCB-9E4C-C0C6F53042CD")
⠇ Building the app......
```

![](/assets/images/7538282563052634121-12.jpg)

![](/assets/images/7538282563052634121-13.jpg)

![](/assets/images/7538282563052634121-14.jpg)

安卓项目

安卓项目对环境更为苛刻，需要魔法上网或者配置ADB、Android Studio、镜像地址下载安卓依赖、gradle 等，这里不展开。

使用 `yarn andorid` 运行安卓项目

![](/assets/images/7538282563052634121-15.jpg)

![](/assets/images/7538282563052634121-16.jpg)

![](/assets/images/7538282563052634121-17.jpg)

这时候项目基本成型，记得使用 git 将代码提交上去，后续的操作都基于这套代码上镜像。

## 技术选型

所有的 RN 第三方组件都在 [ReactNative目录](https://reactnative.directory/)中搜索，根据更新时间、维护热度、使用热度和功能来选择，这里要关键词`image`搜索。

但也不是所有的都在这里收录了，也可以在 github上搜索依赖。

本次选择`@baronha/react-native-multiple-image-picker`来开发上传功能，选择它的原因是使用它的时候遇到了一些典型的问题，在使用其他的组件时或多或少会碰到一些问题，可以通过这些问题和解决方法来练习解决问题的能力。

安装依赖

按照[官方文档](https://nitrogenzlab.github.io/react-native-multiple-image-picker/getting-started)安装依赖

```bash
% yarn add @baronha/react-native-multiple-image-picker
yarn add v1.22.22
[1/5] 🔍  Validating package.json...
[2/5] 🔍  Resolving packages...
[3/5] 🚚  Fetching packages...
warning eslint-plugin-testing-library@7.1.1: The engine "pnpm" appears to be invalid.
[4/5] 🔗  Linking dependencies...
warning "@react-native/eslint-config > @typescript-eslint/eslint-plugin@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > @typescript-eslint/parser@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > eslint-plugin-ft-flow@2.0.3" has incorrect peer dependency "eslint@^8.1.0".
warning "@react-native/eslint-config > eslint-plugin-jest@27.9.0" has incorrect peer dependency "eslint@^7.0.0 || ^8.0.0".
warning "@react-native/eslint-config > eslint-plugin-react-hooks@4.6.2" has incorrect peer dependency "eslint@^3.0.0 || ^4.0.0 || ^5.0.0 || ^6.0.0 || ^7.0.0 || ^8.0.0-0".
warning "@react-native/eslint-config > eslint-plugin-react-native@4.1.0" has incorrect peer dependency "eslint@^3.17.0 || ^4 || ^5 || ^6 || ^7 || ^8".
warning "@react-native/eslint-config > @typescript-eslint/eslint-plugin > @typescript-eslint/type-utils@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > @typescript-eslint/eslint-plugin > @typescript-eslint/utils@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > eslint-plugin-jest > @typescript-eslint/utils@5.62.0" has incorrect peer dependency "eslint@^6.0.0 || ^7.0.0 || ^8.0.0".
warning " > reactotron-react-native-mmkv@0.2.8" has unmet peer dependency "reactotron-core-client@*".
warning " > reactotron-react-query@1.0.4" has unmet peer dependency "react-query@^3.39.2".
warning " > reactotron-react-query@1.0.4" has unmet peer dependency "reactotron-core-client@^2.9.3".
[5/5] 🔨  Building fresh packages...
success Saved lockfile.
success Saved 1 new dependency.
info Direct dependencies
└─ @baronha/react-native-multiple-image-picker@2.2.4
info All dependencies
└─ @baronha/react-native-multiple-image-picker@2.2.4
✨  Done in 5.51s.
```

安装`react-native-nitro-modules`

```bash
% yarn add -D react-native-nitro-modules
yarn add v1.22.22
[1/5] 🔍  Validating package.json...
[2/5] 🔍  Resolving packages...
[3/5] 🚚  Fetching packages...
warning eslint-plugin-testing-library@7.1.1: The engine "pnpm" appears to be invalid.
[4/5] 🔗  Linking dependencies...
warning "@react-native/eslint-config > @typescript-eslint/eslint-plugin@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > @typescript-eslint/parser@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > eslint-plugin-ft-flow@2.0.3" has incorrect peer dependency "eslint@^8.1.0".
warning "@react-native/eslint-config > eslint-plugin-jest@27.9.0" has incorrect peer dependency "eslint@^7.0.0 || ^8.0.0".
warning "@react-native/eslint-config > eslint-plugin-react-hooks@4.6.2" has incorrect peer dependency "eslint@^3.0.0 || ^4.0.0 || ^5.0.0 || ^6.0.0 || ^7.0.0 || ^8.0.0-0".
warning "@react-native/eslint-config > eslint-plugin-react-native@4.1.0" has incorrect peer dependency "eslint@^3.17.0 || ^4 || ^5 || ^6 || ^7 || ^8".
warning "@react-native/eslint-config > @typescript-eslint/eslint-plugin > @typescript-eslint/type-utils@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > @typescript-eslint/eslint-plugin > @typescript-eslint/utils@7.18.0" has incorrect peer dependency "eslint@^8.56.0".
warning "@react-native/eslint-config > eslint-plugin-jest > @typescript-eslint/utils@5.62.0" has incorrect peer dependency "eslint@^6.0.0 || ^7.0.0 || ^8.0.0".
warning " > reactotron-react-native-mmkv@0.2.8" has unmet peer dependency "reactotron-core-client@*".
warning " > reactotron-react-query@1.0.4" has unmet peer dependency "react-query@^3.39.2".
warning " > reactotron-react-query@1.0.4" has unmet peer dependency "reactotron-core-client@^2.9.3".
[5/5] 🔨  Building fresh packages...
success Saved lockfile.
success Saved 1 new dependency.
info Direct dependencies
└─ react-native-nitro-modules@0.27.6
info All dependencies
└─ react-native-nitro-modules@0.27.6
✨  Done in 3.35s.
```

为保险起见，再次执行在`yarn install`

```bash
% yarn
yarn install v1.22.22
[1/5] 🔍  Validating package.json...
[2/5] 🔍  Resolving packages...
[3/5] 🚚  Fetching packages...
[4/5] 🔗  Linking dependencies...
warning " > @react-navigation/stack@7.4.7" has unmet peer dependency "react-native-screens@>= 4.0.0".
[5/5] 🔨  Building fresh packages...
success Saved lockfile.
$ patch-package
patch-package 8.0.0
Applying patches...
@baronha/react-native-multiple-image-picker@2.2.4 ✔
✨  Done in 8.34s.
```

每次增加依赖，都需要运行一下package.json 中的脚本`yarn pod-install`。

```typescript
% yarn pod-install                      
yarn run v1.22.22
$ npx pod-install
🔍️ Scanning for pods...
1.16.2
> pod install
[NitroModules] 🔥 MultipleImagePicker is boosted by nitro!
[react-native-mmkv] Thank you for using react-native-mmkv ❤️
[react-native-mmkv] If you enjoy using react-native-mmkv, please consider sponsoring this project: https://github.com/sponsors/mrousavy
[NitroModules] 🔥 Your app is boosted by nitro modules!
[NitroModules] Found react-native 0.78.2 (78) in /Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native!
Found 9 modules for target `MyReactnativeApp`
link_native_modules! {ios_packages: [{configurations: [], name: "@baronha/react-native-multiple-image-picker", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/@baronha/react-native-multiple-image-picker", path: "../node_modules/@baronha/react-native-multiple-image-picker", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/@baronha/react-native-multiple-image-picker/MultipleImagePicker.podspec", script_phases: []}, {configurations: [], name: "@react-native-masked-view/masked-view", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/@react-native-masked-view/masked-view", path: "../node_modules/@react-native-masked-view/masked-view", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/@react-native-masked-view/masked-view/RNCMaskedView.podspec", script_phases: []}, {configurations: [], name: "react-native-gesture-handler", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-gesture-handler", path: "../node_modules/react-native-gesture-handler", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-gesture-handler/RNGestureHandler.podspec", script_phases: []}, {configurations: [], name: "react-native-mmkv", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-mmkv", path: "../node_modules/react-native-mmkv", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-mmkv/react-native-mmkv.podspec", script_phases: []}, {configurations: [], name: "react-native-reanimated", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-reanimated", path: "../node_modules/react-native-reanimated", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-reanimated/RNReanimated.podspec", script_phases: []}, {configurations: [], name: "react-native-safe-area-context", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-safe-area-context", path: "../node_modules/react-native-safe-area-context", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-safe-area-context/react-native-safe-area-context.podspec", script_phases: []}, {configurations: [], name: "react-native-screens", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-screens", path: "../node_modules/react-native-screens", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-screens/RNScreens.podspec", script_phases: []}, {configurations: [], name: "react-native-svg", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-svg", path: "../node_modules/react-native-svg", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-svg/RNSVG.podspec", script_phases: []}, {configurations: [], name: "react-native-nitro-modules", root: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-nitro-modules", path: "../node_modules/react-native-nitro-modules", podspec_path: "/Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native-nitro-modules/NitroModules.podspec", script_phases: []}], ios_project_root_path: "/Users/pe7er/Desktop/MyReactnativeApp/ios", react_native_path: "../node_modules/react-native"}
[NitroModules] 🔥 MultipleImagePicker is boosted by nitro!
[react-native-mmkv] Thank you for using react-native-mmkv ❤️
[react-native-mmkv] If you enjoy using react-native-mmkv, please consider sponsoring this project: https://github.com/sponsors/mrousavy
[NitroModules] 🔥 Your app is boosted by nitro modules!
[NitroModules] Found react-native 0.78.2 (78) in /Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native!
Auto-linking React Native modules for target `MyReactnativeApp`: MultipleImagePicker, NitroModules, RNCMaskedView, RNGestureHandler, RNReanimated, RNSVG, RNScreens, react-native-mmkv, and react-native-safe-area-context
Framework build type is static library
[Codegen] warn: using experimental new codegen integration
[Codegen] Adding script_phases to ReactCodegen.
[Codegen] Generating ./build/generated/ios/ReactCodegen.podspec.json
[Codegen] Analyzing /Users/pe7er/Desktop/MyReactnativeApp/package.json
[Codegen] Searching for codegen-enabled libraries in the app.
[Codegen] The "codegenConfig" field is not defined in package.json. Assuming there is nothing to generate at the app level.
[Codegen] Searching for codegen-enabled libraries in the project dependencies.
[Codegen] Found react-native
[Codegen] Found react-native-gesture-handler
[Codegen] Found react-native-mmkv
[Codegen] Found react-native-reanimated
[Codegen] Found react-native-safe-area-context
[Codegen] Found react-native-screens
[Codegen] Found react-native-svg
[Codegen] Found react-native-nitro-modules
[Codegen] Searching for codegen-enabled libraries in react-native.config.js
[Codegen] Processing FBReactNativeSpec
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Processing rncore
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Processing rngesturehandler_codegen
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Supported Apple platforms: ios, macos, tvos, visionos for rngesturehandler_codegen
[Codegen] Processing RNMmkvSpec
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Supported Apple platforms: ios, macos, tvos for RNMmkvSpec
[Codegen] Processing rnreanimated
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Supported Apple platforms: ios, macos, tvos, visionos for rnreanimated
[Codegen] Processing safeareacontext
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Supported Apple platforms: ios, macos, tvos, visionos for safeareacontext
[Codegen] Processing rnscreens
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Supported Apple platforms: ios, tvos, visionos for rnscreens
[Codegen] Processing rnsvg
[Codegen] Searching for podspec in the project dependencies.
[Codegen] Supported Apple platforms: ios, macos, tvos, visionos for rnsvg
[Codegen] Processing NitroModulesSpec
[Codegen] Searching for podspec in the project dependencies.
[Codegen] [Codegen - FBReactNativeSpec] Skipping iOS code generation for FBReactNativeSpec as it has been generated already.
[Codegen] [Codegen - rncore] Skipping iOS code generation for rncore as it has been generated already.
[Codegen] Generating Native Code for rngesturehandler_codegen - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating Native Code for RNMmkvSpec - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating Native Code for rnreanimated - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating Native Code for safeareacontext - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating Native Code for rnscreens - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating Native Code for rnsvg - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating Native Code for NitroModulesSpec - ios
[Codegen] Generated artifacts: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios
[Codegen] Generating RCTThirdPartyComponentsProvider.h
[Codegen] Generated artifact: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios/RCTThirdPartyComponentsProvider.h
[Codegen] Generating RCTThirdPartyComponentsProvider.mm
[Codegen] Generated artifact: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios/RCTThirdPartyComponentsProvider.mm
[Codegen] Generating RCTAppDependencyProvider
[Codegen] Generated artifact: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios/RCTAppDependencyProvider.h
[Codegen] Generated artifact: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios/RCTAppDependencyProvider.mm
[Codegen] Generated podspec: /Users/pe7er/Desktop/MyReactnativeApp/ios/build/generated/ios/ReactAppDependencyProvider.podspec
[Codegen] Done.
Analyzing dependencies
[NitroModules] 🔥 MultipleImagePicker is boosted by nitro!
[NitroModules] 🔥 Your app is boosted by nitro modules!
[NitroModules] Found react-native 0.78.2 (78) in /Users/pe7er/Desktop/MyReactnativeApp/node_modules/react-native!
[react-native-mmkv] Thank you for using react-native-mmkv ❤️
[react-native-mmkv] If you enjoy using react-native-mmkv, please consider sponsoring this project: https://github.com/sponsors/mrousavy
Downloading dependencies
Installing HXPhotoPicker (4.2.4)
Installing Kingfisher (8.5.0)
Installing MultipleImagePicker (2.2.4)
Installing NitroModules (0.27.6)
Generating Pods project
Setting USE_HERMES build settings
Setting REACT_NATIVE_PATH build settings
Setting SWIFT_ACTIVE_COMPILATION_CONDITIONS build settings
[Ccache]: Removing Ccache from CC, LD, CXX & LDPLUSPLUS build settings
[SPM] Cleaning old SPM dependencies from Pods project
[SPM] Adding SPM dependencies to Pods project
[Privacy Manifest Aggregation] Appending aggregated reasons to existing PrivacyInfo.xcprivacy file.
[Privacy Manifest Aggregation] Reading .xcprivacy files to aggregate all used Required Reason APIs.
Setting CLANG_CXX_LANGUAGE_STANDARD to c++20 on /Users/pe7er/Desktop/MyReactnativeApp/ios/MyReactnativeApp.xcodeproj
Pod install took 9 [s] to run
Integrating client project
Pod installation complete! There are 77 dependencies from the Podfile and 78 total pods installed.
✨  Done in 14.81s.
```

ios 需要在`Info.plist`中添加权限声明

```xml
<key>NSCameraUsageDescription</key>
	<string>$(PRODUCT_NAME) 需要访问您的相机</string>
	<key>NSLocationWhenInUseUsageDescription</key>
	<string></string>
	<key>NSMicrophoneUsageDescription</key>
	<string>$(PRODUCT_NAME) 需要访问您的麦克风以便您录制音频</string>
	<key>NSPhotoLibraryAddUsageDescription</key>
	<string>$(PRODUCT_NAME) 需要照片库权限</string>
	<key>NSPhotoLibraryUsageDescription</key>
	<string>$(PRODUCT_NAME) 需要访问您的照片库</string>
```

![](/assets/images/7538282563052634121-18.jpg)

添加组件`src/components/medias/MultipleImagePicker.tsx`

```tsx
import type { PickerResult } from '@baronha/react-native-multiple-image-picker';

import { Config, openPicker } from '@baronha/react-native-multiple-image-picker';
import { useState } from 'react';
import { Button, Image, ScrollView, StyleSheet, View } from 'react-native';

function MultipleImagePicker() {
  const [selectedImages, setSelectedImages] = useState<PickerResult[]>([]);

  const handleImagePicker = async () => {
    const config: Config = {
      backgroundDark: '#2f2f2f',
      isHiddenOriginalButton: false,
      language: 'system',
      maxSelect: 10,
      maxVideo: 10,
      mediaType: 'all',
      numberOfColumn: 4,
      primaryColor: '#FB9300',
      selectBoxStyle: 'number',
      selectMode: 'multiple',
      theme: 'dark',
    };

    try {
      const response = await openPicker(config);
      setSelectedImages(response);
    } catch (error) {
      console.warn('Image picker error:', error);
    }
  };
  const selectCameraHandle = () => {
    void handleImagePicker();
  }
{% raw %}
  return (
    <View style={styles.container}>
      <Button onPress={selectCameraHandle} title="选择图片" />
      <ScrollView horizontal style={styles.imageContainer}>
        {selectedImages.map((image) => (
          <Image
            key={image.localIdentifier}
            resizeMode="cover"
            source={{ uri: image.path }}
            style={styles.image}
          />
        ))}
      </ScrollView>
    </View>
  );
}

export default MultipleImagePicker;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  image: {
    borderRadius: 8,
    height: 100,
    marginRight: 8,
    width: 100,
  },
  imageContainer: {
    marginTop: 16,
  },
});
{% endraw %}
```

在 Example.tsx 中添加引用

```tsx
import { useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { Alert, ScrollView, Text, TouchableOpacity, View } from 'react-native';

import { useI18n, useUser } from '@/hooks';
import { useTheme } from '@/theme';

import { AssetByVariant, IconByVariant, Skeleton } from '@/components/atoms';
import MultipleImagePicker from '@/components/medias/MultipleImagePicker';
import { SafeScreen } from '@/components/templates';

const MAX_RANDOM_ID = 9;

function Example() {
  // 其他代码
  return (
    <SafeScreen
      isError={fetchOneUserQuery.isError}
      onResetError={() => {
        handleResetError();
      }}
    >
      <ScrollView>
        
        <View style={[gutters.marginTop_40]}>
          <MultipleImagePicker />
        </View>
        
      </ScrollView>
    </SafeScreen>
  );
}

export default Example;
```

此时执行`yarn ios`会报错，报错信息的最后面如下

```text
error Failed to build ios project. "xcodebuild" exited with error code '65'. To debug build logs further, consider building your app with Xcode.app, by opening 'MyReactnativeApp.xcworkspace'.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

可以增加`yarn ios --verbose`来查看更详细的信息，但我建议用 xcode 编译一下看看具体日志

![](/assets/images/7538282563052634121-19.jpg)

这里除了快捷键，可以通过1、2 来进行编译，可以通过 3、4 来清楚编译。

可以点击错误信息查看具体日志

![](/assets/images/7538282563052634121-20.jpg)

是因为“Kingfisher 8.3.2 升级后报 ‘underlying Objective-C module 'Kingfisher' not found’ 的错误，是因为新版 Kingfisher 的模块导入方式或兼容性发生了变化，而我们项目使用了 CocoaPods 的 modular headers 模式。这可能导致 Kingfisher 的 Objective-C 部分无法被 Swift 正确识别和导入，从而编译失败。

github 上有人已经提出并解决了这个问题，这样 Xcode 就不生成 Swift 分发接口文件了。这样就不会因为找不到 Kingfisher 的 Objective-C 模块而编译失败。这个方法适用于现在我们只在本地用 Kingfisher，不需要给其他 Swift 项目分发二进制库。”

![](/assets/images/7538282563052634121-21.jpg)

参考链接[github.com/onevcat/Kin…](https://github.com/onevcat/Kingfisher/issues/2379)

按照参考链接来修改Podfile

```javascript
# Resolve react_native_pods.rb with node to allow for hoisting
require Pod::Executable.execute_command('node', ['-p',
  'require.resolve(
    "react-native/scripts/react_native_pods.rb",
    {paths: [process.argv[1]]},
  )', __dir__]).strip

platform :ios, min_ios_version_supported
prepare_react_native_project!

linkage = ENV['USE_FRAMEWORKS']
if linkage != nil
  Pod::UI.puts "Configuring Pod with #{linkage}ally linked Frameworks".green
  use_frameworks! :linkage => linkage.to_sym
end

target 'MyReactnativeApp' do
  config = use_native_modules!

  use_react_native!(
    :path => config[:reactNativePath],
    # An absolute path to your application root.
    :app_path => "#{Pod::Config.instance.installation_root}/.."
  )

  post_install do |installer|
    # https://github.com/facebook/react-native/blob/main/packages/react-native/scripts/react_native_pods.rb#L197-L202
    react_native_post_install(
      installer,
      config[:reactNativePath],
      :mac_catalyst_enabled => false,
      # :ccache_enabled => true
    )

    installer.pods_project.targets.each do |target|
      # https://github.com/onevcat/Kingfisher/issues/2379
      if target.name == 'Kingfisher'
        target.build_configurations.each do |config|
          config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'NO'
        end
      end
    end 
  end
end
```

![](/assets/images/7538282563052634121-22.jpg)

修改完成后，还需要再次执行`yarn pod-install`是 Podfile 应用生效。

再次执行`yarn ios`来尝试启动项目。ios可以正常获取权限和加载图片了。

![](/assets/images/7538282563052634121-23.jpg)

再次尝试`yarn android`来启动安卓项目。

![](/assets/images/7538282563052634121-24.jpg)

此时安卓和 ios 虚拟机都安装了 app，可以关掉 metro 的终端，单独使用`yarn start`启动react-native 开发，当修改 react 代码时，安卓和 ios 虚拟机可以同时更新，同时开发调试安卓和 IOS 了。

## 项目脚手架介绍

[react-native-boilerplate](https://thecodingmachine.github.io/react-native-boilerplate/docs/installation)是一个非常优秀的 ReactNative 脚手架，它归纳了常用商业项目的基础功能

- 环境变量- 路由- 国际化- 导航- 网络请求- 主题- 存储- 调试 [reactotron/](https://docs.infinite.red/reactotron/)- 组件：图片资源加载、字体、骨架屏、错误处理、安全屏幕
![](/assets/images/7538282563052634121-25.jpg)

大部分内容在官方文档中都有介绍，下面基于开发过程中的理解，分享一些功能和技巧。

### 环境变量

脚手架支持.env 来添加环境变量，但是环境变量修改需要重新打包 app，并且因为缓存、网络等原因，使用起来并不方便，所以建议将环境变量通过代码写在代码中，但涉及安全的密钥等信息，建议通过接口获得，不要配置在代码和.env中。

### 不同环境使用不同图标

### 修改启动页面

ios的启动页面代码在`ios/MyReactnativeApp/LaunchScreen.storyboard`，可以通过文本方式修改，也可以在xcode 中修改。

双击文字可以对文字内容进行修改。

![](/assets/images/7538282563052634121-26.jpg)

图片修改，在图片修改界面将不同尺寸的图片拖对应的图片中。

![](/assets/images/7538282563052634121-27.jpg)

以上修改需要重新执行 `yarn ios`

安卓修改则在`android/app/src/main/res/layout/launch_screen.xml`、`android/app/src/main/res/drawable/splash_screen.xml`中修改，也可以通过文本方式修改，也可以通过 Android Studio 进行修改。

![](/assets/images/7538282563052634121-28.jpg)

### app图标更换

ios 替换 app 图标

![](/assets/images/7538282563052634121-29.jpg)

安卓替换 app 图标

![](/assets/images/7538282563052634121-30.jpg)

## 代码调试

代码调试使用官方的调试工具，没有什么可以分享的。

## 网络调试

网络调试请使用(reactotron)[[docs.infinite.red/reactotron/…](https://docs.infinite.red/reactotron/%5D%EF%BC%8C%E5%8F%AF%E4%BB%A5%E5%BE%88%E6%96%B9%E4%BE%BF%E7%9A%84%E8%B0%83%E8%AF%95%E5%AE%89%E5%8D%93%E5%92%8C) ios 的网络请求、存储、状态、日志、在编辑器中打开。

安装软件后，需要在 metro 终端使用 R 刷新，底部可以切换监控的设备。

![](/assets/images/7538282563052634121-31.jpg)

## 增加全局 types，减少类型引入

Reactnative 是支持 ts 的，为了避免导出出现`import type { IResponseBody } from '@/request'`这种情况，可以在 src 或更目录下新建一个`types` 或`typings`文件夹，在里面存放所有的 `d.ts` 文件

![](/assets/images/7538282563052634121-32.jpg)
request.d.ts

```typescript
/**
 * 请求响应体
 */
type IResponseBody<T> = {
  data: T;
}
```

`tsconfig.json`中添加 `typeRoots`,注意如果显式的添加了 typeRoots，就必须要显式的将 `./node_modules/@types` 也添加进来。

![](/assets/images/7538282563052634121-33.jpg)

tsconfig.json

```json
{
  "extends": "@react-native/typescript-config/tsconfig.json",
  "compilerOptions": {
    "typeRoots": [
      "./src/types",
      "./node_modules/@types"
    ],
    "types": ["react-native", "jest", "node"],
    "paths": {
      "@/*": ["./src/*"]
    },
    "noUnusedLocals": true
  },
  "include": [
    "__mocks__/**/*",
    "./src/**/*",
    "index.d.ts",
    "*.mjs",
    ".*.mjs",
    "*.js",
    ".*.js",
    "*.ts",
     ".prettierrc.mjs"
],
  "exclude": ["node_modules", "_", "android", "ios"]
}
```

配置完成后，重启 VSCode 或者 ts 服务，就可以愉快的无需导入使用 ts 了。

![](/assets/images/7538282563052634121-34.jpg)

```typescript
import { instance } from '@/services/instance';

import { User, userSchema } from './schema';

export const UserServices = {
  fetchOne: async (id: number) => {
    const response = await instance.get(`users/${id}`).json();
    return userSchema.parse(response.data);
  },
};
```

这里只是简单做一个示范，建议根据类型使用命名空间。

![](/assets/images/7538282563052634121-35.jpg)
request.d.ts

```typescript
/**
 * 应用请求
 */
namespace AppRequest {
  /**
   * 请求响应体
   */
  export type IResponseBody = {
    data: T;
  };
}

![image.png](/assets/images/7538282563052634121-1.jpg)
```

## TODO

- 真机调试- 不同环境使用不同 app
- 发布 TestFlight
