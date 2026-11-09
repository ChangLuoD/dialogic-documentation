<div class="header-banner tropical">
     <div class="header-label tropical">文本修饰符</div>
</div>

*文本修饰符是文本预处理器，也就是说，在文本显示之前会先对它运行一个方法，改变文本内容。许多文本修饰符都带有你需要使用的特殊语法。*

*大多数文本修饰符在文本事件和选项事件中都有效，但并非全部。*

## 📜 目录
[toc]

## 换行

**语法：**  `[br]`

**支持：** *文本事件、选项事件*

该特效插入一个换行。这在选项中（无法添加真正的换行）很有用，或者在使用「新行作为新事件」设置、而又想插入一个不触发新事件的换行时很有用。

---

## 随机选择

**语法：** `<Option1/Option2/Option3>`

**示例：**  `<Hey what's up?/How are you doing?/What's up?>`

**支持：** *文本事件、选项事件*

如果某段文本在游戏中会多次出现，这是一种添加变化的好办法。

---

## 条件文本

**语法：** 

- `[if {Expression} Text if true./Text if false.]` 

- `[if {Expression} Text if true.]`

**示例：** 

- `[if {KeyCollected} You have a key./You don't have any key.]`

- `[if {PlayerDidSomething} You remember the thing you did...]`

**支持：** *文本事件、选项事件*

这样可以轻松地把条件文本插入文本事件，而无需添加一整个条件事件和两个文本事件。 

---

## 自动暂停

**支持：** *文本事件*

自动暂停是通过文本修饰符实现的。与自动暂停相关的设置位于文本设置中。该方法会为设置中指定的字母自动插入 [pause=x] 特效。

---

# 自定义文本修饰符

添加自定义文本修饰符非常简单。如果你使用的是自定义模块设置，只需在 `_get_text_modifiers()` 方法中注册修饰符即可。你需要指定持有该方法的子系统或节点，以及方法的名称。

你还可以指定它是在文本事件和选项上都调用，还是只在其中之一调用（默认只在文本事件上调用）。

```gdscript
func _get_text_modifiers() -> Array[Dictionary]:
    return [
        {
            "subsystem":"MySubsystem", 
             # 或者这样写
             #"node_path":"/root/MyAutoload",
            "method":"my_modifier_method", 
            "mode": -1}, #-1 = 文本和选项，0 = 仅文本，1 = 仅选项
    ]
```

实际的修饰符方法应当接收一个 String 参数并返回一个 String。

例如，下面的方法会把所有 `e` 换成 `E`： 

```gdscript
func big_e_modifier(text: String) -> String:
    text = text.replace("e", "E")
    return text
```

当然，你可能会想用更复杂的正则表达式，或自己发明一套语法，但以上就是基础做法！
