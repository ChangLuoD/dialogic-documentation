<div class="header-banner pineapple">
     <div class="header-label pineapple">术语表</div>
</div>

*本页介绍如何使用内置的术语表功能。*

## 📜 目录
[toc]

## 1. 什么是术语表？

术语表是一组词条及其释义的集合。它是记录游戏世界观、角色以及更多内容的绝佳方式。

一些视觉小说会用术语表来解释玩家可能不熟悉的词。例如，如果你的游戏设定在奇幻世界，你也许想解释这个世界里独有的种族。

### 1.1 Dialogic 的术语表能做什么？

如果术语表包含键，Dialogic 会自动使用每个词条的 `name` 和 `alternatives` 属性来高亮它们，并在鼠标悬停时提供带有释义的工具提示。

## 2. 如何翻译术语表？

更多信息请查看[翻译术语表](translation.md#25-翻译术语表)。

## 3. 实现

> [!NOTE]
> 本节面向想要了解术语表内部工作原理的程序员。


术语表是一个文本资源文件（`tres`），每次改动术语表之后，都必须把它保存到磁盘，否则改动不会保留。

此外，它有一个 `entries` 属性，包含由 `Dictionary` 组成的 `Array`，其中每个 `Dictionary` 代表一个术语表词条。

### 3.1 翻译 ID

一旦完成翻译，每个词条都会拥有一个唯一的 `_translation_id`。\
`_translation_id` 会被添加到术语表的 `_translation_keys` 属性中，
把术语表的名称属性映射到它们对应词条在数组中的索引。


### 3.2 别名键

词条键是术语表词条的唯一标识符，可以是名称、替代名（别名），或词条的翻译 ID（CSV 键）。

别名用于把词条映射到真正的术语表词条键，让代码能把术语表中的词映射到它的术语表词条。

此外，词条的翻译 ID 让代码可以把翻译后的术语表词映射到它的翻译 ID，再通过 `_translation_keys` 映射到术语表词条。\
术语表词条的 ID 是组合而成的，先使用术语表 ID，然后是词条 ID。
这与已翻译术语表的翻译 CSV 键一致：
`Glossary/3b/6c/name`。


### 3.2 访问术语表属性

经验法则是：如果你想从代码中访问术语表，不要访问前面提到的那些带 `_` 前缀的属性：`_translation_id`、
`_translation_keys`。它们可能会在 Dialogic 的开发过程中发生变化。

请改为使用 `DialogicGlossary` 上的辅助方法。


### 3.3 如何获取所有术语表路径？

你可以使用下面的项目设置来获取所有术语表文件：
```gdscript
var glossary_paths: Array[String] = ProjectSettings.get_setting('dialogic/glossary/glossary_files', [])
```
