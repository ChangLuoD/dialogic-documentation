<div class="header-banner blurple">
     <div class="header-label blurple">快速开始</div>
</div>

*本教程将教你如何安装 Dialogic，并快速概览让对话在你的游戏中出现所必须了解的关键要素。*

## 📜 目录
[toc]

## 1. 安装与启用

Dialogic 2 需要 Godot 4.5 或更高版本。如果你不清楚自己安装的是哪个版本，可以查看 Godot 编辑器底部。

我们来安装 Dialogic 2：

- 在 [GitHub Dialogic Releases](https://github.com/dialogic-godot/dialogic/releases) 下载你所需 Dialogic 版本的 ZIP 文件。
- 从这个 ZIP 文件中解压出 `addons` 文件夹，Dialogic 就在其中。
- 将 `addons` 文件夹移动到你的 Dialogic 项目文件夹中。

现在，我们来确认你是否正确安装了 Dialogic：

- 你拥有这个文件夹路径 `res://addons/dialogic`
- 前往 `Project > Project Settings`
- 点击 `Plugins` 标签页。
- 勾选 Dialogic 旁边的 `enabled` 按钮。
- 重启 Godot

## 🚧 安装最新的 WIP 版本（main 分支）

Dialogic 一直在持续演进。很多时候，bug 修复或新功能可能只在 main 分支上提供，在 alpha 与 beta 阶段尤其如此。如果你有兴趣使用 main 开发分支，这很容易做到。
前往[仓库的主页](https://github.com/dialogic-godot/dialogic)，点击绿色的 `Code` 按钮，然后按下 `Download ZIP` 按钮。之后，你就可以像安装发行版一样安装该 ZIP 中的内容。

---

## 2. 认识编辑器

现在，你可以点击最上方的新标签页（位于 2D、3D、Script、Game 和 AssetLib 旁边）来打开 Dialogic 界面。

![godot_main_tabs](media/godot_main_tabs.png)

迎接你的是 Dialogic 的主界面。顶部可以看到 Dialogic 提供的各个编辑器，左侧则是一个侧边栏，其中会列出最近使用的角色与时间线，你也可以在这里创建新的时间线与角色。

<img title="" src="media/editor_overview.png" alt="" width="600">

---

## 3. 创建时间线

要创建时间线，请按侧边栏中的 `Add Timeline` 按钮。你需要选择一个文件夹来存放时间线，并输入文件名。时间线会保存为 `*.dtl` 文件。

点击 `Save` 后，你就可以从编辑器右侧的面板开始添加事件了。
<img src="/media/timeline_editor.png" width="600"/>

*提示：右键点击事件并选择 `Documentation`，可以了解更多关于每个事件及其设置的信息。*

如果你愿意，也可以点击右上角的 `Text Editor` 按钮切换到文本编辑器。你可以在这里进一步了解如何以文本格式编写时间线：[以文本格式编写时间线](timeline-text-syntax.md)。

你可以点击右上角的 `Play Timeline` 按钮来测试时间线。

---

## 4. 创建角色

要创建新角色，请按侧边栏中的 `Add Character` 按钮。

与时间线一样，你需要选择保存位置并输入名称。

> [!NOTE]
> 这个名称（不含 `.dch`）就是你在时间线中使用该角色时会保存的内容，也是你引用该角色时必须书写的内容（例如，如果文件名为 `Emilio.dch`，则写作 `Emilio: This is a text event.`）。所以请务必给它取个好名字。
> 实际在游戏中显示的名称可以通过 `display_name` 属性修改。


角色创建完成后，你会看到角色编辑器。该编辑器有四个主要区域。
<img src="/media/character_editor.png" width="600"/>

你可以在本教程中进一步了解立绘、自定义立绘以及角色设置：[角色与立绘](characters-and-portraits.md)

---

## 5. 在游戏中加入对话

最后一个重要步骤，是真正让对话在你的游戏中显示出来。

为此，我们需要两件事：

- A) 拥有能够显示我们时间线的节点
- B) 启动时间线的执行。

幸运的是，Dialogic 提供了一个同时完成这两件事的方法：`Dialogic.start(timeline_name_or_path)`

*因此，在按下某个输入时启动对话的代码可以是这样：*

```gdscript
func _input(event: InputEvent):
    # 检查是否已有对话正在运行
    if Dialogic.current_timeline != null:
        return

    if event is InputEventKey and event.keycode == KEY_ENTER and event.pressed:
        Dialogic.start('chapterA')
        get_viewport().set_input_as_handled()
```

## 你已经入门了！

恭喜！你现在已经了解了 Dialogic 的基础知识。不过还有更多内容要学。以下是你接下来可能想探索的教程列表：

- [以文本格式编写时间线](timeline-text-syntax.md)
- [角色与立绘](characters-and-portraits.md)
- [在代码中创建时间线](creating-timelines-in-code.md)
- [样式与布局](styles-and-layouts.md)
