<div class="header-banner tropical">
     <div class="header-label tropical">角色事件</div>
</div>

*角色事件用于拥有立绘的角色。你可以用它让角色加入、离开或更新，并配置它们在游戏中的呈现方式。*

## 📜 目录
[toc]


## 文本语法

```dtl
# 基本的加入事件
join MyCharacter center
# 指定立绘与短代码参数
join MyCharacter (SomePortrait) center [animation="Bounce In"]
# 名字中带空格的角色，以及位于分组中的立绘
join "My Character" (Special/Crazy) center
# 手动指定变换
join MyCharacter pos=x0.5y1 size=y400px rot=20`

# 基本的离开事件
leave MyCharacter
# 指定短代码参数
leave MyCharacter [animation="Fade Down"]
# 让所有已加入的角色离开
leave --All--

# 带动画的更新
update MyCharacter [animation="Tada" length="2" wait="true"]
# 改变位置（并补间）的更新
update MyCharacter left [move_time="1" move_trans="Elastic" move_ease="In"]
# 更换立绘的更新
update MyCharacter (OtherPortrait) [fade_time="0.5"]
```

---

## 主要设置

### 动作设置 | Join / Leave / Update

角色事件有三种模式：`Join`、`Leave` 和 `Update`。

**加入**事件会添加一个角色。它要求给定位置/变换，并使用 `In` 类动画。

**离开**事件会移除一个或全部角色。它们会忽略大部分设置，并使用 `Out` 类动画。

**更新**事件允许你修改内容，例如位置或立绘。它也可以用来给角色播放动画。它有几个加入与离开用不到的设置：Fade 与 Fade Time、Move Time、Move Trans 与 Move Ease。此外，更新事件只在某个设置被指定时才会修改它，因此如果没有给定立绘，就保留之前的立绘，位置等其他设置同理。

**文本语法：**
模式通过在事件开头使用 `join`、`leave` 或 `update` 来设置。

---

### 角色设置

要使用的角色。在 `Leave` 模式下，这里可以是 `--All--`，表示全部角色。

**文本语法：** `join MyCharacter`、`leave "My Character"`、`leave --All--`

在 join/leave/update 之后紧接着写角色标识符。如果标识符中含有空格，就必须把它放进引号里。

---

### 立绘设置

**文本语法：** 在角色标识符之后，把立绘写在 `(`括号`)` 里：`join MyCharacter (MyPortrait)`

要使用的立绘。如果没有指定，`Join` 模式会使用角色的默认立绘，而 `Update` 模式则不会改变立绘。

位于分组中的立绘必须用完整路径指定：`Group/SubGroup/Portrait`

立绘设置也可以是 `{`花括号`}` 中的表达式，例如一个变量：`{PlayerPortrait}`。该表达式会被求值，并解释为一个字符串，其内容应当是立绘的名称/路径。

---

### 位置/变换设置

该设置决定角色在屏幕上的位置。角色加入时会被放进一个全新的 `PortraitContainer`，随后你可以用变换设置修改这个容器的位置、尺寸与旋转。有两种方式：

- 使用**预设「位置」**：你可以借用样式中定义的某个 `PortraitContainer` 的标识符，轻松复制它的变换。默认情况下 Dialogic 提供 5 个这样的预设位置，分别命名为 "leftmost"、"left"、"center"、"right"、"rightmost"。

- 使用**变换命令**：你可以更精细地控制容器的位置、尺寸与旋转。例如 `pos=x0.5y1 size=y400px rot=20`。

##### 使用预设位置

把容器移动到预设位置，只需写上该位置的名称即可。

在默认的 "5-Portraits" 样式图层中，定义的位置是 `leftmost`、`left`、`center`、`right`、`rightmost`。

> [!TIP]
> 如果你想要更多/其他预设位置，就需要创建一个自定义立绘图层。
> 然后你还得在 Settings>Portraits 中修改时间线编辑器里会推荐的位置名称。


##### 使用变换命令

共有三条变换命令：`pos=`（位置）、`size=` 和 `rot=`（旋转）。

**位置与尺寸**可以这样指定：`x0.5 y1`、`x100px y1%`
默认情况下，x 和 y 会被理解为相对于视口尺寸，也就是说 0.5 表示窗口宽度/高度的一半。

位置定义的是立绘的原点（通常是底部中心）。这意味着 `x0.5 y1` 会把立绘摆成这样：

![](media/character_event/portrait_centered.jpg) 

角色首次加入时，位置、尺寸与旋转会从找到的第一个立绘预设中复制过来。这意味着如果你只想沿 x 轴调整位置，通常一个简单的 `pos=x0.3` 就够了，其他值默认都是正确的。

**旋转**以角度为单位。立绘会绕其原点（通常是底部中心）旋转！

---

---

## 其他设置

在文本编辑器中，这些设置都以参数的形式存储在事件末尾的短代码里。

例如：`join MyCharacter center [animation="Bounce" length="1" mirrored="true" ]`

### 动画设置

**文本语法：** `animation="Bounce"`

动画设置用于指定要播放的动画。可选的动画会因模式不同而不同。

如果没有指定动画，`Join` 与 `Leave` 会回退到 Settings>Portraits 中可设置的默认值，而 `Update` 不会播放任何动画。

动画名称相当宽容，也就是说 `Fade Up`、`fade up in`、`fade In Up` 都会指向正确的动画（前提是这是个加入事件）。

你可以通过扩展为 Dialogic 添加动画：[创建扩展](./creating-extensions.md)

### 动画时长设置

**文本语法：** `length="2"` / `length="0.5"`

如果设置了动画，这里定义动画的时长（秒）。

### 动画等待设置

**文本语法：** `wait="true"` / `wait="false"`

如果设置了动画，这里决定是否要等动画播完再进入下一个事件。

### 动画重复设置

**文本语法：** `repeat="2"`

动画重复设置（仅 `Update` 模式可用）可以让动画重复播放多次。

---

### 淡入淡出设置

**文本语法：** `fade="Fade"`

淡入淡出设置（仅 `Update` 模式可用，且只在立绘发生变化时才有意义）定义用于从上一个立绘过渡到下一个立绘的交叉淡化动画。如果没有指定，就会回退到 Setting>Portraits 中可设置的默认值。

### 淡入淡出时长设置

**文本语法：** `fade_length="0.5"`

定义淡入淡出的时长（秒）。

---

### Z-Index 设置

**文本语法：** `z_index="2"`

z-index 用于给角色排序。它*并不*使用 Godot 的 z-index，而是手动对角色排序！

---

### 镜像设置

**文本语法：** `mirrored="true"`

允许镜像立绘。

---

### 移动设置

**文本语法：** `move_time="0.8"` / `move_trans="Elastic"` / `move_ease="In Out"`

在改变位置的 `Update` 事件中，你可以设置从旧位置补间到新位置所用的时间（秒）、过渡与缓动。

缓动：`In`、`Out`、`In Out`、`Out In`

过渡：`Linear`、`Sine`、`Quint`、`Quart`、`Expo`、`Elastic`、`Cubic`、`Circ`、`Bounce`、`Back`、`Spring`

想要更直观的效果，可以看看这份[缓动速查表](https://raw.githubusercontent.com/godotengine/godot-docs/master/img/tween_cheatsheet.webp)。

---

### 额外数据设置

**文本语法：** `extra_data="something"`

额外数据会传给立绘场景，让你可以直接从时间线与立绘场景通信。它可以在[自定义立绘](./custom-portraits.md)的 `_set_extra_data(data:String)` 方法中读取。
