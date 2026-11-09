<div class="header-banner dawn">
     <div class="header-label dawn">存档与读档</div>
</div>

*显然，很多游戏都需要存档与读档功能！*

## 📜 目录
[toc]

## 1. 内置存档

存档子系统让你可以把 Dialogic 时间线状态、游戏缩略图（截图）以及额外信息保存到存档槽位中，并在需要时读回来。\
想保存玩家位置或最后看到的文本？内置存档系统全都帮你搞定了！

请注意，存档会以它们的名称作为文件夹名；操作系统对文件夹名允许的字符有限制。
建议只使用拉丁字母、数字和下划线。

如果你不在意存档槽位，可以这样做：

```gdscript
# 示例信号处理。
func _on_save_game_button_pressed() -> void:
    Dialogic.Save.save()

func _on_load_game_button_pressed() -> void:
    Dialogic.Save.load()
```

不过，这些方法也会一并截图，可能会稍微影响性能。\
如果你想避免这种情况，写法会略微啰嗦一些：

```gdscript
# 示例信号处理。
func _on_save_game_button_pressed() -> void:
    Dialogic.Save.save("", false, Dialogic.Save.ThumbnailMode.NONE)

func _on_load_game_button_pressed() -> void:
    Dialogic.Save.load()
```


### 1.1 拍摄缩略图

如果你希望每个存档槽位都有自己的缩略图，建议在打开你的*存档与读档*图层之前先截图。\
这可以通过在显示图层*之前*调用 `Dialogic.Save.take_thumbnail()` 实现；例如，在按钮的信号连接里先打开图层。

如果你采用这种方式，就需要让 `Dialogic.Save.save(...)` 方法*只存储*，而*不要*截图。

```gdscript
Dialogic.Save.save(slot_name_variable, false, Dialogic.Save.ThumbnailMode.STORE_ONLY, slot_extra_info)
```

这段代码会自动取用保存在 `Dialogic.Save.latest_thumbnail` 的缩略图。

## 1.2 保存额外信息

默认情况下，存档里包含 Dialogic 时间线和 Dialogic 变量的信息。如果你想保存某个存档的特定信息，可以使用 `Dialogic.Save.set_slot_info(slot_name:String, info: Dictionary)` 和 `Dialogic.Save.get_slot_info(slot_name: String)` 方法。\
不过，`Dialogic.Save.save(...)` 方法可以简化这项工作，省去调用 set 方法的步骤。

额外信息*不会*被载入 Dialogic 的状态信息。换句话说，它只用于提供*关于*某个存档的信息。

在下面的代码片段中，我们会把最后使用的文本行和当前时间作为额外信息保存下来。

```gdscript
var extra_info := {}
extra_info["text"] = Dialogic.current_state_info["text"]
extra_info["date"] = Time.get_datetime_string_from_system(false, true)

Dialogic.Save.save(slot_name, false, Dialogic.Save.ThumbnailMode.STORE_ONLY, extra_info)
```

## 1.2.1 读取额外信息

现在，如果你想在自己的自定义存档图层上显示存档，可以通过 `Dialogic.Save.get_slot_info` 读取它们。

下面是一个读取某个存档额外数据的示例：

```gdscript
# 安全检查：我们的存档是否真的存在。
if Dialogic.Save.has_slot(save_game_name):
     var slot_info_dictionary := Dialogic.Save.get_slot_info(save_game_name)
     var date: String = slot_info_dictionary.get("date", "")
     var text: String = slot_info_dictionary.get("text", "")
```

这个示例会检查名为 `save_game_name` 的存档是否存在，如果存在就取出它的额外数据。
之后，你就可以随意使用这些数据了，例如把它们显示在自定义存档图层上。

### 1.3 全局数据



不依赖槽位、简单地保存游戏数据的方式是使用 `Dialogic.Settings` 子系统。\
你可以直接往它上面存信息：
```gdscript
Dialogic.Settings.text_speed = 0.05
```

存档时，数据会被自动写入全局存档文件。

不过，如果你想直接写入全局存档文件，可以使用 `Dialogic.Save` 子系统：
```gdscript
func set_player_name(name: String) -> void:
    # 为键 `player_name` 设置名称。
    Dialogic.Save.set_global_info("player_name", name)
```

全局信息的 `key` 取值没有硬性规定，不过保持一致仍是个好习惯。

如果你对保存已读文本的历史记录感兴趣，请看下一节。


### 1.4 保存已访问的事件

在玩视觉小说并使用多个存档时，你可能会在完全不同的剧情分支上再次看到已经读过的文本。\
默认情况下，Dialogic *不会*保存玩家已经访问过的事件；不过有两种主要方法可以让它生效。

1. 第一种方法是通过 Dialogic 编辑器。前往 `Settings` 标签页并选择 `History`。在这里，你可以决定是否让已访问的事件自动保存。\

2. 这种方法略有不同：下面的代码片段会手动把历史记录保存到文件中。
如果玩家已经到达游戏结局（制作人员名单？），而你想确保已访问的事件
最后一次被正确更新，就可以使用它。

```gdscript
var slot_name := "slot-3-page-2"

func _save_game() -> void:
     Dialogic.History.save_visited_history()
     Dialogic.Save.save(slot_name)

func _load_game() -> void:
     Dialogic.History.load_visited_history()
     Dialogic.Save.load(slot_name)

func _reset_visited_history() -> void:
     Dialogic.History.reset_already_visited_history()
```

如果你更喜欢用代码来设置，可以看看下面的代码片段：

```gdscript
func _ready() -> void:
    Dialogic.History.save_already_visited_history_on_save = true
    Dialogic.History.save_already_visited_history_on_autosave = true
```

示例中的取值会让已访问事件的历史记录在自动存档和普通存档时自动保存。



# 2. 处理存档槽位

存档槽位由它的槽位名称来标识，这是一个由你（开发者）决定的唯一单词。\
命名槽位的一种方式是使用索引加上当前存档页（如果你的存档很多）。这样得到的槽位名可能是 `slot_3_page_4`。

# 2.1 检查存档槽位是否存在

当你自己制作存档与读档图层时，可能需要检查这些槽位是否已经存在。

```gdscript
func load_save_game_slot(slot_name: String) -> void:
    if Dialogic.Save.has_slot(slot_name):
        # 执行你的逻辑...
```

不过，你也可以使用 `Dialogic.Save.add_empty_slot(slot_name: String)` 来填充缺失的槽位。但如果你用上面的片段做了检查，那就完全没必要了。

# 2.2 删除存档槽位

你可以使用 `Dialogic.Save.delete_slot(slot_name: String)` 方法删除存档槽位。


# 3. 手动存档

如果你想自己实现一套存档与读档系统，可以用 `Dialogic.get_full_state() -> Dictionary` 获取 Dialogic 的状态，然后按你喜欢的方式保存它。\
等你准备好把数据读回来时，需要使用 `Dialogic.load_full_state(state: Dictionary)` 把数据重新载入 Dialogic。

总的来说，最好先想清楚*为什么*要做手动存档，并从 Dialogic 的 `save` 和 `load` 方法中获取灵感。\
Dialogic 很可能已经支持你特定的存档与读档需求。
