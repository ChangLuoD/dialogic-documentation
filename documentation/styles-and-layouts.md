<div class="header-banner dawn">
     <div class="header-label dawn">样式与布局</div>
</div>

*本页介绍 Dialogic 如何使用样式、布局和 Dialogic 节点在你的游戏中显示时间线。*

## 📜 目录
[toc]

## 1. 简介

Dialogic 的一个重要方面是它如何让你显示时间线。为实现这一点，有多个系统协同工作：Dialogic 节点、布局场景和样式。

**Dialogic 节点**是可以放置在任何场景中的节点。这些节点会被 Dialogic 激活，并做各种截然不同的事情，从显示文本、标签或选项，到播放音效或定义一个可点击区域。

**布局场景**是同时包含 Dialogic 节点、其他 Godot Control 节点以及自定义脚本的场景，提供一个独立的 UI 部分。例如，有 `VisualNovel Textbox Scene`、`Glossary Popup Scene` 或 `Centered Choices Scene`。

**Dialogic 样式**是一种资源，它保存一组布局场景的组合，并允许对它们进行设置。这是修改 Dialogic 外观最简单的方式，因为样式可以在 Dialogic 的样式编辑器中编辑。

## 2. 使用样式

你可以在这里找到 Dialogic 样式的视频讲解：[Dialogic 2 - New Style System - YouTube](https://www.youtube.com/watch?v=TLnzSzqBwu4)

一个样式由以下部分组成：

- 一个 `Base` 布局场景（类型为 `DialogicLayoutBase`），以及该场景的设置。

- 一个 `Layer` 布局场景列表（类型为 `DialogicLayoutLayer`），以及每个图层的设置。

在样式编辑器中，你可以

- 添加新样式（从空白开始或基于预设样式）

- 重命名样式

添加、替换或移除图层（预设场景和自定义场景都可以）

- 为属于该样式的任意场景设置各种选项

- 将某个预设图层或整个样式设为自定义

- 基于另一个样式创建继承样式（继承其场景和默认设置）

- 更改默认样式

### 2.1 在游戏中使用样式

你可以通过 `Change Style` 事件、在调用 `Dialogic.start()` 之前调用 `Dialogic.Styles.load_style()`，或者给角色指定一个样式，来切换不同的样式。

## 3. 自定义样式与布局

你可以自定义 Dialogic 这个“显示”部分的所有环节。

### 3.1 自定义样式

最简单的方式是通过组合不同的图层并修改它们的设置来创建自定义样式。

### 3.2 自定义布局场景

下一步是把你布局的一部分（其中一个场景）自定义到超出所提供设置能达到的程度。

一个简单的做法是使用图层列表上方的“Make Custom”按钮，并选择“Current Layer”。
这会创建该图层场景的一份副本，你可以在 Godot 中编辑它。如果你总体上喜欢比如这个文本框，但想改动一些仅靠设置无法改动的东西，这就是你要做的。

> [!TIP]
> 在编辑已经设为自定义的布局时，要注意哪些子资源（例如脚本、字体、图片等）可能仍在 addons/dialogic 文件夹中。
> 如果你修改了它们，更新 Dialogic 时这些改动可能会丢失。
> 我建议在修改之前先把任何子资源设为“unique”。
> 不过，有些资源（例如场景的根脚本）在使用“Make Custom”时通常会自动设为自定义。

或者，你也可以从零开始创建一个自定义布局场景。

- 你的场景根节点必须挂载一个脚本，该脚本根据你的用途继承 `DialogicLayoutBase` 或 `DialogicLayoutLayer`。

- 在场景根脚本中，你可以导出变量，它们会作为设置项出现在 Dialogic 的样式编辑器中。用 @export_group 和 @export_subgroup 对它们分组会更美观。注意这里并非所有类型都受支持。

- 如果你想利用 Dialogic 的样式系统，那么你应该在一个名为 `_apply_export_overrides()` 的方法中应用所有 @export 出来的设置，该方法会在样式变化时被调用。

你可以通过图层列表上方的 `Add Layer` 或 `Replace Layer` 轻松添加你的自定义场景。

### 3.3 自定义节点

虽然 Dialogic 的模块通常期望显示它们的节点以某种特定方式构建，但这并不妨碍你真正去自定义这些节点本身。每个 Dialogic 节点只是一个普通的 control 节点，挂载了一个脚本。通常，Dialogic 节点通过把自己加入某个分组来与 Dialogic 子系统交互。

注意，在大多数情况下，响应某些 Dialogic 信号（或现有 Dialogic 节点的信号）要比创建一个 Dialogic 节点的自定义版本容易得多。
