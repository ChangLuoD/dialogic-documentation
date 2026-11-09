<div class="header-banner pineapple">
     <div class="header-label pineapple">信号与信号事件</div>
</div>

*Dialogic 提供了大量信号，让你可以响应各种各样的状况。
如果你是 Godot 信号的新手，请[在这里学习如何使用它们](https://docs.godotengine.org/en/stable/getting_started/step_by_step/signals.html)。*

*在其他程序里，信号常被称为事件，它们是实现[观察者模式](https://en.wikipedia.org/wiki/Observer_pattern)的一种方式。*

## 📜 目录
[toc]

## 1. 信号事件

信号事件用于告知 Dialogic 外部的代码：某件事已经发生，或者应当发生。

当执行到该事件时，它会发出 `Dialogic.signal_event` 信号，并把事件中填写的参数一并传出去。

![信号事件](media/signal_event.png)

*这里的参数是 `"activate_something"`。*

要响应信号事件，请在开始对话之前先连接它：

```gdscript
func _ready():
     Dialogic.signal_event.connect(_on_dialogic_signal)

func _on_dialogic_signal(argument:String):
    if argument == "activate_something":
        print("Something was activated!")
```

---

## 2. 文本信号

使用 `[signal=activate_something]` 文本特效时，Dialogic 会带着给定参数发出 `Dialogic.text_signal` 信号。

你可以像连接 signal_event 信号那样连接它：

```gdscript
func _ready():
     Dialogic.text_signal.connect(_on_dialogic_text_signal)

func _on_dialogic_text_signal(argument:String):
    if argument == "activate_something":
        print("Something was activated!")
```

---

## 3. 开始与结束信号

使用 `Dialogic.timeline_ended` 和 `Dialogic.timeline_started` 信号（两者都没有参数），你可以响应对话的结束或开始。

示例：

```gdscript
func start_dialog():
    Dialogic.timeline_ended.connect(_on_timeline_ended)
    Dialogic.start("my_timeline")

func _on_timeline_ended():
    Dialogic.timeline_ended.disconnect(_on_timeline_ended)
    # 在这里做别的事情
```

---

## 4. 子系统信号

Dialogic 的各个子系统都有许多有用的信号。下面是其中一部分：

- `Dialogic.Text` 提供

    - 信号 `about_to_show_text(info:Dictionary)`
    - 信号 `text_finished(info:Dictionary)`
    - 信号 `speaker_updated(character:DialogicCharacter)`
    - 信号 `textbox_visibility_changed(visible:bool)`
    - 信号 `animation_textbox_new_text`
    - 信号 `animation_textbox_show`
    - 信号 `animation_textbox_hide`

- `Dialogic.Portraits` 提供

    - 信号 `character_joined(info:Dictionary)`
    - 信号 `character_left(info:Dictionary)`
    - 信号 `character_portrait_changed(info:Dictionary)`

- `Dialogic.VAR` 提供

    - 信号 `variable_changed(info:Dictionary)`
    - 信号 `variable_was_set(info:Dictionary)` # 仅在设置变量事件时

如果你想找到所有信号，请前往[子系统索引](subsystem_class_index.md)，选择一个子系统，然后查看相关的 **Signals** 分类。
