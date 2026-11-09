<div class="header-banner dawn">
     <div class="header-label dawn">Dialogic 2 简介</div>
</div>

> [!IMPORTANT]
> 本文档面向 Dialogic 2。
> Dialogic 2 仍处于开发中，并被标记为 Alpha，工作流程可能会出现破坏性变动！


## 📜 目录
[toc]

## 什么是 Dialogic 2？

Dialogic 插件让你可以借助 Godot 轻松创建**游戏内对话**，或完整的**视觉小说**。

我们先来总览一下 Dialogic 2 能为你做些什么！

### 1. 时间线！

时间线代表你的对话。它是一系列事件（例如文本、条件、动画、信号等）的列表，并控制你故事的流程。你可以在对写作者友好的**可视化编辑器**、我们高效易用的**文本编辑器**，或你喜欢的任意文本编辑器中编辑它们。

**可视化编辑器**把每个事件呈现为一个可移动的块，并带有易于使用的字段：
![Dialogic 2：可视化模式下的时间线](media/introduction/visual_timeline.png)

**文本编辑器**允许你以纯文本格式编写事件。它提供语法高亮与自动补全，使编写时间线极为高效。不过，你也可以使用任意文本编辑器。下面是用文本格式表示的同一个时间线：

```dtl
join Juliet left
Juliet: 哦 Romeo，你最喜欢哪种游戏类型？
join Romeo right
Romeo: 哦 Juliet，这很简单……
- 视觉小说
	jump VNs
- JRPG
	jump other_timeline/
```

没有任何束缚；你可以随心所欲地混用它们！

### 2. 事件

每条时间线都包含事件，Dialogic 为你准备了许多内置事件，让你今天就能开始讲故事！而当它们不够用时，你也可以轻松创建更多。

![Dialogic 2：内置时间线事件](media/introduction/events.png)

### 3. 源于设计的可定制性

找不到合适的事件？需要为角色立绘提供更多选项？想让设计师能更简单地调整游戏主题？

这些都已就绪，等你取用。Dialogic 2 对写作者而言易于上手，对程序员而言强大而又直观，便于扩展。

只需极少代码就能[创建你自己的专属事件](creating-extensions.md)，而你的设计师可以立刻像使用 Dialogic 自带功能一样使用它们。

### 4. 角色系统与编辑器

一整套用于创建和管理角色的系统，Dialogic 内置了对应的编辑器。
无需编写任何程序；只要创建角色，就能在时间线中使用它们。

### 还有更多！

我们才刚刚触及可能性的皮毛。\
以下是你还可以做的其他一些事情的简短列表：

- [在代码中创建事件与时间线！](creating-timelines-in-code)

- 使用内置的「文本操作」，例如 `[speed]`、`[pause]`、`[signal]`、`[portrait]`。

- [在时间线中轻松使用变量！](variables.md)

- 为词语创建术语表，让玩家将鼠标悬停其上即可查看其定义。

- [借助集成的 CSV 文件系统翻译角色与时间线](translation.md)！

- [无缝创建自定义事件](creating-extensions.md)；它们全都是脚本！（`.gd`）。

- 在代码中调用功能，例如 `"Dialogic.Portraits.change_portrait(Emilio, "happy")`、`Dialogic.Backgrounds.update_background("res://icon.png")`

## 我该如何开始？

首先，[让我们把 Dialogic 安装并运行起来](getting-started)！完成这一步后，左侧导航栏上的众多页面将助你踏上对话驱动的游戏之旅！

需要更多细节？试试搜索，或在 GitHub 仓库上提交 issue！

---

## 我能从 Dialogic 1 升级到 Dialogic 2 吗？

从 Dialogic 1 升级时会遇到若干问题。

首先，Dialogic 1 仅适用于 Godot 3.x，而 Dialogic 2 需要 Godot 4.5 或更高版本。
因此，你的项目必须首先能在该 Godot 版本下工作，这是一个改动很多的、截然不同的引擎。

如果你的游戏已经取得了大量进展，那没有升级的理由。我们建议你继续留在 Godot 3 与 Dialogic 1 上。
