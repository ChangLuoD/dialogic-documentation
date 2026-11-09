<div class="header-banner purple">
     <div class="header-label purple">自动前进</div>
</div>

*自动前进让你无需玩家输入即可推进时间线。*

## 📜 目录
[toc]

## 1. 什么是自动前进？

自动前进是 Dialogic 中一项改变体验的功能，它让玩家无需手动输入就能自动走完时间线！
它特别适合为玩家提供无需动手的体验，作为游戏里传统「点击推进」方式的一种替代。

你可能也见过它叫「auto-forward」，或者干脆就叫「auto」。

---

## 2. 主要设置

在 Dialogic 的 Settings 标签页中，Text 分区下可以找到自动前进的主要设置。把鼠标悬停在提示图标上，可以了解每项设置的更多信息。
![header_saving_loading](media/auto_advance_settings.png)

#### 2.1 额外延迟

一个关键设置是 `Additional Delay`，它让你可以在 **Per Character**（按字）与 **Per Word**（按词）之间选择。这些设置会改变文字显示的速度，按字数或词数追加额外延迟。

有些语言（例如日语）不用空格分隔单词。Dialogic 用空格键输入的那种空白字符来判断一个词在哪里结束、另一个词从哪里开始。如果你打算提供多种本地化版本，可以通过 API 同时设置这两项。

##### 忽略字符

这些字符在计数时会被忽略。这个功能可以让你进一步微调自动前进的体验。如果你用不到，把它关掉就好。

---

## 3. 通过代码修改自动前进

通过 Inputs 子系统可以很方便地访问 Dialogic 的自动前进类，从而在游戏运行期间修改自动前进。它还有一些未暴露在 Dialogic 界面上的设置。

所有这些功能都位于 `Dialogic.Inputs.auto_advance`，一个 `DialogicAutoAdvance` 对象中。

此外，你可以通过发出 `autoadvance` 信号来强制推进：`Dialogic.Inputs.auto_advance.autoadvance.emit()`。

### 3.1 设置/属性

设置/属性有两类：

#### 启用条件

- `enabled_until_next_event: bool`
- `enabled_forced: bool`
- `enabled_until_user_input: bool`

只要有一个启用条件为 `true`，自动前进就会继续。
这样就可以把启用自动前进的多个原因叠加起来。设想一下：玩家开启了自动前进，同时文本事件又把它强制开启到下一个事件为止。

多亏了多个启用条件，即使玩家关掉了自动前进，
它仍会继续到下一个事件为止。

把任意一个启用变量设为 `true` 就能开启自动前进。如果你有一个自动前进按钮，可以用下面的代码来开启该功能：

```gdscript
Dialogic.Inputs.auto_advance.enabled_until_user_input = true
```

#### 改变行为的设置

- `fixed_delay: float` *与设置中的基础延迟相同*
- `per_word_delay: float` *额外延迟乘以词数*
- `per_character_delay: float` *额外延迟乘以字符数*
- `await_playing_voice: bool` *如果为 true（默认），自动前进会等待语音播完*

##### 按字延迟与按词延迟

可以看到，使用这些属性你可以同时设置按词延迟与按字延迟，二者会相加。

为了适配词与词之间没有空格的语言，你可以在切换语言时调整 `per_word_delay` 与 `per_character_delay` 变量。

下面是一个在脚本中配置这些设置的例子：

```gdscript
# 我们可以直接写入返回的字典来修改设置。
Dialogic.Inputs.auto_advance.per_word_delay = 0.3
Dialogic.Inputs.auto_advance.per_character_delay = 0.1
```

### 3.2 切换信号

如果你想在自动前进状态变化时收到通知，可以使用 `Dialogic.Inputs.auto_advance.toggled` 信号。

```gdscript
signal toggled(enabled: bool)
```

如果你好奇 Dialogic 内部是怎么使用这个信号的：`Inputs` 子系统会连接它：

```gdscript
Dialogic.Inputs.autoadvance.toggled.connect(_on_autoadvance_toggled)
```

---

## 4. 调整阅读速度

在视觉小说中，让玩家可以设置文字速度是常见做法。
如果想用 Dialogic 提供同样的功能，可以考虑设置 `Dialogic.Settings.autoadvance_delay_modifier`。

这个设置位于 `Dialogic.Settings` 中，让你可以微调延迟系数，乘以「Additional Delay」。
默认情况下，如果没有设置该值，Dialogic 会使用系数 1，延迟不会发生变化。

> [!NOTE]
> 添加到 `Dialogic.Settings` 的每项设置，Dialogic 都会为*玩家*自动保存和加载。


## 5. 自动前进文本特效

在文本中写上 `[aa]` 或 `[aa = 2]`，就能在文本事件中临时开启自动前进，并覆盖延迟。

- `[aa]` 会开启自动前进，直到（至少）下一个文本事件

- `[aa=2]` 会开启自动前进，并把延迟设为 2 秒。
  
  - 这会忽略 `per_word_delay` 与 `per_character_delay`，但依然遵循 `await_playing_voice`。

- `[aa=2?]` **不会**开启自动前进，但如果自动前进已经开启，就把时间设为 2 秒。
