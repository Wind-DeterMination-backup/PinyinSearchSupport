# workflows 层说明

## 这一层做什么
这一层存放 GitHub Actions 工作流定义，是仓库的持续集成与持续发布入口。对这个项目来说，它只有一个明确目标：当维护者打标签或手动触发时，自动把模组构建出来并发布为 GitHub Release 资产。

## 实现方式
`release.yml` 做了几件关键事情：
- 使用 `actions/checkout@v4` 取源码。
- 使用 `actions/setup-java@v4` 配置 JDK 17。这里用 17 只是为了跑 Gradle，本项目编译目标仍由 Gradle 强制成 Java 8 字节码。
- 使用 `android-actions/setup-android@v3` 和 `sdkmanager` 安装 `build-tools;34.0.0`，目的是给 `dexAndroid` 提供 `d8`。
- 执行 `clean deploy`，而 `deploy` 在根层脚本里依赖 `jarMerged` 和 `zipMerged`，因此工作流得到的是桌面与 Android 兼容的合并包。
- 若触发源是 `refs/tags/v*`，则用 `softprops/action-gh-release@v2` 把 `dist/*.jar` 与 `dist/*.zip` 上传为 release 资产。

## 与其他层级的关系
- 向上，它依赖 `.github/` 作为平台配置容器。
- 向下，它实际消费的是根层构建脚本、`src/` 源码和 `dist/` 产物。
- 对用户来说，这一层不影响游戏内行为，但决定用户能否拿到自动构建好的下载包。
