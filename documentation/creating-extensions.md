<div class="header-banner ocean">
     <div class="header-label ocean">创建扩展</div>
</div>

*有时候 Dialogic 就是不够用*。

*无论缺的是一个事件、一个编辑器、一项设置还是一个文本特效，你都可以自己加上。
扩展就是一个文件夹，里面装着关于这些内容的全部信息，好让 Dialogic 能够使用它们。*
*扩展非常强大。其实它和 Dialogic 内置的东西一样强大，因为它们本来就是同一套机制。*

**扩展可以添加：**
- 主编辑器
- 事件
- 子系统
- 文本特效与文本修饰符
- 立绘动画
- 设置
- 角色设置
- 布局预设
- Dialogic 节点。

# 📜 目录
[toc]

# 1. 入门

创建扩展最好的方式，是使用 Dialogic 设置（General）页面里的扩展创建器。

<img src="/media/extension_creator.png" width="600"/>

点击 `Create New Extension` 按钮，就可以创建一个扩展文件夹和一个自定义事件脚本。为新模块输入名称，并勾选你想添加的内容，然后点击 `Create`。这样会在你的扩展文件夹里生成一个文件夹，其中包含两个脚本：`index.gd` 和 `event_(module name here).gd`。

> [!WARNING]
> 已存在的文件会被覆盖！

---

# 2. 关键部分：`index.gd`

任何扩展的核心都是 `index.gd` 脚本。它是你的扩展唯一必须有的东西。它必须继承 `DialogicIndexer`，并且可以重写该类的方法，来告诉 Dialogic 要添加哪些内容。

例如，下面这段代码注册了一个自定义事件：

```gdscript
func _get_events() -> Array:
    return [this_folder.path_join('event_print.gd')]
```

*去看看 `DialogicIndexer` 的其他方法，了解如何注册其他内容。*

---

# 3. 自定义事件

## 什么是事件？

Dialogic 事件是一个脚本，它定义了一个继承 `DialogicEvent` 的新类。这个脚本会定义 (a) 事件在编辑器中如何呈现，(b) 事件如何被保存，以及 (c) 当时间线执行到该事件时它做什么。

事件常常与子系统配合工作。

## 你的自定义事件

扩展创建器可以帮你得到一个基础的事件脚本。它已经为你设置好了一些值。

要让事件完全可用，你需要做以下几件事：

### 3.1 事件设置/参数/属性：

事件的所有选项（也就是你能「传给」事件的参数）都应该保存在变量里。把这些变量定义在脚本顶部。

```gdscript
var print_text: String = ""
var in_game: bool = false
```

### 3.2 执行代码：

在 `_execute()` 方法里写上事件被执行到时应该发生什么：

```gdscript
func _execute() -> void:
    print(print_text)

    if in_game and Dialogic.has_subsystem('Text'):
        Dialogic.Text.update_dialog_text(print_text)
    else:
        finish()
```

*`finish()` 方法会告诉 Dialogic 继续执行下一个事件。*

### 3.3 通用设置：

在 `_init()` 方法里，你可以设置事件的一些基础设置：

```gdscript
func _init() -> void:
    event_name = "Printer"
    set_default_color('Color3')
    event_category = "Godot"
```

你可以把 `icon.svg` 或 `icon.png` 文件放在事件的脚本旁边来更改事件图标，也可以重写 `_get_icon()` 方法并返回一个 Texture2D。

### 3.4 保存与读取（即解析、以及在时间线文本编辑器中的表示）

接下来讲如何使用短代码。它们基本上就是一个事件在时间线中的文本形态。
下面是一个背景事件的短代码。
```
[background arg="res://Graphics/Backgrounds/sunset.png" fade="1.5"]
```
要实现保存你的短代码，就在 `get_shortcode()` 中返回一个短代码标识符，并填好 `get_shortcode_parameters()`：

```gdscript
func get_shortcode() -> String:
    return "print"


func get_shortcode_parameters() -> Dictionary:
    return {
        #参数名            : 属性信息
        "text"            : { "property": "print_text", "default": "" },
        "ingame"          : { "property": "in_game",    "default": false },
    }
```

*上面这个事件可能会被保存为 `[print text="Some text to print" in_game="true"]`*

#### 3.4.1 疑难排查

如果你的变量在时间线编辑器里给自定义事件设了值却依然不变，这里有一份小清单：

- 记住你的变量名；示例变量名：`_audio_path`。
- 检查 `get_shortcode_parameters`：
  - 这样写是对的：`"path" : { "property": "_audio_path", "default": "" },`
  - `path` 会作为文本出现在短代码的文本视图里；它*可以*和你的变量同名。
  - `property` 的值必须和你的变量一致，见 `_audio_path`。
- 检查 `add_header_edit`，包括 `add_body_edit` 等：
  - 第一个参数是 `variable_name`，它必须和你的变量一致。
  - 一个例子：

	```gdscript
	add_header_edit(
		"_audio_path",
		ValueType.SINGLELINE_TEXT,
		{
			"left_text": "File Path",
			"mode": 1,
		},
	)
	```

当这些值全都对上之后，可视化模式和文本模式都会修改你事件里的变量，你就可以在 `_execute` 中访问它们了。

#### 3.4.2 自定义保存与读取语法

你也可以通过重写 `to_text() -> String` 和 `from_text(timeline_event: String)` 来实现自定义的保存语法。
如果你想快速检查事件名是否正确，还需要重写 `is_valid_event(event_name: String) -> bool`。

自定义保存与读取什么时候有用？当你的短代码有特殊的文本语法，或者需要在值之间做转换时——比如把一个单词映射成整数。
文本、角色、选项、条件和变量事件就是这么做的，如果你感兴趣可以去看看它们。
一个很好的例子是注释事件，你可以在 `addons/dialogic/modules/comment` 下找到它。


### 3.5 可视化编辑器字段：

你的事件现在功能齐全了，但在可视化编辑器里它还是一片空白。你需要重写 `build_event_editor()` 方法，来定义事件上会显示哪些字段/文本。

```gdscript
func build_event_editor() -> void:
   add_header_edit("print_text", ValueType.SINGLELINE_TEXT)
   add_header_edit("in_game", ValueType.BOOL, {"left_text":"(also show it in game:", "right_text":")"}, "!print_text.is_empty()")
```

*其他可用的方法有 `add_header_label()`、`add_body_edit()` 和 `add_body_line_break()`。其中大多数都允许把条件作为最后一个参数，从而实现自适应显示。在上面的例子里，只有第一个字段里输入了文字，第二个字段才会显示。*

如果你想了解更多关于事件的内容，我强烈建议你看看内置事件。

#### 3.5.1 疑难排查

如果你的自定义事件没有出现在可视化编辑器里，但用文本编辑器写出来却能生效，可以考虑重启 Godot 让改动生效。

---

# 4. 自定义子系统

## 什么是子系统？

子系统是一个脚本，里面装着游戏运行时常用的方法。游戏运行时，可以通过 `Dialogic.SubsystemName.method()` 访问子系统。它们应该承载你扩展的全部代码。
例如，内置子系统包括 `Text, Portraits, Choices, Audio, etc.`。它们的方法被内置事件使用，也可以被你的事件使用。

另外，如果由子系统来管理 dialogic 节点会更好。可以这样做：

```gdscript
func update_my_dialogic_nodes(some_setting):
    for node in get_tree().get_nodes_in_group("dialogic_custom_nodes"):
        node.update_something(some_setting)
```

## 你的自定义子系统

子系统是一个继承 `DialogicSubsystem` 的脚本。它可以重写该类的方法。
你可能最想重写的、最重要的方法是用于存档与读档的 `clear_game_state()` 和 `load_game_state()`，以及用于暂停的 `pause()` 和 `resume()`。
如果你想保存持久化数据，就把它存进 `Dialogic.current_state_info` 字典。

---

# 5. 自定义动画

## 什么是动画？

立绘动画是继承 `DialogicAnimation` 类的脚本。注册它们最简单的方式，是把它们放在你模块的一个子文件夹里，然后在 `index.gd` 文件中加入以下代码：

```gdscript
func _get_special_resources() -> Array[Dictionary]:
    # 第一个参数是子文件夹，第二个参数指定
    # 这是什么类型的资源（Dialogic 需要用到）
	return list_special_resources('MySubFolder', &'PortraitAnimation')
```

## 创建一个动画

动画是继承 `DialogicAnimation` 类的小脚本；你可以去 `dialogic/Modules/Character/DefaultAnimations` 看看默认动画（它们使用补间）。

你的动画脚本大致应该长这样（以 `bounce_in` 为例）：

### 动画示例 Bounce

```gdscript
extends DialogicAnimation

func animate():
    var tween := (node.create_tween() as Tween)
    node.scale = Vector2()
    node.modulate.a = 0

    tween.set_ease(Tween.EASE_IN_OUT)
    tween.set_trans(Tween.TRANS_SINE)
    tween.set_parallel()
    tween.tween_property(node, 'scale', Vector2(1,1), time).set_trans(Tween.TRANS_SPRING).set_ease(Tween.EASE_OUT)
    tween.tween_property(node, 'modulate:a', 1.0, time)
    tween.finished.connect(emit_signal.bind('finished_once'))
```

> [!NOTE]
> 注意，你必须：
> - 继承 `DialogicAnimation` 类并实现 `animate()` 方法
> - 在某个时刻发出 `finished_once` 信号；之所以这么叫，是因为 Dialogic 可以重复播放动画


*你可以使用以下变量：*

- **node** = 要动画的节点。它不是你的立绘节点本身，而是持有那个立绘节点的一个 Node2D
- **time** = 动画应该持续的时间
- **end_position** = 动画结束的位置
- **orig_pos** = 你起始的位置

## 为动画命名
重要的是，动画文件的名字会决定它是入场、退场还是更新动画！如果名字以 `_in` 结尾或包含 `_in_`，它就是入场动画；以 `_out` 结尾或包含 `_out_` 则是退场动画，而其他所有名字都算更新动画（在其他软件里常被称为吸引注意力类的动画）。

---

# 6. 自定义 dialogic 节点

## 什么是 dialogic 节点？

[Dialogic 节点](dialogic-nodes.md) 是那些以某种方式在视觉上、逻辑上或听觉上执行某些东西的节点。
它们通常由某个子系统管理，并通过被自动加入某个组来被发现。这样就保证了无论它们位于场景树的哪个位置、存在多少个，都不影响使用。

`DialogicNodes` 不需要加到 `index.gd` 文件里！

---

# 7. 自定义设置页面

扩展可能想要添加一个 dialogic 设置编辑器。它只是一个 UI 场景，其脚本继承 `DialogicSettingsPage`。

- 重写一些方法（只写你需要的那些）
  - `_get_title()`
  - _get_priority() -> 返回一个整数，影响设置页面的顺序
  - _refresh() -> 每当设置被打开时
  - _about_to_close() -> 每当设置编辑器关闭时
  - _get_info_section() -> 返回你场景中的一个节点，用作信息区域
设置的保存方式通常是
  - 保存在 `project settings` 中 dialogic 的子分类下（例如 `dialogic/myextension/setting`）：
    - `ProjectSettings.set_setting('dialogic/myextension/setting', some_value)`
  - 你也可以用 `DialogicUtil.set_editor_setting('setting', value)` 和 `DialogicUtil.get_editor_setting('setting')` 保存/读取编辑器设置。
    - 这些取决于项目
- 记住，这个场景里的所有脚本都必须处于 @tool 模式。
