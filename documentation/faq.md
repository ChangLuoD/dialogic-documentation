<div class="header-banner blurple">
     <div class="header-label blurple">常见问题</div>
</div>

## 📜 目录
[toc]

## 我可以在自己的项目中使用 Dialogic 吗？

可以，你可以使用 Dialogic 制作任何类型的游戏——包括商业游戏！
本项目基于 [MIT 许可证](https://github.com/dialogic-godot/dialogic/blob/master/LICENSE)开发。我们唯一的请求是：请记得在你的项目中署名致谢！

如果你想出现在「Made with Dialogic」页面上，请通过 [Emilio 的 Discord 服务器](https://discord.gg/2hHQzkf2pX)联系我们！

## 我该如何更改输入？

Dialogic 使用一个 Godot `InputAction` 作为所谓的「Input Action」。默认情况下，它是启用插件时自动创建的 `dialogic_default_action`。你可以编辑这个操作，也可以在 Dialogic 设置页的文本区中更改设置，改用其他输入操作。

## 如何让对话在游戏中显示？

让对话在你的游戏中显示的最简单方式，是在游戏中调用 `Dialogic.start("res://path/to/timeline.dtl")`。这会同时创建一个[布局场景](styles-and-layouts.md)并启动时间线。

## 为什么我的立绘不显示？

使用视觉小说布局时，要让角色显示在屏幕上，你需要用角色事件（Join 模式）让他们加入当前场景。

## 为什么它没有成为 Godot 的一部分？

> 这个插件很酷！为什么它没有随 Godot 一起发布？

我看到很多人说这个插件应该随 Godot 一起发布，但我认为它应该保持为插件，因为大多数做游戏的人并不会用到它。很感谢你的评价，但它会继续保持为插件 :)

## 我该如何开始和停止背景音乐？

使用一个 `Music` 事件设置资源，然后用一个不设置资源的 `Music` 事件取消它。

![header_faq](./media/faq/background_music_toggling.png)

这个例子会在 4 秒内淡入音乐，然后在 5 秒内淡出。

## 如何找到当前说话者或其立绘索引？

下面的代码片段会给出当前说话者及其立绘信息，包括索引。

```gdscript
var current_speaker: DialogicCharacter = Dialogic.Text.get_current_speaker()
var portrait_info := Dialogic.Portraits.get_character_info(current_speaker)
var speaker_portrait_index := portrait_info.position_index
```

## 如何找到当前时间线的名称？

时间线没有名称，它们是纯文本。\
不过，你可以访问资源路径：
```gdscript
var timeline_path := Dialogic.current_timeline.resource_path
```

## 我看不到或无法点击我的按钮？
你可能有一些 UI 元素，希望它们显示在正在播放的对话之上，但它们无法被点击，或者被背景遮住了。
这种情况下，这些节点应该放在索引更高的 Canvas Layer 中！
在此了解 Canvas Layer 的用法：[Godot 文档：Canvas Layers](https://docs.godotengine.org/en/stable/tutorials/2d/canvas_layers.html)


## 我用来开始对话的输入操作与推进对话的输入操作冲突
很多人用某种「交互」键来开始对话，而这个键可能与 Dialogic 的 Input Action 相同（例如 Enter 或 X 等）。
这时就可能出现：每当你想推进对话，或在对话的最后一次输入时，又启动了一个新对话。
通过在代码中做简单检查就能轻松解决这些问题，例如：
```gdscript
func _input(event):
    if player_is_in_area and Input.is_action_pressed("start_interaction"):
         # 只有在当前没有对话激活时才启动新对话
         if Dialogic.current_timeline == null:
              Dialogic.start("ARelevantTimeline")
```

## 我修改了时间线文本，但游戏显示的仍是旧文本？

如果你启用了翻译，就需要更新 CSV。\
一旦你的时间线事件带上了翻译 ID，匹配的 CSV 行就会优先。\
建议在大部分文本完成之前先禁用翻译。

## 如何隐藏和显示文本框？

下面的代码可以让你检查文本框是否可见，并根据其状态执行操作。
```gdscript
if Dialogic.Text.is_textbox_visible():
    Dialogic.Text.hide_textbox()
else:
    Dialogic.Text.show_textbox()
```

## 我在开始对话时会遇到轻微卡顿或冻结！

预加载样式非常有用，可以用它的 `prepare` 方法。\
你可以在游戏的启动画面期间，对所有会用到的样式调用它。

```gdscript
var style: DialogicStyle = load("res://path/to/my/style.tres")
style.prepare()
```

除此之外，你还可以在游戏的加载阶段预加载一条空时间线。

最后，请注意 Godot 的着色器编译器是按需运行的；每当样式（或任何资源）中需要加载新的着色器时，它就会进行编译，从而导致卡顿。\
因此，如果你仍然遇到问题，建议提前编译这些着色器。

## 如何从对话过渡到游戏玩法？

当你的对话结束，而你想切换到其他上下文（例如游戏玩法）时，你需要决定如何处理当前的对话布局。在 `Settings/General` 中你会找到 `Layout Node Behaviour` 设置，它可以在 **Delete**、**Hide** 和 **Keep** 之间切换。

如果你打算使用在 Dialogic 之外管理的转场，可能需要使用 **Hide**，然后手动再次显示它，稍后再隐藏。例如：

```gdscript
func _on_dialogic_timeline_ended() -> void:
    var layout := Dialogic.Styles.get_layout_node()
    layout.show()
    # 执行我的转场
    await transtion_finished
    layout.hide()
```

或者你也可以在时间线内部触发转场，让时间线只在转场要求时才结束。那么你可以使用 **Hide** 或 **Delete** 模式。下面是一段示例场景转场代码（挂在自动加载上），然后从时间线中调用它：

```gdscript
func change_scene(scene_path: String):
    Transition.fade_out()
    await Transition.fade_out_completed
    get_tree().change_scene_to_file(scene_path)
```

![transition-call-event.jpg](./media/faq/transition-call-event.jpg)
