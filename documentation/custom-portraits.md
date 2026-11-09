<div class="header-banner pineapple">
     <div class="header-label pineapple">自定义立绘</div>
</div>

*本页介绍何时以及如何创建一个自定义场景，用于游戏中的一个、多个或全部立绘。*

## 📜 目录
[toc]

## 1. 简介

很多时候，你想要的东西比一张简单的图片更复杂：一张动画图片（例如 `AnimatedSprite`）、一个带骨骼且带动画的角色、一个只有单个身体但有许多脸部变体的角色，或者完全不同的其他东西。

要实现这些，你必须创建一个**自定义立绘场景**。

如果你想使用分层立绘，请看这里：[分层立绘](layered-portraits.md)

一个场景可以用于一个或多个立绘。如果一个场景被多个立绘使用，在它们之间切换时不会重新实例化该场景，只会在该场景上调用 `_update_portrait(portrait_name)`（详见下文）。这让你甚至可以从一个立绘动画过渡到下一个立绘。

---

## 2. 要求

立绘场景的要求并不多：

- **根节点必须是 Node2D 或 Control：**
  *根节点需要有 position 和 scale 属性，因此它必须是 Node2D 或 Control 节点（或其子类）。
  因为根节点会充当“轴心点”，所以建议使用像 Marker2D 这样的简单节点，并让所有功能节点（如 sprite、动画播放器等）作为它的子节点。*
- **根节点脚本继承 `DialogicPortrait`：**
  *根节点的脚本应继承 `DialogicPortrait` 类，并覆盖它的一些方法。*

---

## 3. 覆盖方法

你可以通过给根节点添加脚本并添加特定方法来定制行为。

与 `_ready` 或 `_process` 一样，这些方法会在需要时由 Dialogic 自动调用（前提是你实现了它们）：

- `_update_portrait(character: DialogicCharacter, portrait_name: String) -> void` \
该方法在立绘被实例化时调用，以及在使用同一场景切换到另一个立绘时调用。例如，你可以根据立绘名称在 `AnimatedSprite` 中播放特定动画，或显示特定图片。

- `_set_mirror(is_mirrored: bool) -> void` \
不同的场景可能希望用不同的方式镜像（AnimationSprite.flip_h；self.scale.x = -1 等）。因此，你需要自己实现这一功能。

- `_get_covered_rect() -> Rect2:` \
这用于正确计算场景的尺寸，因为通常无法得知立绘的大小。如果你实现它并返回一个覆盖立绘的 Rect2（相对于根节点的位置），那么立绘容器的尺寸模式和角色编辑器的 `Full View` 就能正常工作。

- `_set_extra_data(data: String) -> void` \
角色事件的 Join/Update 模式允许在文本编辑器中指定一些额外信息。如果提供了这些额外信息，就会调用该方法。这对于启用某些功能非常有用。例如，你的角色可以有不同模式或道具。以 `addons/dialogic/Example Assets/Portraits` 下的 `CustomPortraitFaceAtlas` 为例，要显示角色的外星人形态，你可以使用一个角色更新事件：`update Character [extra_data="alien"]`。

- `_should_do_portrait_update(character: DialogicCharacter, portrait: String) -> bool` \
很少需要，但如果覆盖了它，当要把立绘切换到使用同一场景的另一个立绘时，就会检查它。如果返回 false，那么这个实例会被删除，并创建一个新实例。如果未实现，它默认为 true，意味着这个实例也会被用于立绘更新。

- `_highlight() -> void` \
当该角色成为当前说话者时调用。

- `_unhighlight() -> void` \
当该角色不再是当前说话者时调用，例如另一个角色开始说话。

---

## 4. 导出覆盖

如果你给场景的脚本添加导出变量，就可以在角色编辑器的立绘设置中修改它们的值。
这让你可以为使用同一场景的不同立绘修改这些值。

```gdscript
@export_file var image := ""
@export var modulation := Color.WHITE
```

*这会为使用该场景的所有立绘添加 image 和 modulation 两项设置。*

如果你定义一个名为 `Main` 的导出分组，其中的所有设置都会显示在其他设置之上（其他设置归在 `Settings` 之下）：

![立绘设置](media/custom-portrait-settings.png)

---

## 5. 技巧与提示

在大多数立绘中，你应该在 `_update_portrait()` 方法的第一行调用 `apply_character_and_portrait()`。这个方法只是设置角色和立绘，并在立绘无效时把立绘字符串切换为默认立绘。

```gdscript
func _update_portrait(passed_character:DialogicCharacter, passed_portrait:String) -> void:
    apply_character_and_portrait(passed_character, passed_portrait)
```

基于图片的简单场景（使用 `Sprite` 或 `TextureRect`）可以使用 `apply_texture` 方法，它接收用于显示的节点和一个图片路径。如果你这样做，立绘类还会自动为你处理 `_get_covered_rect()` 和 `_set_mirror()`。

```gdscript
func _update_portrait(passed_character:DialogicCharacter, passed_portrait:String) -> void:
    apply_character_and_portrait(passed_character, passed_portrait)
    
    # $Portrait 是一个 TextureRect 或 Sprite
    # image 是一个导出的字符串
    apply_texture($Portrait, image)
```

---

## 6. 示例

除了 `DefaultPortrait` 场景（位于 `addons/dialogic/Other`）之外，`addons/dialogic/Example Assets/Portraits` 中还有一些示例场景，它们实现了这些方法中的一部分或全部，以达到不同的效果。

如果你不确定如何实现一个可用的立绘场景，就去看看它们吧！
