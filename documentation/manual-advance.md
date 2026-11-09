<div class="header-banner purple">
     <div class="header-label purple">手动前进</div>
</div>

*手动前进让你的玩家可以用映射到 `dialogic_default_action` 的输入来推进时间线（项目设置 → Input Map）。*

## 📜 目录
[toc]

## 1. 什么是手动前进？

手动前进是 Dialogic 的一项功能，让玩家*通过*输入来推进时间线！它默认是开启的：玩家点击一下，时间线就在事件组成的栈上往前走一步。

## 2. 通过代码修改手动前进

通过 Inputs 子系统可以很方便地访问 Dialogic 的手动前进类，从而在游戏运行期间修改手动前进。它还有一些未暴露在 Dialogic 界面上的设置。

所有这些功能都位于 `Dialogic.Inputs.Manual-Advance`，一个 `DialogicManualAdvance` 对象中。
我们可以限制玩家与时间线的交互，以便展示一段写好的时间线内容。

### 2.1 通过代码限制玩家输入
用脚本实现这个行为很直接。下面的代码片段展示了做法：

```gdscript
Dialogic.Inputs.auto_advance.disabled_until_next_event = true
```

在 Dialogic 中，手动前进与自动前进共享 `system` 这一概念：system 指功能本身，在这里就是手动前进。你可以阻止玩家推进时间线：

```gdscript
# 玩家，没有输入给你了。
Dialogic.Inputs.auto_advance.system_enabled = false
```

此外，你可以调用 `Dialogic.Inputs.auto_advance.is_enabled()` 方法来检查玩家是否还能手动推进。
