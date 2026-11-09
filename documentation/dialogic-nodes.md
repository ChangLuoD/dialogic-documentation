<div class="header-banner tropical">
     <div class="header-label tropical">Dialogic 节点</div>

</div>

*Dialogic 节点是 Dialogic 提供的自定义节点，负责在游戏中呈现你的时间线。*

## 📜 目录
[toc]

## 1. 简介

通常，这意味着 Dialogic 会添加一个由 `DialogicNodes` 构成的**布局场景**。这就是你调用 `Dialogic.start()` 时发生的情况，它可以在[样式编辑器](styles-and-layouts.md)中设置。也可以把 `DialogicNodes` 集成到你现有的 UI 中。

本主题与[样式与布局](styles-and-layouts.md)密切相关，如果你还没看过那一页，就去看一下吧。

你可以在我们的[节点类参考索引](node_class_index.md)中找到所有 Dialogic 节点。

---

## 2. 什么是 Dialogic 节点？

许多事件都需要相应的节点，才能实际影响游戏：

- **文本事件：** `DialogText`、`NameLabel`、`NextIndicator`、`TypingSound`

- **角色事件：** `PortraitContainer`

- **选项事件：** `ChoiceButton`、`ChoiceSound`

- **背景事件：** Background

- **文本输入事件：** `TextInput`

如果你想找到所有 Dialogic 节点，请查看[节点类参考索引](node_class_index.md)。

以下是一些关于 Dialogic 节点需要了解的事情：

- Dialogic 节点提供的功能差异很大，因此它们的属性也差异很大。把某个 Dialogic 节点添加到你的场景中并查看检查器，可以了解更多信息。

- 有些 Dialogic 节点会使用 Dialogic 设置页中的设置。
  一个通用的经验法则是：对所有 Dialogic 节点实例都应保持相同的设置，通常属于 Dialogic 设置；而可能因节点而变的东西，则是属性。

---

## 3. 使用 Dialogic 节点

你可以通过常规的 `Add Node` 窗口添加这些节点。

它们在树中的位置并不重要，因为它们会自动加入一个分组，并通过该分组被识别。

有些 Dialogic 节点需要特殊设置。例如，`TypingSound` 节点要求自己是 `DialogText` 节点的子节点。
