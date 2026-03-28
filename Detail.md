# 仓库总览

## 这一层做什么
这一层是整个 `PinyinSearchSupport` 模组的总入口，负责把源码、资源、构建脚本、发布说明和 CI 流程放在同一个可维护单元里。它描述的不是单一功能点，而是“这个模组如何被开发、构建、发布、加载”的全链路。

从可见文件看，根层同时承担了四种职责：项目元数据定义、构建流程编排、开发者说明、发布产物落点。`build.gradle` 决定 Gradle 怎么编译和打包，`mod.json` 决定 Mindustry 识别这个模组时看到什么，`README.md` 和 `RELEASE_NOTES.md` 面向人类读者解释功能与版本变化，`src/` 才是功能实现本体。

## 实现方式
这个项目是标准 Gradle Java 工程，但打包目标不是普通桌面程序，而是 Mindustry Java 模组。`build.gradle` 除了常规 `compileJava`，还额外定义了 `zipMod`、`jarMod`、`jarAndroid`、`jarMerged`、`zipMerged`、`deploy` 等任务，把同一份业务代码分别组装成桌面包、Android 包和合并包。

源码被放在 `src/main/java`，运行时文案放在 `src/main/resources/bundles`。Gradle 编译后把 class 放进 `build/classes`，把资源复制到 `build/resources`，把最终归档产物放到 `build/libs` 和 `dist/`。因此根层不仅是“源代码所在位置”，也是“从源码到发布包的映射起点”。

## 与其他层级的关系
- `.github/` 负责自动发布，是根层构建规则在 GitHub Actions 上的远程执行版本。
- `src/` 是权威源，所有 `build/`、`dist/` 内容都由这里和根层脚本派生。
- `.gradle/` 保存本机 Gradle 状态，不定义功能，只记录构建工具执行过程。
- `build/` 是中间产物与阶段性结果目录，`dist/` 是更接近“给用户分发”的最终输出目录。

## 关键文件
- `build.gradle`：定义依赖、Java 8 兼容性、D8 查找逻辑、各种打包与复制任务。
- `mod.json`：定义模组名、版本、入口类、最小游戏版本等给 Mindustry 读取的元数据。
- `README.md`：面向用户的功能说明和构建命令。
- `RELEASE_NOTES.md`：手写变更记录，用来补充 GitHub 自动生成的 release note。
- `LICENSE`：GPLv3，约束源代码和二进制分发方式。

## 边界说明
本轮文档没有进入 `.git/` 内部对象库逐层写说明。`.git` 保存的是 Git 仓库数据库与引用，不属于模组本身的实现层级，把对象哈希目录逐个文档化不会增加对项目逻辑的理解。
