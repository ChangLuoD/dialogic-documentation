<div class="header-banner tropical">
     <div class="header-label tropical">变量</div>
</div>

*变量是在游戏中跟踪各种信息的好办法。
Dialogic 内置了易用且对新手友好的变量系统。不过，Dialogic 同样允许你轻松使用外部变量（或自动加载单例）。
你也可以从外部脚本访问 Dialogic 变量。*

为了充分利用这些变量，本页包含了你需要了解的全部内容。

## 📜 目录
[toc]

## 1. Dialogic 变量编辑器

Dialogic 变量可以在「变量」标签页中设置。每个变量由一个名称和一个初始默认值组成。

变量可以分组放在文件夹中，文件夹只有一个名称。

重要：在一个文件夹内，任何项目（变量或文件夹）都不能重名。

---

## 2. 在时间线中使用变量

### 2.1 文本中的变量

在文本事件中，你可以输入 `{variable_name}` 来插入一个 Dialogic 变量的值。
如果你的 Dialogic 变量位于某个文件夹（或多个文件夹）中，就必须用点号分隔写出完整名称：`{MyFolder.variable_name}` 或 `{FolderA.FolderB.variable_name}`

如果你想使用自动加载中的变量，请写 `{AutoloadName.variable}`。
这种语法也允许调用该自动加载上的方法。方法返回的内容会被插入文本：`{Autoload.get_random_name()}`

你甚至可以在花括号内进行简单的运算：`{{Player.Health}/10.0+2}`

### 2.2 条件

变量的主要用途之一是在条件中使用：`if`、`elif`，或选项上的条件。

##### 条件：可视化编辑器

在可视化编辑器中，你只需从下拉菜单中选择变量，并把它与你选择的值进行比较。如果需要更复杂的写法，请点击 `<E>` 按钮并使用下面描述的语法。

##### 条件：文本语法

在条件中，你可以使用与文本事件相同的所有技巧，另外还有几个额外的：

- 访问自动加载：`Autoload.property`
  
  - 在这些条件中，自动加载不必用 `{}` 花括号包裹。*

- 使用自动加载方法：`Autoload.check_info("Argument", false)`

  - 条件最终总是得到布尔值，类型转换遵循与 GDScript 相同的规则。

- 使用变量和典型的布尔运算符（`==`、`!=`、`<`、`>`、`<=`、`>=`、`not`、`and`、`or`、括号）：

  - `{Player.Health} > 5 or {Player.Luck} > randi()%10`

### 2.3 设置变量事件

你可以使用设置变量事件在时间线中更改变量。它既能设置 Dialogic 变量，也能设置自动加载变量。

##### 设置变量：可视化编辑器

在可视化编辑器中，你只需选择一个变量和一个值。你也可以直接在变量字段中输入 `Autoload.variable` 并按 `Enter` 提交，以此设置自动加载变量的值（没有自动补全）。

它将被设置为以下类型的值：

- 字符串，

- 数字（float 或 int），

- 另一个变量的值，

- 复杂表达式，

- 随机数

**复杂表达式**允许你使用全部可能性。在底层，所有这些类型都是表达式；之所以做出区分，只是为了简化界面。

表达式遵循下面描述的语法。

##### 设置变量：文本语法

设置变量事件有一套特殊语法，便于编写和阅读：

```dtl
set {variable_name} = 20

set {MyFolder.variable} += 2

set Autoload.property = "Hello World"

set {Player.Health} += range(10,20).pick_random()

set Autoload.health = {Player.Health} + 2
```

该值会作为表达式解析，与文本事件或条件中的 {expressions} 处理方式类似。这里也能做同样的事情，唯一的区别是它不会被转换成 string/bool。

### 2.4 文本输入事件

`Text Input` 事件是一种特殊的设置变量事件，它允许你把玩家所做的文本输入捕获到一个变量中。

---

## 3. 用变量做其他有趣的事

#### 3.1 用变量作为角色名称

角色的 `Display Name` 可以是一个变量。这是在游戏过程中控制该角色名称的最佳方式。要做到这一点，只需在角色编辑器的显示名称字段中输入 `{MyVariableName}`。

#### 3.2 术语表中的变量

你可以在术语表的标题、文本和附加内容中使用变量。这是在游戏过程中控制文本的最佳方式。你可以做到与文本事件中完全相同的事情。

---

## 4. 在 Dialogic 之外使用变量

你可能想从 Dialogic 之外的脚本中访问变量。

可以通过直接访问的方式做到：

```gdscript
Dialogic.VAR.my_variable = 20
print(Dialogic.VAR.Group.other_variable)
```

也可以通过字符串的方式做到：

```gdscript
Dialogic.VAR.set('my_variable', 20)
print(Dialogic.VAR.get('Group').get('my_variable'))
```

> [!WARNING]
> 变量只有在 Dialogic 自动加载就绪之后才可访问，所以也不要在你的节点就绪之前使用它们！


文件夹（以及根「文件夹」）有一些可能有用的方法：

- `folders()` 列出该文件夹中的所有文件夹（不是字符串，而是 `FolderObjects`）

- `variables(@absolute)` 列出该文件夹中所有变量的名称

- `has(@variable_name)` 如果存在这样的变量则返回 true

这让你可以做出这样的花样：

```gdscript
var location = Dialogic.VAR.Towns.folders().pick_random().variables().pick_random()
```

如果你的变量是这样设置的，它就能随机挑选一个房子/地点：

```
Towns（文件夹）

   - Town A（文件夹）

      - CharacterAHouse

      - CharacterBHouse

      - Church

      - School

   - Town B（文件夹）

      - CharacterCHouse

      - CharacterDHouse

      - University

      - Park
```

Dialogic.VAR 还有：

- `Dialogic.VAR.reset(@variable_path="")` 重置指定变量；如果未指定，则重置所有变量

- `Dialogic.VAR.parse_variables(@string)` 返回把变量替换后的给定字符串。文本事件等使用的就是它。
