# taro-study

两份 **Taro 3.6.22** 官方脚手架放在一起的学习记录。每个子项目只有一个页面，渲染 `Hello world!`，没有额外业务功能。

[![Taro](https://img.shields.io/badge/Taro-3.6.22-blue)](https://docs.taro.zone/)
[![React](https://img.shields.io/badge/React-18.2.0-149eca)](https://react.dev/)
[![React Native](https://img.shields.io/badge/React%20Native-0.70.14-61dafb)](https://reactnative.dev/)

仓库根目录没有 `package.json`。`taro-default-demo` 和 `taro-rn-demo` 各自带锁文件，需要分别安装、分别构建。两边的 Taro 配置日期都是 `2024-1-8`。

## 内容

| 目录 | 模板（`package.json` → `templateInfo`） | 页面实现 | 现有内容 |
| --- | --- | --- | --- |
| [`taro-default-demo`](taro-default-demo) | `default`：React、TypeScript、Sass | 函数组件，`useLoad` 时打印 `Page loaded.` | 单页、Jest 用例、多端构建脚本 |
| [`taro-rn-demo`](taro-rn-demo) | `react-native`：React、TypeScript、Sass | 类组件，生命周期方法为空 | 单页、Android / iOS 工程、模板自带的 GitHub Actions |

两个页面的导航栏标题都是「首页」。`src/app.config.ts` 里的窗口标题是 `WeChat`。`src/app.scss` 和 `src/pages/index/index.scss` 都是空文件。

`taro-default-demo` 的页面：

```tsx
export default function Index() {
  useLoad(() => {
    console.log('Page loaded.')
  })

  return (
    <View className='index'>
      <Text>Hello world!</Text>
    </View>
  )
}
```

`taro-rn-demo` 用类组件渲染同样的 `View` / `Text`。应用启动入口是 `index.js`，只引入 `@tarojs/rn-supporter/entry-file.js`。

子目录 [`taro-rn-demo/readme.md`](taro-rn-demo/readme.md) 是官方 React Native 模板自带的英文说明（安装 peer、打包、签名、Actions），不是针对本仓库改写过的文档。

## 技术栈

版本取自各目录的 `package.json` 与 `yarn.lock`。

### 两边都有

| 依赖 | taro-default-demo | taro-rn-demo |
| --- | --- | --- |
| `@tarojs/*` | `3.6.22` | `3.6.22` |
| React / React DOM | `^18.0.0`，锁定 **18.2.0** | `^18.1.0`，锁定 **18.2.0** |
| 框架 / 编译器 | React，Webpack 5 | React，Webpack 5 |
| Webpack | 固定 **5.78.0** | `^5.78.0`，锁定 **5.89.0** |
| TypeScript | `^5.1.0`，锁定 **5.3.3** | `^4.1.0`，锁定 **4.9.5** |
| 样式 | Sass（样式文件目前为空） | Sass（样式文件目前为空） |
| 包管理 | Yarn 1，`yarn.lock` | Yarn 1，`yarn.lock` |

`package.json` 的 `browserslist`：`last 3 versions`、`Android >= 4.1`、`ios >= 8`。

`taro-default-demo` 另有 Jest **29.7.0**（`^29.3.1`）和 `@tarojs/test-utils-react` `^0.1.1`。`taro-rn-demo` 没有测试脚本。

### taro-rn-demo 的原生栈

| 项 | 仓库中的值 |
| --- | --- |
| React Native | `^0.70.1`，锁定 **0.70.14** |
| Expo | `~47.0.3`，锁定 **47.0.14** |
| Android `applicationId` | `com.tarodemo`（`AndroidManifest` 的 package 同名） |
| Android 应用名 | `taroDemo`（`res/values/strings.xml`） |
| Android SDK | `minSdkVersion 21`，`compileSdkVersion` / `targetSdkVersion` **31** |
| Gradle | **7.5.1**（`android/gradle/wrapper`） |
| iOS 工程 / 显示名 | `taroDemo` |
| iOS 最低版本 | **13.0**（`ios/Podfile`） |
| iOS Bundle Identifier | `org.reactjs.native.example.taroDemo`（Xcode 工程里的模板值） |
| Hermes | `Podfile` 中 `:hermes_enabled => true` |

原生依赖还包括 `@react-native-async-storage/async-storage`、`react-native-gesture-handler`、`react-native-screens`、`expo-camera` 等，都列在 `taro-rn-demo/package.json`，页面代码没有调用它们。

## 目录结构

```text
.
├── taro-default-demo/                 # 默认模板
│   ├── config/                        # index.ts、dev.ts、prod.ts
│   ├── src/
│   │   ├── app.ts                     # useLaunch，打印 App launched.
│   │   ├── app.config.ts              # 注册 pages/index/index
│   │   ├── index.html                 # H5 模板，标题 taro-default-demo
│   │   └── pages/index/               # 唯一页面
│   ├── __tests__/index.test.js        # 快照测试（仓库未提交快照）
│   ├── project.config.json            # 微信项目，appid 为 touristappid
│   ├── project.tt.json                # 字节项目，appid 为 testAppId
│   ├── package.json
│   └── yarn.lock
└── taro-rn-demo/                      # React Native 模板
    ├── android/                       # Android 工程
    ├── ios/                           # iOS 工程 taroDemo
    ├── config/                        # 含 RN bundle 输出路径
    ├── src/                           # 与默认模板相同的单页结构
    ├── .github/workflows/             # 模板自带的 Android / iOS 打包流水线
    ├── index.js                       # React Native 入口
    ├── readme.md                      # 模板英文说明
    ├── project.config.json            # appid 为 touristappid
    ├── project.tt.json                # appid 为 touristappid
    ├── package.json
    └── yarn.lock
```

两个项目的 `config/index.ts` 都把 `sourceRoot` 设为 `src`，`outputRoot` 设为 `dist`，`designWidth` 为 `750`，`framework` 为 `react`，`compiler` 为 `webpack5`。持久化缓存 `cache.enable` 为 `false`。默认模板用 `defineConfig` 导出；RN 模板用 `module.exports` 导出。

RN 的 bundle 输出路径：

- iOS：`./ios/main.jsbundle`，source map `./ios/main.map`
- Android：`./android/app/src/main/assets/index.android.bundle`

## 环境要求

- **Node.js**：仓库没有 `engines` 字段。下面「已验证」的命令在 **Node.js 22.23.4** 上跑过。
- **Yarn Classic 1.x**。本环境是 **Yarn 1.22.22**。两个子项目都提交了 `yarn.lock`，安装时使用 `yarn install --frozen-lockfile`。
- 预览小程序需要对应平台的开发者工具。微信 `project.config.json` 的 `appid` 是模板占位 `touristappid`。
- 运行 Android 需要 Android SDK。运行 iOS 需要 macOS、Xcode 和 CocoaPods。
- `.env.dev`、`.env.prod`、`.env.test` 里只有被注释掉的 `TARO_APP_ID` 示例，没有启用的变量。

## 安装与运行

两个目录都会把产物写到自己的 `dist/`。换一个端再构建，会覆盖上一次的 `dist/`。根目录 `.gitignore` 已忽略 `dist/`、`node_modules/`、`.temp/`、`.rn_temp/`。

### taro-default-demo

```bash
cd taro-default-demo
yarn install --frozen-lockfile
```

H5 生产构建，产物在 `dist/`（含 `index.html` 与带 hash 的 js/css）：

```bash
yarn build:h5
```

监听构建：

```bash
yarn dev:h5
```

微信小程序构建。用微信开发者工具打开生成的 `dist/`（`project.config.json` 的 `miniprogramRoot` 也是 `./dist`）：

```bash
yarn build:weapp
yarn dev:weapp
```

`package.json` 里还有这些模板脚本，本环境没有逐个编译：

| 平台 | 开发 | 构建 |
| --- | --- | --- |
| 百度小程序 | `yarn dev:swan` | `yarn build:swan` |
| 支付宝小程序 | `yarn dev:alipay` | `yarn build:alipay` |
| 抖音小程序 | `yarn dev:tt` | `yarn build:tt` |
| QQ 小程序 | `yarn dev:qq` | `yarn build:qq` |
| 京东小程序 | `yarn dev:jd` | `yarn build:jd` |
| 快应用 | `yarn dev:quickapp` | `yarn build:quickapp` |

`taro-default-demo` 的 `yarn build:rn` 会让 CLI 自动安装 React Native 相关包，并改写 `package.json` 和 `yarn.lock`，随后配置加载失败。这个目录没有 `android/`、`ios/`，也没有预先声明 `@tarojs/rn-runner`。React Native 使用下面的 `taro-rn-demo`。

测试：

```bash
yarn test
```

`__tests__/index.test.js` 会创建应用、触发 `pages/index/index` 的 `onShow`，再对 HTML 做 `toMatchSnapshot()`。仓库没有提交对应快照，因此这条命令目前会失败。测试运行时控制台会打印 `App launched.` 和 `Page loaded.`，渲染结果里包含 `Hello world!`。

### taro-rn-demo

```bash
cd taro-rn-demo
yarn install --frozen-lockfile
```

安装过程会打印若干 peer dependency 警告（例如 `react-native@0.70.14` 期望 `react@18.1.0`，锁文件解析为 `18.2.0`）。`yarn install --frozen-lockfile` 仍能完成。

H5 与小程序脚本和默认模板相同：

```bash
yarn build:h5
yarn dev:h5
yarn build:weapp
yarn dev:weapp
```

React Native 命令来自 `package.json` 和 [`readme.md`](taro-rn-demo/readme.md)：

```bash
yarn dev:rn          # taro build --type rn --watch --qr
yarn start           # react-native start
yarn android         # react-native run-android，需要 Android SDK
yarn ios             # react-native run-ios，需要 macOS / Xcode
yarn podInstall      # pod-install，新增或升级原生库之后，在 macOS 上执行
```

模板说明里还有：

```bash
yarn upgradePeerdeps
yarn build:rn --platform ios
yarn build:rn --platform android
```

`upgradePeerdeps` 会按 Taro `3.6.22` 重装 `@tarojs/taro-rn`、`@tarojs/components-rn`、`@tarojs/router-rn` 的 peer 依赖，并执行 `pod-install`。这些原生依赖已经写在当前 `package.json` 里，日常安装用 `yarn install` 即可。

打包签名、Fastlane 和 App Store / APK 发布步骤以 [`taro-rn-demo/readme.md`](taro-rn-demo/readme.md) 为准。其中的 Team ID、证书和 keystore 口令仍是模板占位。

### GitHub Actions

流水线只在 `taro-rn-demo/.github/workflows/`：

| 文件 | 触发（文件中的配置） |
| --- | --- |
| `assemble_android_debug.yml` / `assemble_android_release.yml` | 推送 `master` 或 `v*` tag、针对 `master` 的 PR、手动 |
| `assemble_ios_debug.yml` / `assemble_ios_release.yml` | 同上 |
| `taro_playground_bundle_release.yml` | `b*` tag、手动 |

这些 workflow 按「仓库根目录就是这个 RN 工程」来写：在检出目录执行 `yarn`，并缓存 `${{ github.workspace }}/node_modules`。本仓库的 Git 根目录是 `taro-study`，RN 工程在子目录里，因此这些文件不能直接在当前仓库结构下跑通。本环境没有触发 GitHub Actions。

## 本次核对过的命令

环境：Linux、Node.js 22.23.4、Yarn 1.22.22。没有 Android SDK、`adb`、Xcode 或 CocoaPods。

| 命令 | 结果 |
| --- | --- |
| `taro-default-demo`：`yarn install --frozen-lockfile` | 成功 |
| `taro-default-demo`：`yarn build:h5` | 成功，约 9s。有入口体积超过 244 KiB 的警告（约 423 KiB），退出码 0。产物 `dist/index.html` 可被静态服务返回 200 |
| `taro-default-demo`：`yarn build:weapp` | 成功。`dist/pages/index/index.js` 中含 `Hello world!` 与导航标题「首页」 |
| `taro-default-demo`：`yarn test` | 失败。缺少快照文件，见上文 |
| `taro-default-demo`：`yarn build:rn` | 失败。CLI 改写依赖后无法加载配置。文档核对时产生的改动已还原，请不要在该目录执行此命令 |
| `taro-rn-demo`：`yarn install --frozen-lockfile` | 成功，伴随 peer dependency 警告 |
| `taro-rn-demo`：`yarn build:h5` | 成功，Webpack 5.89.0，同样有入口体积警告 |

未在本环境执行：`yarn dev:*` 常驻监听、各小程序开发者工具导入、`yarn android`、`yarn ios`、`yarn podInstall`、`yarn upgradePeerdeps`、`yarn build:rn --platform ios|android`、GitHub Actions。Headless Chrome 打开 H5 产物时进程挂起，因此没有完成浏览器里的页面渲染确认；页面文案以源码和微信端编译产物为准。

## 备注

仓库里的 PNG 是 Android 启动图标和 iOS App Icon，没有界面截图。仓库根目录没有 `LICENSE`。

## 参考

- [Taro 文档](https://docs.taro.zone/docs/)
- [Taro React Native](https://docs.taro.zone/docs/react-native)
- [NervJS/taro](https://github.com/NervJS/taro)
- 本仓库 RN 模板说明：[taro-rn-demo/readme.md](taro-rn-demo/readme.md)
