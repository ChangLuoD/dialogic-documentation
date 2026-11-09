<div class="header-banner blurple">
     <div class="header-label blurple">用代码创建时间线</div>
</div>

*由于时间线和事件都只是资源，你可以在代码中创建新的时间线。*

## 📜 目录
[toc]

# 如何通过代码创建时间线？

创建一个空时间线看起来像这样：

```gdscript
var timeline : DialogicTimeline = DialogicTimeline.new()
Dialogic.start_timeline(timeline)
```

当然，这里创建的时间线还不包含事件。要添加事件，你可以做两件事：

- 创建一个**事件资源**数组：

```gdscript
var events : Array = []
var text_event = DialogicTextEvent.new()
text_event.text = "Hey, this was made in code!"
text_event.character = load("res://characters/Emilio.dch")
events.append(text_event)

var timeline : DialogicTimeline = DialogicTimeline.new()
timeline.events = events
# 如果你的事件已经是资源，需要加上这一行：
timeline.events_processed = true
Dialogic.start(timeline)
```

- 创建一个 **String** 数组：

```gdscript
var events : Array = """
Jowan (Surprised): Wow this is interesting!
- Yes
    [background arg="res://icon.png"]
    [wait seconds="1"]
    set MyAutoload.exitement += 20
- No
    set MyAutload.exitement -= 10 """.split('\n')

var timeline : DialogicTimeline = DialogicTimeline.new()
timeline.events = events
Dialogic.start(timeline)
```

*在这种情况下，你必须遵循[本教程](timeline-text-syntax.md)中说明的语法。*

如果你希望在代码中动态创建角色资源，可以这样做：
- 在代码中创建角色资源

```gdscript
var speaker:String = "Fred"
var speaker_resource:DialogicCharacter = DialogicCharacter.new()
speaker_resource.display_name = speaker
speaker_resource.color = Color.WHITE
speaker_resource.set_identifier(speaker)
text_event.character = speaker_resource
```
