# pinyinsearchsupport 包说明

## 这一层做什么
这一层是模组核心包，负责把“拼音匹配能力”“模组生命周期”“GitHub 更新检查”三件事情组织起来。它不直接操纵具体的列表格子布局，而是提供上层规则和跨 UI 的共用能力。

## 实现方式
当前有三个关键类：
- `PinyinSearchSupportMod`：模组入口，监听 `ClientLoadEvent`，注册设置项，并周期性调用搜索框补丁器。
- `PinyinSupport`：纯匹配与转换逻辑，负责识别输入是否像拼音、规范化查询、把中文转换为拼音、应用模糊规则、做缓存。
- `GithubUpdateCheck`：异步访问 GitHub Releases 或 `mod.json`，比较版本，弹窗展示更新信息，并支持直接下载、导入模组、重启游戏。

这里能看出明显的分层意识：入口类负责接线，算法类负责字符串处理，网络/UI 复合功能则单独隔离在更新检查类中。

## 与其他层级的关系
- 它依赖 `ui/` 包完成真正的搜索框补丁。
- 它被 `mod.json` 的 `main` 字段直接引用，因此是运行时最先被 Mindustry 触达的代码层。
- 它读取 `resources/bundles` 中的设置文案键，并通过 Mindustry/Arc API 显示给用户。
