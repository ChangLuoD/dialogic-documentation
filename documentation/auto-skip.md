<div class="header-banner purple">
     <div class="header-label purple">自动跳过</div>

</div>

*自动跳过让你或玩家可以快速推进时间线。*

## 📜 目录
[toc]

## 1. 什么是自动跳过？

自动跳过指的是以快于阅读速度的节奏推进时间线。

在视觉小说中，自动跳过可以帮助玩家快速走完已经知道的剧情分支。
如果你的故事本就不打算让玩家反复游玩，这个功能对你可能没什么用。

---

## 2. 怎么用？

在 Dialogic 中，可以通过 Inputs 子系统访问 `DialogicAutoSkip` 对象，从代码里开启和修改自动跳过（例如 `Dialogic.Inputs.auto_skip`）。

这个功能最常见的用法，是给玩家提供一个自动跳过开关按钮。本节会教你怎么实现相应的逻辑。

首先，你可以在 Dialogic 的 Text 设置页面里设置每个事件允许占用的时间。

![header_saving_loading](media/auto_skip_settings.png)

不过，还有很多设置没有暴露在界面上！

---

## 3. 开启与修改自动跳过

自动跳过的所有设置都是 `DialogicAutoSkip` 类上的变量。该类可以通过 `Dialogic.Inputs.auto_skip` 访问。它有一堆有用的设置：

- `enabled: bool` *如果为 true，Dialogic 会自动跳过。*

**禁用条件**

如果这些设置处于开启状态，它们会让 Dialogic 在特定条件/特定时机自动关闭自动跳过。

- `disable_on_user_input: bool` *如果为 true（默认），当 Dialogic 的输入动作被触发时，Dialogic 会关闭自动跳过。*

- `disable_on_unread_text: bool` *如果为 true（默认），当遇到任何未读文字时，Dialogic 会关闭自动跳过。*

```info
`disable_on_unread_text` 需要开启 Dialogic 的历史记录功能。
```

**启用条件**

- `enable_on_seen: bool` *如果为 true（默认），每当遇到你之前读过的文本事件时，Dialogic 就会开启自动跳过。*

**常规设置**

- `skip_voice: bool` *如果为 true（默认），自动跳过会跳过语音事件，而不是播放它们。*

- `time_per_event: float` *定义每个事件的时间（等同于 Settings>Text 页面中的设置）。*

所以你可以这样开启自动跳过：

```gdscript
Dialogic.Inputs.auto_skip.enabled = !Dialogic.Inputs.auto_skip.enabled
```

你可以用它在时间线中快速前进，例如为了调试。为此你应当像下面这样关掉 `disable_on_unread_text` 设置：

```gdscript
Dialogic.Inputs.auto_skip.disable_on_unread_text = false
```

> [!TIP]
> 在你使用 `Play Timeline` 时已经实现了这一点。你可以在那里用鼠标中键开启自动跳过，并用任意用户输入停止它。


## 4. 响应自动跳过

### 4.1 信号

自动跳过被开启或关闭时，该类会发出
`DialogicAutoSkip.toggled` 信号。

下面是一个连接该信号的例子：

```gdscript
# 连接信号。
func _init():
    Dialogic.Inputs.auto_skip.toggled.connect(_on_auto_skip_toggled)

## 如果自动跳过被关闭，我们想停止计时器。
func _on_auto_skip_toggled(is_enabled: bool) -> void:
    # 在这里写你响应自动跳过变化的代码！
```

### 4.2 在自定义事件中

事件的行为多种多样：等待信号、立即执行、条件混合、播放音频……

不可能让自动跳过从外部优雅地处理所有这些效果。因此，自动跳过的行为必须由每个时间线事件自己实现。

#### 我的事件属于哪种类型？

你的事件会立即结束吗？它在播放音频吗？在等待信号吗？这些情况必须用不同的方式实现。

如果你在等待信号，可以用 `DialogicAutoSkip.toggled` 让你的事件响应自动跳过。
文本事件就是用这个信号来跳过文字动画，然后推进时间线。

如果你在等待计时器，或执行需要持续多帧的操作，可以用 `time_per_event` 变量限制事件可占用的时间。
下面这段代码可以给你一个思路：

```gdscript
var animation_length: float = 10.0

if Dialogic.Inputs.auto_skip.enabled:
    var time_per_event: float = Dialogic.Inputs.auto_skip.time_per_event
    animation_length = min(time_per_event, animation_length)
```

这段代码会在自动跳过开启时，把动画时长限制在设定的最大时间内。
