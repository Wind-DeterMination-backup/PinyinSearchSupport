# ui 子包说明

## 这一层做什么
这一层专门处理 Mindustry 界面树上的搜索框与结果列表，是“把拼音匹配能力接入现有 UI”的落地层。核心问题不是拼音怎么转，而是如何在不改游戏源码的前提下，找到正确的 `TextField`、列表容器、文本来源，并安全地替换过滤结果。

## 实现方式
目录下三类职责分离得很清楚：
- `SearchFieldPatcher`：扫描场景树，识别需要接管的搜索框，注册变更监听，做延迟触发和目标缓存。
- `SearchTarget`：根据搜索框反向定位对应 `ScrollPane` 和 `Table`，快照原始元素，再按拼音匹配结果重建列表或网格。
- `SearchTextExtractor`：从 tooltip、`Label` 或复合 `Group` 中提取可搜索文本，避免补丁逻辑直接耦合到每一种 UI 元素类型。

## 与其他层级的关系
- 它依赖上层 `PinyinSupport` 提供匹配结果，不自行实现拼音算法。
- 它被 `PinyinSearchSupportMod` 实例化并周期性调用。
- 它操作的对象来自 Mindustry/Arc 的运行时场景树，因此属于“对外部 UI 结构做非侵入式适配”的边缘层。
