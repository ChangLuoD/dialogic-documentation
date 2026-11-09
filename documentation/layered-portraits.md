<div class="header-banner pineapple">
     <div class="header-label pineapple">分层立绘</div>
</div>

*本页介绍如何创建和使用分层立绘场景。*

## 📜 目录

[toc]

## 1. 简介

如果你想要一个拥有多个图层的角色，比如身体、头和嘴，你可以使用自定义的**分层立绘**场景。该场景允许用许多 sprite 拼出一个角色，然后**在时间线中**显示、隐藏和切换这些 sprite！

如果你急着想了解更多关于 Dialogic 如何使用场景创建立绘，以及创建自定义立绘的要求，请参阅[自定义立绘](custom-portraits.md)页面。

## 2. 概览

首先，你将学习如何创建一个新的分层立绘场景并正确设置它。

最后，你将学习如何使用时间线控制立绘的各个图层。

## 2.1. 准备

我将使用一个名为 `Agustina` 的角色，如果你想跟着做，可以从 [https://dejinyucu.itch.io/agustina-visual-novel-sprite](https://dejinyucu.itch.io/agustina-visual-novel-sprite) 下载素材。

*提示：该角色以 PSD 文件形式提供；你需要自行挑选图层并将它们导出为图片文件。*

你还需要有一个角色。你可以使用已有的角色，或为此新建一个。在该角色上（在角色编辑器中）添加或选择一个立绘。

## 3. 创建基础分层立绘场景

在角色编辑器的“立绘设置”部分，前往 **Scene** 区域。

![立绘设置场景为空](media/layered_portrait/portrait_setting_scene_empty.jpg)

点击那个小箭头图标来更换立绘场景。

在弹出的窗口中，选择 `Layered Portrait`，然后点击 `Customize This Scene`。

![自定义立绘选择弹窗](media/layered_portrait/custom_portrait_selection_popup.jpg)

这会提示你将新场景保存到文件系统中的某个位置。导航到你的文件组织结构中合理的位置，并按需调整文件名。然后点击 `Save`。

现在你可以通过那个小箭头图标按钮（或从文件系统）打开这个场景了。

![立绘设置场景已设置](media/layered_portrait/portrait_setting_scene_set.jpg)

## 4. 填充场景

一开始，场景中只有少数几个节点供你起步：

![](media/layered_portrait/scene_empty.jpg)

你通常会有一个“Base”图层，它应该是我们添加的第一个 Sprite。所以我们选中第一个 Sprite，将其重命名为“Base”，然后把基础纹理拖到检查器的 `Texture` 属性中。

对于任何有“变体”的东西，比如这里的 Eyes、Eyebrows、Mouths 和 Clothes，你会想要一个“分组”，就像场景中已有的那个分组一样。分组之后可以让我们方便地在这些变体之间切换。

通过添加更多分组（Node2D）和 Sprite（Sprite2D）、正确命名它们（我们后面会用到这些名称）并设置纹理，我们会得到类似这样的东西：

![](media/layered_portrait/scene_final.jpg)

*注意：Augustina 示例中的 sprite 都占据同一个矩形区域。这意味着它们会自动正确地叠放在彼此之上。如果你的 sprite 覆盖角色不同的区域，你可能需要手动移动它们，使它们正确定位。*



保存场景并返回角色编辑器。如果一切顺利，你现在应该能在那里看到你的场景：

![](media/layered_portrait/finished_portrait_in_editor.jpg)

太棒了！

## 5. 从时间线控制图层

你可以从角色事件（在加入或更新时）控制这些图层。与立绘场景的所有通信都通过 `extra_data` 参数进行，例如这样：

```tml
join Agustina center [extra_data="set Mouth/Happy"]
[wait_input]
update Agustina [extra_data="set Eyes/Wink, set Mouth/Laugh, set Clothes/Outfit2"]
```

在可视化编辑器中，你可以在“Extra Data”字段中使用相同的命令：

![](media/layered_portrait/character_join_extra_data.jpg)

你也可以使用[文本特效](./text-effects.md#额外数据特效)来控制这些图层。

## 5.1. 图层命令语法

以下是你可以用在 `extra_data` 参数中的有效命令列表：

```tml
# 显示整个分组：
show Group1

# 显示一个图层，不隐藏其他图层：
show Group1/Layer1

# 隐藏一个分组：
hide Group1

# 隐藏一个图层：
hide Group1/Layer1

# 只显示一个图层并隐藏其他图层：
set Group1/Layer1
```

如果你希望在一个 `extra_data` 参数中使用多条命令，可以用逗号分隔：

```tml
update agustina [extra_data="show Glasses/Normal, set Emote/Shock"]
```

# 6. 常见问题

## 6.1. 我的立绘位置不对

分层立绘会计算所有图层的合并尺寸。如果你的图层尺寸/位置各不相同，请确保没有图层超出所需范围。试着启用所有图层并选中所有节点；你可以在场景的编辑器窗口中看到所有边界。

默认情况下，分层立绘脚本的 `fix_offset` 参数（在检查器的 Layered Portrait/Private/fix_offset 下）是开启的。它会基于第一个 sprite（假定它是基础 sprite）来调整位置。对许多立绘来说这更方便，但如果想要更精细的控制，你可以关掉它。在这种情况下，你的 sprite 应该被摆放成让角色左上角位于场景原点！

## 6.2 我必须把 `CanvasGroup` 用作根节点吗？

不必须，但它是处理过渡效果最好的选择。
