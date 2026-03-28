# src/main 层说明

## 这一层做什么
这一层对应 Gradle 的主源码集 `main`。它表示“应用运行时真正要打进产物里的内容”，与测试、样例、工具脚本等非运行时内容区分开。

## 实现方式
目录下分成两个标准子层：
- `java/`：编译成 `.class`，最终进入 jar/zip/classes.dex。
- `resources/`：原样或近原样复制到产物中，供运行时通过 bundle 或资源加载逻辑读取。

## 与其他层级的关系
- 它是 `src/` 的主实现分支。
- `build/classes/java/main` 和 `build/resources/main` 分别是这一层在编译期和资源处理期的镜像结果。
