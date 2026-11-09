<div class="header-banner dawn">
     <div class="header-label dawn">时间线文本语法</div>
</div>

*时间线以文本格式保存，这意味着你可以用任何文本编辑软件来编辑和创建它们。内置的文本编辑器提供了实用的自动补全和语法高亮。*

*Dialogic 要识别你的时间线文件，它必须使用 `.dtl` 扩展名！*

## 📜 目录

[toc]

## 关于短代码事件

大多数事件采用类似短代码的风格：

```dtl
[background arg="res://icon.png" fade="1.0"]
```

参数的顺序无关紧要，但它们之间必须至少用一个空格分隔。所有参数，无论是什么类型，都必须用双引号括起来。

要了解每个事件可用的全部参数，可以在 Godot 帮助或本文档中搜索它们对应的类，查看其文档。

## 关于特殊事件

有些事件带有自定义语法，以便更容易编写。包括：

### 文本事件

```dtl
一个精彩的文本事件，由某个不知名的人说出。

Emilio: 你好，欢迎！

Emilio (excited): 我很兴奋，你看得出来吗？

Emilio: 这是一个带 \
多行的文本事件。很棒吧？确实！
```

在文本事件末尾加上 `\` 会让它把下一行也包含进来。

### 角色事件

```dtl
join Emilio (happy) center [animation="Bounce In"]

leave Emilio [animation="Bounce Out" length="0.3"]

update Emilio (excited) left [animation="Tada" wait="true" repeat="3" move_time="0.3"]
```

*更多信息和示例请查看[角色事件](./event-character.md)文档。*

### 选项事件

可以用 `|` 把逻辑加到选项背后。

```dtl
- Yes
- No | [if {John.Relationship} > 23]
- Maybe | [if {Stats.Charisma} > 10] [else="disable" alt_text="Maybe [too insecure]"]
```

### 条件事件

```dtl
if {Player.Wisdom} > 3:
    # Dialogic 的语法基于缩进！
elif {Player.Health} <= 10:
    # 一些事件
else:
    #...
```

### 设置变量事件

```dtl
set {MyVariable} += 10
```

支持的运算符有 =、+=、-=、*=、/=

### 注释事件

```dtl
# 一些注释
```

### 标签事件

```dtl
label LabelIdentifier

# 带显示名称的标签
label LabelIdentifier (显示名称)
```

### 跳转事件

```dtl
jump LabelIdentifier

# 跳转到另一个时间线中的标签
jump TimelineName/LabelIdentifier

# 跳转到另一个时间线的开头
jump TimelineName/
```

### 返回事件

```dtl
return
```

### Do/Call 事件

```dtl
do Autoload.method("argument")
```

## 关于缩进

时间线使用 TAB 缩进来判断哪些事件属于某个选项或条件块。只会考虑缩进的变化，所以缩进多少其实并不重要，只要在同一个块内保持一致即可。

## 时间线示例

```dtl
[background arg="res://assets/backgrounds/dialogic_factory.png"]

join Jowan left
Jowan (excited): 你好，欢迎来到[portrait=confused]……[pause=0.5] 等等？这是什么？

join Emilio (happy) right
Emilio: 嗯，这就是示例时间线。

Jowan: 我还以为这是一个很酷的新功能？

Emilio: 不，抱歉。

Jowan (sad): 没关系。

# 一些选项会跳回这里
label WhatAbout

Jowan (default): 那么这个示例应该讲些什么呢？
- 怎样烤曲奇
    Emilio (confused): 等等，这和 Dialogic 有关系吗？！
    jump WhatAbout

- 怎样登上月球 | [if {Player.Name} == "NASA"]
    Jowan (angry): NASA！又是你。这里是做对话的地方！\
    关于月球的问题请去问别人！
    
    jump WhatAbout

- 怎样以文本格式编写时间线
    Jowan: 哦，其实挺直观的。[pause= 0.2][portrait=questioning] 我希望如此。
    
    Emilio: 希望它也同样简单。

[end_timeline]
```
