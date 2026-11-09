<div class="header-banner purple">
     <div class="header-label ocean">自定义选项按钮的额外数据</div>
</div>

*有时候你需要选项按钮做的不只是响应点击和显示文字。Dialogic 允许你通过一个「额外数据」字典，在选项事件与选项按钮节点之间轻松通信。太好了。*

## 📜 目录
[toc]


## 这是做什么的？
Dialogic 默认的选项按钮可以做三件事：
- 显示文字
- 根据条件隐藏/禁用自身
- 可选地在条件不成立时显示另一段文字

通常你会想要更多，例如
- 一个图标
- 一种颜色
- 一段花哨的动画

或者别的什么。

要做到这一点，你可以创建一个自定义选项按钮脚本，让它响应选项事件上提供的额外数据。
这意味着如果你还没有自定义选项图层，就得先做这一步。

## 实现示例
下面是一个可以应用到所有选项按钮上的示例脚本。
```gdscript
extends DialogicNode_ChoiceButton

func _load_info(info:Dictionary) -> void:
    # 加载文字与可见性
    super(info)
    
    # 示例：根据字符串（十六进制颜色码或颜色名）改变调制颜色
    modulate = Color(info.get("color", "WHITE"))

    # 示例：根据路径改变图标
    if ResourceLoader.exists(info.get("icon", "")):
        icon = load(info.get("icon"))
    else:
        icon = null
    
    # 响应你可能想用到的所有额外数据
    # （例如音效、工具提示、动画，等等。）
```

对于使用上面脚本的选项按钮，一个选项事件可以长这样：

![](media/custom_choices/custom_choice_example.png)

它在文本编辑器中等同于下面这样：
```dtl
- 一个绝妙的选项 | [color="Blue" icon="res://icon.svg"]
```

## 细节
你的选项按钮脚本应当继承 [DialogicNode_ChoiceButton](/classes/class_dialogicnode_choicebutton.md)。

如果你重写了 `_load_info()`，但仍想沿用 Dialogic 基本的文字与条件逻辑，就在其中调用 `super(info)`。
