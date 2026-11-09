<div class="header-banner tropical">
     <div class="header-label tropical">跳转、标签与返回事件</div>
</div>

*跳转事件用于改变时间线的流程，或切换到另一条时间线。它天然与标签事件绑定在一起。
跳转流程与标签事件、返回事件紧密相连。*

## 📜 目录
[toc]


## 1. 文本语法

不使用任何目标的 `jump` 会把下一个事件设为当前时间线的开头。

```dtl
# 使用 "jump" 会回到这条时间线的开头。
character: 你好，这是开头。
character: 这是第二条消息。
jump
```

下面用标签事件来介绍标签。这会打印 `你好 1`、`你好 2` 和 `你好 3`。之后，跳转会再打印 `你好 2` 和 `你好 3`。

```dtl
character: 你好 1
label your_label_name
character: 你好 2
character: 你好 3
jump your_label_name
```

---

## 2. 跳转栈
每当你使用跳转事件，它都会在改变流程之前记录你当前所在的位置。这样就可以使用返回事件，把流程倒回上一个跳转点。
如果你从时间线 A 跳转到时间线 B 的起点，然后在时间线 B 中使用返回事件，它就会退回时间线 A。这一逻辑行为是用栈实现的。

## 3. 通过代码跳转

### 3.1 从标签开始
如果你想从某个特定标签开始，可以在开始事件中传入它：`Dialogic.start(timeline, "your_label_name")`。

### 3.2 跳转信号
可以在这里查看我们提供的信号：[所有信号列表](../classes/subsystem_jump.html#signals)。

## 3.3 直接跳转
如果从标签开始还不够，你也可以使用 `Dialogic.Jump.jump_to_label(label: String)`。它不会使用跳转栈。
另外，你也可以使用 `Dialogic.Jump.resume_from_last_jump()`。
