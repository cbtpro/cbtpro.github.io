---
layout: post
title: "React Native 多环境配置全攻略：环境变量、iOS Scheme 和 Android Build Variant"
date: 2026-02-26
tags: ["前端", "每天一个知识点", "Android", "React Native", "Xcode", "iOS"]
---
在 React Native 项目开发中，经常会遇到不同环境（开发、测试、生产）需要不同配置的需求。本文结合实践经验，详细讲解如何管理环境变量（`.env` 文件）、如何配置和使用 iOS 的 Scheme，以及 Android 的 Build Variant，帮助你构建灵活且高效的多环境应用。

![image.png](/assets/images/7610741353883320346-4.jpg)

## 环境变量管理 — 使用 .env 文件

### 为什么用 .env？

- 将敏感信息（API 地址、Key 等）和环境相关配置分离- 不同环境使用不同的配置，不必修改代码- 方便本地和 CI/CD 环境管理
### 工具选择

- dotenv：读取 .env 文件的标准方案- babel-plugin-inline-dotenv：打包时将环境变量注入代码
### 使用示例

1. 在项目根目录创建不同的环境配置文件，比如：
```
.env.development
.env.test
.env.production
```

2.  安装依赖：

```scss
yarn add dotenv babel-plugin-inline-dotenv --dev
```

3.  配置 Babel 插件 `babel.config.js`：

```arduino
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    ['inline-dotenv', {
      path: process.env.DOTENV_CONFIG_PATH || '.env'
    }]
  ]
};
```

4.  启动时指定环境配置文件：

```ini
DOTENV_CONFIG_PATH=.env.test yarn start
```

## iOS 多环境配置

### 什么是 Scheme 和 Configuration？

- Scheme：Xcode 的构建方案，管理运行和打包的流程- Configuration：对应不同的 Build Settings，比如 Debug、Release，通常和 Scheme 配合使用
### 新增 Scheme

- 在 Xcode 顶部菜单选择 Product > Scheme > Manage Schemes...- 点击 + 新建 Scheme，复制现有 Scheme，命名为 MyReactnativeTemplate-Dev、MyReactnativeTemplate-Test 等- 每个 Scheme 关联对应的 Build Configuration
![image.png](/assets/images/7610741353883320346-3.jpg)

### 3. 配置图片

![image.png](/assets/images/7610741353883320346-2.jpg)

- 打开项目的 Build Settings。打开项目的 Build Settings。- 搜索 ASSETCATALOG_COMPILER_APPICON_NAME，设置为对应环境的 AppIcon 名称（比如 Debug 配置使用 AppIcon-Debug，Release 使用默认的 AppIcon）。- 也可以在 Xcode Scheme 的 Build Configuration 里，针对不同 Scheme 设置不同的 Build Configuration，从而加载不同的 AppIcon。
![image.png](/assets/images/7610741353883320346-1.jpg)

~~### 3. 新增 `.xcconfig` 文件？

~~*   在 Xcode 新建配置文件（比如 `Dev.xcconfig`、`Test.xcconfig`）

- 配置各环境特有的变量- 在 Build Settings 中，将不同 Configuration 指向对应的 .xcconfig~~~~
### 在 React Native 中使用 Scheme 和环境变量

- 通过 react-native run-ios --scheme "MyReactnativeTemplate-Dev" 指定 Scheme- 使用 DOTENV_CONFIG_PATH=.env.test yarn ios --scheme "MyReactnativeTemplate-Dev" 来启动，确保使用正确的环境变量
### 关于 xcodebuild 命令

带空格的 Scheme 名称要用引号包裹：

```rust
xcodebuild -workspace MyReactnativeTemplate.xcworkspace -configuration Debug -scheme "MyReactnativeTemplate-Dev" -destination 'id=设备ID'
```

## Android 多环境配置

### 什么是 Build Variant 和 Flavor？

- Build Variant = Build Type + Flavor- 通过 Flavor 可以生成多个不同版本的 APK（比如开发版、测试版、生产版）
### 配置 build.gradle

在 `android/app/build.gradle` 中新增 flavor：

```bash
android {
    ...
    flavorDimensions "env"
    productFlavors {
        dev {
            dimension "env"
            applicationIdSuffix ".dev"
            versionNameSuffix "-dev"
        }
        test {
            dimension "env"
            applicationIdSuffix ".test"
            versionNameSuffix "-test"
        }
        prod {
            dimension "env"
        }
    }
}
```

### 环境变量管理

- 使用第三方库 react-native-config 管理 Android 端环境变量- 根据 flavor 加载不同的 .env 文件，比如 .env.dev、.env.test 等
### 构建和运行指定 Flavor

```ini
# 运行开发版
yarn android --variant=devDebug

# 运行测试版
yarn android --variant=testDebug

# 打包生产版
cd android && ./gradlew assembleProdRelease
```

## App 内集成开发者工具实现环境切换

在开发过程中，为了方便调试和验证不同环境，很多项目会在 App 里内置开发者工具，支持动态切换环境配置。常见实现有两种方式：

### 配置写在代码里，开发者工具里切换

- 将多环境配置（API 地址、Key 等）预先写在代码里（JSON 或常量）- 开发者工具页面展示环境选项（比如开发、测试、预发布、生产）- 用户选择后，App 内部切换到对应配置，刷新接口调用等
优点：实现简单，不依赖外部服务
缺点：配置发布需跟随 App 版本更新，不够灵活

### 从后台配置中心动态拉取环境配置（类似 Nacos）

- App 启动或打开开发者工具时，向配置中心请求最新环境配置列表和内容- 用户选择环境后，App 动态加载对应的配置（比如 API 地址）- 支持远程更新配置，无需重新发布 App
优点：配置管理集中，灵活性高
缺点：需要搭建并维护配置中心，增加复杂度

无论哪种方式，都能极大提升多环境调试效率。建议根据项目规模和需求选择合适方案。

## 总结与建议

- 环境变量：用 .env 文件和 dotenv 系列库统一管理，方便调试和打包- iOS：通过 Scheme + Configuration + .xcconfig 实现多环境，运行和打包时指定 Scheme 和环境变量- Android：利用 Build Flavor + Build Variant 配置多环境，结合 react-native-config 实现环境变量注入- 启动/打包：使用命令时指定 Scheme 或 Flavor，确保打包环境正确- App中增加开发者工具时，则需要统一配置不同环境的配置
## 参考资料

Native Environment Variables]([github.com/goatandshee…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fgoatandsheep%2Freact-native-dotenv))

- [Xcode Schemes and Configurations](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.apple.com%2Flibrary%2Farchive%2Ffeaturedarticles%2FXcodeConcepts%2FConcept-Schemes.html)- [Android Product Flavors](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.android.com%2Fstudio%2Fbuild%2Fbuild-variants)- [react-native-config](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fluggit%2Freact-native-config)
