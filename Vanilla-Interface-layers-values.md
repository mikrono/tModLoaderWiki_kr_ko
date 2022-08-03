This is a list of the vanilla interface layers stored in the layers parameter in the Mod.ModifyInterfaceLayers(List<MethodSequenceListItem> layers), in the order that they appear.
In order to use those, write your code like this:
```cs
int mouseItemIndex = layers.FindIndex(layer => layer.Name.Equals("Vanilla: Mouse Item / NPC Head"));
if (mouseItemIndex != -1) layers.Insert(mouseItemIndex, ...);
```
(Descriptions to be filled)

| Interface layer	| Description
| :--             	| :--
| Vanilla: Interface Logic 1 | |
| Vanilla: MP Player Names | |
| Vanilla: Emote Bubbles | |
| Vanilla: Entity Markers | |
| Vanilla: Smart Cursor Targets | |
| Vanilla: Laser Ruler | |
| Vanilla: Ruler | |
| Vanilla: Gamepad Lock On | |
| Vanilla: Tile Grid Option | |
| Vanilla: Town NPC House Banners | |
| Vanilla: Hide UI Toggle | |
| Vanilla: Wire Selection | |
| Vanilla: Capture Manager Check | |
| Vanilla: Ingame Options | |
| Vanilla: Fancy UI | |
| Vanilla: Achievement Complete Popups | |
| Vanilla: Entity Health Bars | |
| Vanilla: Invasion Progress Bars | |
| Vanilla: Map / Minimap | |
| Vanilla: Diagnose Net | |
| Vanilla: Diagnose Video | |
| Vanilla: Sign Tile Bubble | |
| Vanilla: Hair Window | |
| Vanilla: Dresser Window | |
| Vanilla: NPC / Sign Dialog | |
| Vanilla: Interface Logic 2 | |
| Vanilla: Resource Bars | |
| Vanilla: Interface Logic 3 | |
| Vanilla: Inventory | |
| Vanilla: Info Accessories Bar | |
| Vanilla: Settings Button | |
| Vanilla: Hotbar | |
| Vanilla: Builder Accessories Bar | |
| Vanilla: Radial Hotbars | |
| Vanilla: Mouse Text | |
| Vanilla: Player Chat | |
| Vanilla: Death Text | |
| Vanilla: Cursor | |
| Vanilla: Debug Stuff | |
| Vanilla: Mouse Item / NPC Head | |
| Vanilla: Mouse Over | |
| Vanilla: Interact Item Icon | |
| Vanilla: Interface Logic 4 | |