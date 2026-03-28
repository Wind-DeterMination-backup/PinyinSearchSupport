# src/main/java 层说明

## 这一层做什么
这一层存放所有 Java 业务代码，是模组功能实现的主体。Mindustry 通过 `mod.json` 中声明的入口类加载这里编译出来的类。

## 实现方式
代码按包名 `pinyinsearchsupport` 归档，而不是散放在默认包下。这保证了：
- 入口类、工具类、UI 补丁类命名空间清晰。
- 打包后 class 路径稳定。
- 后续若扩展更多功能，也能继续沿包结构拆分。

## 与其他层级的关系
- 它向上隶属 `src/main`。
- 它向下分为核心包 `pinyinsearchsupport/` 和更聚焦界面搜索补丁的 `ui/` 子包。
- 这里的代码会被编译到 `build/classes/java/main`，再进入 `build/release-stage` 与 `dist/`。
