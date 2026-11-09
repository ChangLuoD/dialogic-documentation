<div class="header-banner pineapple">
     <div class="header-label pineapple">翻译</div>
</div>

*本页介绍 Dialogic 如何让你翻译时间线以及对话中其他重要的部分。*

## 📜 目录
[toc]

## 1. 什么是翻译？

翻译是 Godot 和 Dialogic 2 的功能，让你可以把游戏翻译成多种语言。

有时也会使用*本地化*这个词。
本地化会为每类受众提供非常具体的区域化考量。\
这不仅包括游戏的翻译，还包括对游戏本身的改动，以体现特定的文化差异。

虽然 Dialogic 支持翻译你的时间线，但最好先专注于游戏本身，之后再添加翻译。\
不过，从一开始就把翻译和本地化纳入考虑，是个非常好的主意：

- 你要如何处理变量？
- 你需要术语表吗？
- 你想本地化哪些图片？
- 有没有哪些功能可能会让翻译变得复杂？

> [!NOTE]
> 目前，Dialogic 只支持 CSV 翻译。
> Godot 本身也支持 `gettext`。


## 1.1 Dialogic 能翻译哪些内容？

Dialogic 可以自动翻译你的时间线、角色名称和术语表词条。

## 2. 如何翻译时间线？

在 Dialogic 中，前往 Settings 标签页，查看 Translation 区域。勾选 “Enable Translation” 复选框。

![translation_settings](media/translation/translation_settings.png)

### 2.1 设置翻译

我们来逐个过一遍这些设置！你需要选择一个默认的语言/区域设置（locale）。
这个 locale 必须是你撰写时间线所用的语言。
此外，当某个可翻译事件没有对应翻译时，这个 locale 会被用作回退方案。
“translation file mode” 允许你把所有时间线存进一个文件（Per Project），或分成多个文件（Per Timeline）。

设置一个翻译文件夹是个好主意；它能让项目保持整洁，也方便你找到翻译文件。

### 2.2 撰写翻译

首先，你需要一个时间线文件。点击 “Update CSV files” 按钮后，Dialogic 会自动为你找到并生成 CSV 文件。

CSV 格式很简单，你可以用任何电子表格软件打开它，比如 Excel 或 Google Sheets。甚至 VSCode 也有编辑该文件的扩展。\
CSV 文件的要点是：每一行代表一条记录，每一列用逗号分隔。

Dialogic 生成 CSV 文件后，它可能长这样：

```csv
keys,en
Text/1/text,Hello World!"
```

`keys` 是区域设置；`en` 是英语的 locale 代码。如果你选择别的默认 locale，locale 代码也会不同。

你可以通过添加新的一列来为游戏添加新语言。下面的示例在第一行添加了代表日语的 “ja”，并翻译了第二行的文本。注意逗号的位置！

```csv
keys,en,ja
Text/1/text,Hello World!,こんにちは世界！
```

就是这样！现在你可以点击 “Collect translation”，Dialogic 就会为你生成翻译文件。\
翻译文件是一种特定的 Godot 文件。这里是它们的官方文档：[国际化游戏](https://docs.godotengine.org/en/stable/tutorials/i18n/internationalizing_games.html)

> [!NOTE]
> 在 CSV 中，你用逗号分隔列。不过，如果你想在文本内部使用逗号，就必须用引号把文本括起来，并对嵌套的引号进行转义。


> [!WARNING]
> 生成翻译之后，如果你改动了某个已翻译单元（文本事件、角色名称或术语表词条）的文本，就必须重新生成翻译。


### 2.3 翻译工作流程

从现在起，每当你改动 CSV 文件，都可以点击 “Collect translation”，Dialogic 就会为你更新翻译文件。\
编辑完时间线后，你需要点击 “Update CSV files” 来更新 CSV 文件。

要验证你的时间线是否已正确把文本插入 CSV，你可以切换到时间线的文本编辑器。当然，你也可以直接检查 CSV 文件。

```dtl
Character: Hello world! #id:14
Do you like Visual Novels? #id:15
- Yes, I do! #id:16
- No, I love them! #id:17
That's the spirit! #id:18
```

每个文本事件末尾的 `#id` 标记就是翻译 ID。它们在文本显示时不会出现；不过，Dialogic 已经把文本插入到 CSV 文件中了。

### 2.4 翻译角色

如果角色属于某个时间线，它们就会被翻译。
它们总是会被放进一个按项目（per-project）的文件中。它们的名称和昵称都会被翻译。

## 2.5 翻译术语表

术语表有一个专门保留用于翻译的属性：`_translation_id`。\
它们还有一个保留的 `_translation_keys` 属性，用于把翻译后的词映射到
翻译键。

这些属性会通过 “Update CSV files” 按钮自动填充。\
以 `_`（下划线）为前缀的条目被视为私有，会被忽略。其他每个 `String` 属性都会被翻译并写入 CSV。

> [!WARNING]
> 术语表词条的名称和替代名必须唯一，翻译后的对应内容同样如此。\
> 否则将无法解析出正确的词条。
> 术语表名称或替代名不能以 `Glossary/` 开头。


一旦启用了翻译，而你在测试另一种 locale，
当某个词条属性尚未翻译时，术语表不会回退到默认 locale。

## 2. 测试翻译

如果一切顺利，你可以在 “Testing locale” 下拉菜单中选择另一种 locale。\
点击 “Play Timeline” 或 “Run Project”，看看翻译后的文本。\
这个设置仅限编辑器使用，在导出的项目中可能不生效。

---

## 3. 更改语言

翻译流程由 Godot 负责处理！

要在游戏导出后更改 locale，请使用 [TranslationServer](https://docs.godotengine.org/en/stable/classes/class_translationserver.html#translationserver) 类上的 Godot API 方法：

```gdscript
# 日语的语言代码是 "ja"
TranslationServer.set_locale("ja")
```
