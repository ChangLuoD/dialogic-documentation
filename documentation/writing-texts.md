<div class="header-banner tropical">
     <div class="header-label tropical">编写文本</div>
</div>

*在文本事件或选项中编写文本时，你可以利用许多系统来调整文本的行为、内容与样式。*
## 📜 目录

[toc]

## BBCode

在文本事件中你可以使用 BBCode 标签，为文本设置样式：从粗体、斜体和下划线，到颜色、字体、图片以及各种夸张的波浪效果。详情请参阅 Godot 文档：[RichTextLabel 中的 BBCode](https://docs.godotengine.org/en/stable/tutorials/ui/bbcode_in_richtextlabel.html)。
如果你需要在选项按钮上使用 BBCode，就必须使用以 RichTextLabel 显示文本的自定义选项按钮！

## 变量/表达式

`{`花括号`}` 中的任何内容都会被当作表达式。它会在文本显示时被求值，并把结果插入文本。这可以是一个简单的 **Dialogic 变量**，也可以是**自动加载（Autoload）上的变量**。详情请参阅[变量页面](./variables.md)。

## 文本修饰符

文本修饰符涵盖从简单的插入（如 `[br]` 用于插入换行），到 `<random/selection>`（随机选择一个选项），再到条件文本 `[if {Expression} Show if true./Else show this.]`（根据表达式的结果打印其中一段文本）。

你可以在这里找到文本修饰符的完整列表：[文本修饰符](./text-modifiers.md)

## 文本特效

文本特效看起来与 BBCode 相似，但通常会在文本逐字显示到该位置时产生某种效果。例如 `[pause=x]`（暂停 x 秒）、`[speed=x]`（改变显示速度）或 `[signal=x]`（以参数 x 发出 Dialogic.text_signal）。

你可以在这里找到所有文本特效的列表：[文本特效](./text-effects.md)
