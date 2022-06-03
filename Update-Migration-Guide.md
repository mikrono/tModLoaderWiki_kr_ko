This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here. For migration information from past versions, see [Update Migration Guide Previous Versions](Update-Migration-Guide-Previous-Versions)

# v2022.X (1.4 alpha)
v2022.X updates tModLoader to Terraria 1.4. This update changed everything. The first thing you need to do is copy your mod sources into the `My Games/Terraria/tModLoader/ModSources/` folder. Next, visit the mod sources menu in game and click on the upgrade .csproj button. After that, you are ready to open Visual Studio and begin working on updating your code. Here are the most relevant changes.

## Renamed or Moved Members

### Namespaces / Classes
* `World.Generation` -> `WorldBuilding`
* `ItemText` -> `PopupText`

### Static Methods
* `ItemText.NewText(Item, ...)` -> `PopupText.NewText(PopupTextContext, Item, ...)`
* `GameContent.Generation.WorldGenLegacyMethod(GenerationProgress)` -> , `GameContent.Generation.WorldGenLegacyMethod(GenerationProgress, GameConfiguration)` (This affects the `GameContent.Generation.PassLegacy` constructor) (and do note the changed namespace of `GenerationProgress`)
* `Lighting.BlackOut` -> `Lighting.Clear`
* `Main.NewText` -> unchanged, but now you need a `Main.netMode != NetmodeID.Server` check whenever you use it (if it's not a client-only context), otherwise it will crash
* `Main.NPCAddHeight(int)` -> `Main.NPCAddHeight(NPC)`
* `Main.PlaySound` -> `SoundEngine.PlaySound` -> in the `Terraria.Audio` namespace
* `Main.IsTileSpelunkable(Tile)` -> `Main.IsTileSpelunkable(int, int)`
* `Main.PlaySoundInstance(SoundEffectInstance)` -> completely removed
* `TileDrawing.IsTileDangerous(Player, Tile, ushort)` -> `TileDrawing.IsTileDangerous(int, int, Player)`
* `NetMessage.BroadcastChatMessage` -> `Chat.ChatHelper.BroadcastChatMessage`

### Static Fields / Constants / Properties
* `ID.ItemUseStyleID` has renamed fields (`SwingThrow` -> `Swing` (1), `EatingUsing` -> `EatFood` (2), `Stabbing` -> `Thrust` (3), `HoldingUp` -> `HoldUp` (4), `HoldingOut` -> `Shoot` (5)) and alot more use styles to choose from
* `Main.ActivePlayersCount` -> `Main.CurrentFrameFlags.ActivePlayersCount`
* `Main.font*` -> `FontAssets.*.Value`<br/>
**Regex:** `Main.font(\w+)]` -> `FontAssets.$1.Value`
* `Main.*Texture[i]` -> `TextureAssets.*[i].Value`<br/>
**Regex for items:** `Main.itemTexture\[([^\]]*)\]` -> `TextureAssets.Item[$1].Value`
* `Main.campfire` and similar environmental flags are now in `Main.SceneMetrics` and slightly renamed, e.g. `Main.SceneMetrics.HasCampfire`
* `Main.dresserX/Y` -> `Main.interactedDresserTopLeftX/Y`
* `Main.GlobalTime` -> `Main.GlobalTimeWrappedHourly`
* `Main.itemLockoutTime` -> `Main.timeItemSlotCannotBeReusedFor`
* `Main.maxInventory` -> `Main.InventorySlotsTotal`
* `Main.quickBG` -> `Main.instantBGTransitionCounter`
* `Main.SmartCursorEnabled` -> `Main.SmartCursorIsUsed`
* `Main.tileValue` -> `Main.tileOreFinderPriority`
* `Main.worldRate` -> `Main.desiredWorldTilesUpdateRate`
* `Lighting.lightMode` -> `Graphics.Light.LegacyLighting.Mode` (Accessible via `Lighting.LegacyEngine.Mode`)
* `Localization.GameCulture.*` -> `Localization.GameCulture.CultureName.*`  
**Regex for replacing ModTranslation.AddTranslation uses:** `\bGameCulture\.([^,]+)+` -> `GameCulture.FromCultureName(GameCulture.CultureName.$1)`
* `NPCID.Sets.TechnicallyABoss` -> `NPCID.Sets.ShouldBeCountedAsBoss`
* `ProjectileID.Sets.Homing` -> `ProjectileID.Sets.CultistIsResistantTo`
* `Utils.InverseLerp` -> `Utils.GetLerpValue`

### Non-Static Methods
* `Player.GetItem(int, Item, bool, bool)` -> `Player.GetItem(int, Item, GetItemSettings)` (`GetItemSettings` class contains various static instances of it to use for the last parameter)
* `Player.Spawn` -> `Player.Spawn(PlayerSpawnContext)`
* `Player.SporeSac` -> `Player.SporeSac(Item)`
* `Item.IsTheSameAs` -> removed, use `item.type == compareItem.type` directly
* `Item.IsNotTheSameAs` -> `Item.IsNotSameTypePrefixAndStack`
* `Main.instance.DrawPlayer(...)` -> `Main.PlayerRenderer.DrawPlayer(Main.Camera, ...)`

### Non-Static Fields / Constants / Properties
* `Main.Rasterizer` is now static
* `UIElement.Id` -> `UIElement.UniqueId`<br/>
(changed from string to automatically assigned auto-incrementing int)
* `Player.hideVisual` -> `Player.hideVisibleAccessory`
* `Item.prefix` -> `byte` to `int` (Many changes to related methods aswell)
* `Item.dye` -> `byte` to `int` (Many changes to related methods aswell)
* `Item.hairDye` -> `short` to `int` (Many changes to related methods aswell)
* `Item.owner` -> `Item.playerIndexTheItemIsReservedFor`
* `Player.showItemIcon` -> `Player.cursorItemIconEnabled`
* `Player.showItemIcon2` -> `Player.cursorItemIconID`
* `Player.showItemIconText` -> `Player.cursorItemIconText`
* `Player.ZoneHoly` -> `Player.ZoneHallow`
* `Player.doubleJumpCloud` and other jumps -> `Player.hasJumpOption_Cloud` etc.
* `Player.dash` -> `Player.dashType`. Player.dash is used for something else now.
* `Player.bee` and similar accessory flags that spawn projectiles -> `Player.honeyCombItem` etc. To check if they are enabled: `X != null && !X.IsAir`; To enable them: assign your own accessory to it.
* `Player.talkNPC = X;` -> `Player.SetTalkNPC(X);` (Changed by vanilla due to the bestiary).  Getting the value of `Player.talkNPC` was not changed, only setting it was.
* `Player.flyingPigChest = -1;` -> `Player.piggyBankProjTracker.Clear(); Player.voidLensChest.Clear();` (Changed by vanilla due to the new inventory access projectiles). Setting it is now done using the `Set` method on the respective tracker.
* `Player.extraAccessorySlots` -> `Player.GetAmountOfExtraAccessorySlotsToShow()`

### Misc changes
* Shaders registered as `MiscShaderData` now require `float4 uShaderSpecificData;` as a parameter
* Armor shaders now require the `float2 uTargetPosition`, `float4 uLegacyArmorSourceRect` and `float2 uLegacyArmorSheetSize` parameters

### tModLoader changes
The following contains smaller scale changes to tModLoader members. More elaborate changes (i.e. things surrounding IEntitySource) are handled in separate categories below

_All ModX things listed here apply to GlobalX aswell_

* All lowercase properties are now capitalized (e.g. `ModX.mod`, `ModProjectile.aiType`, and `ModPlayer.player` -> `ModX.Mod`, `ModProjectile.AIType`, `ModPlayer.Player`)
* `ModLoader.ModWorld` -> `ModLoader.ModSystem` (With some additions from `Mod`. `ModWorld.Load/Save/Initialize` have been changed to accomodate for the world context)
* `ModLoader.Mod.UpdateUI` -> `ModLoader.ModSystem.UpdateUI`
* `ModLoader.Mod.ModifyInterfaceLayers` -> `ModLoader.ModSystem.ModifyInterfaceLayers`
* `ModLoader.Mod.PostAddRecipes` -> `ModLoader.Modsystem.PostAddRecipes`
* `ModLoader.Mod.AddRecipes` -> `ModLoader.Modsystem.AddRecipes`
* `ModLoader.Mod.AddRecipesGroups` -> `ModLoader.Modsystem.AddRecipesGroups`
* `ModLoader.Mod.PostSetupContent` -> `ModLoader.ModSystem.PostSetupContent`
* `ModLoader.Mod.HotKeyPressed` -> removed, use `ModLoader.ModPlayer.ProcessTriggers`
* `ModLoader.Mod.AddEquipTexture` -> removed, use `ModLoader.EquipLoader.AddEquipTexture` with Mod as the first parameter
* `ModLoader.Mod.GetEquipSlot` -> removed, use `ModLoader.EquipLoader.GetEquipSlot` with Mod as the first parameter
* `ModLoader.Mod.GetAccessorySlot` -> removed, use `ModLoader.EquipLoader.GetEquipSlot` with Mod as the first parameter, cast to `sbyte` if necessary
* `ModLoader.PlayerHooks` -> `ModLoader.PlayerLoader`
* `ModLoader.ModHotKey` -> `ModLoader.ModKeybind`
* `ModLoader.SpawnCondition` -> `ModLoader.Utilities.SpawnCondition`
* `ModLoader.PlayerDrawInfo` -> `DataStructures.PlayerDrawSet`
* `ModLoader.SoundType` -> removed, modded sounds are not categorized anymore
* `ModLoader.ModSound` -> removed
* `ModLoader.ModContent.TextureExists(string)` -> `ModLoader.ModContent.HasAsset(string)`
* `ModLoader.ModContent.GetTexture(string)` -> `ModLoader.ModContent.Request<Texture2D>(string)`, similar for other assets like `Effect`  
**Regex:** `ModContent\.GetTexture\(([^)]+).` -> `ModContent.Request<Texture2D>($1)`
* `ModLoader.Mod.GetTexture(string)` -> `ModLoader.Mod.Assets.Request<Texture2D>(string)`, similar for other assets like `Effect`  
**Regex:** `mod\.GetTexture\(([^)]+).` -> `Mod.Assets.Request<Texture2D>($1).Value`
* `ModLoader.ModContent.GetEffect(string)` -> `ModContent.Request<Effect>(string).Value` (Second parameter of `Request` has to be `ReLogic.Content.AssetRequestMode.ImmediateLoad`)
* `ModLoader.Mod.GetEffect(string)` -> `ModLoader.Mod.Assets.Request<Effect>(string).Value` (Second parameter of `Request` has to be `ReLogic.Content.AssetRequestMode.ImmediateLoad`)
* `ModLoader.Mod.GetMod(string)` now throws if the mod is not loaded, use `ModLoader.TryGetMod(string, out Mod)`
* `ModLoader.Mod.AddItem(string, ModItem)`, `ModLoader.Mod.AddProjectile(string, ModProjectile)` and other similar methods -> `ModLoader.Mod.AddContent(ILoadable)`, with the name now being specified through the `Name` property on the `ILoadable`
* `ModLoader.Mod.AddBossHeadTexture(string, int)` now returns `int` which is the head texture slot.
* `ModLoader.Mod.AddTranslation(ModTranslation)` -> `ModLoader.LocalizationLoader.AddTranslation(ModTranslation)`
* `ModLoader.Mod.CreateTranslation(string)` -> `ModLoader.LocalizationLoader.CreateTranslation(Mod, string)`
* `Mod.GetLegacySoundSlot(ModLoader.SoundType, string)` -> removed
* `ModLoader.Mod.RegisterHotKey(string, string)` -> `ModLoader.KeybindLoader.RegisterKeybind(Mod, string, string)`
* `ModLoader.ModPlayer.CatchFish(Item, Item, int, int, int, int, int, ref int)` -> `ModLoader.ModPlayer.CatchFish(FishingAttempt, ref int, ref int, ref AdvancedPopupRequest, ref Vector2)`
* `ModLoader.ModPlayer.DrawEffects(PlayerDrawInfo, ...)` -> `ModLoader.ModPlayer.DrawEffects(PlayerDrawSet, ...)`
* `ModLoader.ModProjectile.PreDraw(SpriteBatch, Color)` -> `ModLoader.ModProjectile.PreDraw(ref Color)`, `ModLoader.ModProjectile.PostDraw(SpriteBatch, Color)` -> `ModLoader.ModProjectile.PostDraw(Color)`, and `PreDrawExtras(SpriteBatch)` -> `PreDrawExtras()`, so use `Main.EntitySpriteDraw` instead of `spriteBatch.Draw` (using the same parameters (except the last one is float -> int, which should stay at 0)).
* `ModLoader.ModNPC.PreDraw(SpriteBatch, Color)` -> `ModLoader.ModNPC.PreDraw(SpriteBatch, Vector2, Color)` and `ModLoader.ModNPC.PostDraw(SpriteBatch, Color)` -> `ModLoader.ModNPC.PostDraw(SpriteBatch, Vector2,Color)`, this means you should use the new parameter instead of `Main.screenPosition` so things draw correctly in the bestiary.
* `ModLoader.ModNPC.NPCLoot` -> `ModLoader.ModNPC.OnKill` (Drops will now have to be added in `ModifyNPCLoot`, see the [Bestiary](#Bestiary)<a name="Bestiary"></a> section)
* `ModLoader.ModNPC.bossBag` -> removed, spawn the treasure bag alongside other loot via `npcLoot.Add(ItemDropRule.BossBag(type))`
* `ModLoader.ModItem.Clone` -> `ModLoader.ModItem.Clone(Item)`
* `ModLoader.ModItem.NetRecieve` -> `ModLoader.ModItem.NetReceive` (typo)
* `ModLoader.ModItem.NewPreReforge` -> `ModLoader.ModItem.PreReforge`
* `ModLoader.ModItem.UseItem` -> return type changed from `bool` to `bool?`
* `ModLoader.ModItem.UseStyle(Player)` -> `ModLoader.ModItem.UseStyle(Player, Rectangle)`
* `ModLoader.ModItem.DrawX` -> now use `ArmorIDs.X.Sets.Draw/Hide/etc[equipSlotID] = true` to specify these qualities of an equip texture.
* `ModLoader.ModItem.DrawHair` -> Removed. Porting: `drawAltHair = true` -> `ArmorIDs.Head.Sets.DrawHatHair[Item.headSlot] = true` (in `SetStaticDefaults`), for other uses check the other sets in `ArmorIDs.Head.Sets`
* `ModLoader.ModPlayer/ModItem.GetWeaponDamage` -> removed/deprecated
* `ModLoader.ModPlayer/ModItem.GetWeaponCrit(..., ref int)` -> `ModLoader.ModPlayer/ModItem.ModifyWeaponCrit(..., ref float)`
* `ModLoader.ModPlayer/ModItem.GetWeaponKnockback(..., ref bool)` -> `ModLoader.ModPlayer/ModItem.ModifyWeaponKnockback(..., ref StatModifier)`
* `ModLoader.ModPlayer/ModItem.ModifyWeaponDamage(..., float, float)` -> removed/deprecated
* `ModLoader.ModPlayer/ModItem.ModifyWeaponDamage(..., float, float, float)` -> `ModLoader.ModPlayer/ModItem.ModifyWeaponDamage(..., ref StatModifier)`
* `ModLoader.ModTile/ModWall.drop` -> `ModLoader.ModTile/ModWall.ItemDrop`
* `ModLoader.ModTile/ModWall.soundType` -> `ModLoader.ModTile/ModWall.HitSound`
* `ModLoader.ModTile/ModWall.soundStyle` -> removed/integrated into `HitSound`
* `ModLoader.ModTile.DrawEffects(int, int, SpriteBatch, ref Color, ref int)` -> `ModLoader.ModTile.DrawEffects(int, int, SpriteBatch, ref TileDrawInfo)`
* `ModLoader.ModTile.NewRightClick` -> `ModLoader.ModTile.RightClick`
* `ModLoader.ModTile.Dangersense` -> `ModLoader.ModTile.IsTileDangerous` (`GlobalTile` variant is `bool?` instead of `bool`)
* `ModLoader.ModTile.HasSmartInteract` -> `HasSmartInteract(int i, int j, SmartInteractScanSettings settings)` (`SmartInteractScanSettings` requires `using Terraria.GameContent.ObjectInteractions;`)
* `ModLoader.ModTile.disableSmartCursor` -> `TileID.Sets.DisableSmartCursor[Type]`
* `ModLoader.ModTile.disableSmartInteract` -> `TileID.Sets.DisableSmartInteract[Type]`
* `ModLoader.ModTile.dresser` -> `TileID.Sets.BasicDresser[Type]`
* `ModLoader.ModTile.sapling` -> `TileID.Sets.TreeSapling[Type]`
* `ModLoader.ModTile.torch` -> `TileID.Sets.Torch[Type]`
* `ModLoader.ModPrefix.GetPrefix(byte)` -> `ModLoader.PrefixLoader.GetPrefix(int)`
* `ModLoader.BuffLoader.CanBeCleared(int)` -> removed
* `ModLoader.ModBuff.canBeCleared` -> `BuffID.Sets.NurseCannotRemoveDebuff[Type]`
* `ModLoader.ModBuff.longerExpertDebuff` -> `BuffID.Sets.LongerExpertDebuff[Type]`
* `ModLoader.ModX.Load(TagCompound)` -> `ModLoader.ModX.LoadData(TagCompound)`
* `ModLoader.ModX.Save()` -> `ModLoader.ModX.SaveData(TagCompound)` - now returns `void`, this means you should be assigning your data to the passed in tag.


## Big change concepts

### Assets
Every asset (`Texture2D`, `DynamicSpriteFont`, `Effect`, etc.) is now wrapped inside an `Asset<T>`. You'll need to use `.Value` to access the actual asset. For example, instead of `Texture2D test = ModContent.GetTexture("Test");`, you would write `Texture2D test = ModContent.Request<Texture2D>("Test").Value;` (The `Mod` method is `Mod.Assets.Request<Texture2D>("Test")`). You could also technically do `Texture2D test = (Texture2D)GetTexture("Test");`, which, depending on your style, might be easier to look at. It does the exact same thing as `.Value`, which is load the texture.
In addition to that, tModLoader by default loads textures asynchronously. This means that upon requesting an asset for the first time, the associated value might not be assigned yet. This is usually not a problem (for textures, tModLoader supplies a dummy texture until the real asset is loaded), but it can be for UI things that need texture dimensions on construction (such as `UIImageButton`). Then, specify `AssetRequestMode.ImmediateLoad` as the second parameter in `Request<T>`.

Texture/Asset paths are now also slightly changed, so any use of something like this: `"Terraria/Item_" + ItemID.IronPickaxe;`, will have to be changed to this: `"Terraria/Images/Item_" + ItemID.IronPickaxe;`

Finally, when summoning vanilla textures, make sure to call the right variant of `Main.instance.LoadItem(type);` before using it in cases such as `TextureAssets.Item[type].Value` to avoid null errors.

Gores (`ModGore`) are now autoloaded from any folder that contains "Gores" in the path (instead of having to be in the "Gores" folder in the mod root folder).

### Sounds
Sounds are now greatly simplified, there is only one way to play a sound, and one object to represent a sound. See the updated [Basic Sounds](https://github.com/tModLoader/tModLoader/wiki/Basic-Sounds) guide for more info. The [Adapting Vanilla Code or Code From Past tModLoader Versions](https://github.com/tModLoader/tModLoader/wiki/Basic-Sounds#adapting-vanilla-code-or-code-from-past-tmodloader-versions) section in particular will teach you what to change. To migrate existing code, use `SoundEngine.PlaySound`, `SoundID` fields, and `new SoundStyle(pathtosoundwithoutextension)` as taught in the guide. For more info and a few examples, see the corresponding [Pull Request](https://github.com/tModLoader/tModLoader/commit/1f8670de56611833db64bb7082bbd2f17b9c0877).

### Recipes
Recipes were totally reworked (don't panic, read below). Instead of creating a `ModRecipe` (now just `Recipe`), and calling methods on that, recipes can now use fluent api syntax. If you don't know what that is, here's an example of what it looked like before:
```
// this would be in your item
public override void AddRecipes()
{
    var recipe = new ModRecipe(mod);
    recipe.AddIngredient(ItemID.Wood, 5);
    recipe.SetResult(this);
    recipe.AddRecipe();
}
```
This has been replaced with:
```
// still in your item
public override void AddRecipes()
{
    CreateRecipe()
        .AddIngredient(ItemID.Wood, 5)
        .Register();
}
```
You can even use an expression for this:
```
public override void AddRecipes() => CreateRecipe()
        .AddIngredient(ItemID.Wood, 5)
        .Register();
```
There is a more detailed explanation of how to do this in [ExampleMod/Content/ExampleRecipes.cs](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/ExampleRecipes.cs). _Keep in mind that chaining methods is optional, you can still use the old pattern._

#### RecipeFinder and RecipeEditor
The two classes were removed. Now just use any `PostAddRecipes` hook (such as in `ModSystem`) and manually iterate over `Main.recipe` to find and edit what you need.

#### Custom ModRecipe class/extension
As ModRecipe is gone, and Recipe is sealed, you cannot extend from it anymore. Any 1.3 patters that utilized custom classes and its methods will need to use the new methods.
Porting notes for methods (delegates are interchangeable with passing in a method directly):
* `ConsumeItem`: write your code as a method for `AddConsumeItemCallback`:
```cs
.AddConsumeItemCallback(delegate (Recipe recipe, int type, ref int amount) {
	//Code here
})
```

* `OnCraft`: write your code as a method for `AddCraftItemCallback`:
```cs
.AddOnCraftCallback(delegate (Recipe recipe, Item item) {
	//Code here
})
```

* `RecipeAvailable`: write your code as a method for `AddCondition` (You find more about it in ExampleRecipes, aswell as how to use vanilla conditions):
```cs
var conditionDescription = NetworkText.FromKey("Mods.MyMod.MyConditionKey"); //You are encouraged to localize your conditions properly (it would be <MyConditionKey: My Condition> in the localization file). If you don't want that, use NetworkText.FromLiteral("My Condition")
.AddCondition(conditionDescription , recipe => {
	//Code here
})
```

### Mod.XType, Content Fetching
String-based mod content fetching (such as `Mod.ItemType("ItemName")` or `Mod.ProjectileType("ProjectileName")`) have been replaced and unified under `Mod.Find<ModX>("XName").Type` (rarely`.Type` is `.Slot`). Instead of returning 0 if said content is not found, it will now throw an exception, encouraging use of the compile-time safe generic methods (which still exist, such as `ModContent.ItemType<ItemName>()`), or if using the generic method is not possible (such as cross mod), `Mod.TryFind<ModX>("XName", out var baseObj)` (and then using `baseObj.Type` if the method returned `true`).
The same methods also exist in `ModContent`, the string parameter then expects the format "ModName/XName" (which is by the way what `baseObj.FullName` would return).

**IMPORTANT: Do NOT replace/remove the `<ModX>` part!**

Other methods unified to the new approach:
* `Mod.GetGoreSlot` with `ModGore` (Important to note: this will error if called serverside, as `ModGore` is now a clientside type, so check for `Main.netMode != NetmodeID.Server`)

Examples:
```cs
// Replacement for recipe.AddIngredient(Mod.ItemType("ItemName")) assuming it exists in your mod, but will crash if said item was removed/renamed:
recipe.AddIngredient(Mod.Find<ModItem>("ItemName").Type);

// The same but safe:
if (Mod.TryFind<ModItem>("ItemName", out var modItemName)) {
	recipe.AddIngredient(modItemName.Type);
}

// The same but using a different mod (cross mod):
if (ModLoader.TryGetMod("OtherMod", out var otherMod)) {
	if (otherMod.TryFind<ModItem>("ItemName", out var modItemName)) {
		recipe.AddIngredient(modItemName.Type);
	}
}

// The same but shortering it by using ModContent:
if (ModContent.TryFind<ModItem>("OtherMod/ItemName", out var modItemName)) {
	recipe.AddIngredient(modItemName.Type);
}

// This would also work, first parameter split in two:
if (ModContent.TryFind<ModItem>("OtherMod", "ItemName", out var modItemName)) {
	recipe.AddIngredient(modItemName.Type);
}
```

### Damage Classes
`Item.melee`, `Projectile.ranged` etc. are replaced by tModLoaders own `DamageClass` implementation, which utilizes `StatModifier` for consolidated access to base/additive/multiplicative/flat bonuses. This means `item.ranged = true` turns into `Item.DamageType = DamageClass.Ranged;`, and `if (item.ranged)` turns into `if (Item.CountsAsClass(DamageClass.Ranged))`. You can also make your own custom classes through this system. For more information, visit `ExampleMod/Content/DamageClasses/ExampleDamageClass.cs`, and its items and projectiles in general.

Minion and sentry projectiles will have to have `Projectile.DamageType = DamageClass.Summon;` (in case of minions, in addition to `Projectile.minion = true;`).

With the inclusion of throwing damage, thrown weapons/damage class bonuses will go from `thrown` to `Throwing` for example `Player.GetCritChance(DamageClass.Throwing)`.

Accessories giving damage bonuses are changed from `player.minionDamage += 0.1f;` to `Player.GetDamage(DamageClass.Summon) += 0.1f;`.
Flat increases are done via `Player.GetDamage(DamageClass.Summon).Flat += 4;` for increased damage after applied multipliers, or `.Base += 4;` for before.

Use `Player.GetTotalDamage` to get the effective stat modifier for the damage class (including inheritance, like `Generic`)

Other porting changes:
* `Player.armorPenetration` -> `Player.GetArmorPenetration(DamageClass.Generic)`
* `Player.meleeSpeed` -> `GetAttackSpeed(DamageClass.Melee)`
* `Player.arrowDamage/bulletDamage/rocketDamage` -> Type change to `StatModifier`
* `Item.thrown` -> While normally removed in 1.4, it is reimplemented by tML through Damage Classes (detailed further below).

### XItem.Shoot
`ModPlayer/GlobalItem/ModItem.Shoot` has been split into three methods: `CanShoot`, `ModifyShootStats`, and `Shoot`. Now allowing shooting and changing shooting related parameters is separated from creation of the projectiles. `CanShoot` simply controls if the item can shoot projectiles or not. `ModifyShootStats` contains all the parameters as `ref`, while `Shoot` doesn't.
The `float speedX/Y` parameters have also been combined into `Vector2 velocity`. If you misuse `Shoot` for i.e. changing damage but not actually spawning any projectiles manually, then returning true, you need to switch (or move that code) to `ModifyShootStats`.

Small overview of the changes:
* `ModPlayer/GlobalItem/ModItem.Shoot` -> `ModPlayer/GlobalItem/ModItem.CanShoot` (for allowing or preventing spawning in projectiles)
* `ModPlayer/GlobalItem/ModItem.Shoot` -> `ModPlayer/GlobalItem/ModItem.ModifyShootStats` (for changing shooting related parameters)
* `ModPlayer/GlobalItem/ModItem.Shoot` -> `ModPlayer/GlobalItem/ModItem.Shoot` (for spawning projectiles and controlling if the default projectile spawns)

### Player Drawing
Modders previously using `PlayerLayer` and `ModPlayer.ModifyDrawLayers` will now have to adapt to the replacements: `PlayerDrawLayer`, `ModPlayer.HideDrawLayers`, and `ModPlayer.ModifyDrawLayerOrdering`.

Instead of instantiating custom `PlayerLayer` objects, and then adding them to a list of existing layers by using an index in the list, you can now use the self-contained `PlayerDrawLayer` class (use it just like any other `ModX` class), which specifies it's draw order using the `GetDefaultPosition` override by returning objects that define an **unconditional** order/hierarchy (`new Between(layer1, layer2)`, `new BeforeParent(layer)`, `new AfterParent(layer)` (`layer` usually being a vanilla layer found in the `PlayerDrawLayers` class). The `ModPlayer` hooks are not used for inserting anymore. To hide layers (previously done by setting Visible to false), you call the `Hide` method on them, which will also hide any children appended to this layer.

Small overview of the (equivalent) changes:
* `ModLoader.PlayerLayer` -> `ModLoader.PlayerDrawLayer` (for creating your layers)
* `ModLoader.PlayerLayer` -> `DataStructures.PlayerDrawLayers` (for referencing vanilla layers)
* Accessing all layers is done through `ModLoader.PlayerDrawLayerLoader.Layers` (not ordered by draw order!) instead of the `layers` param in `ModPlayer.ModifyDrawLayers`
* `ModLoader.ModPlayer.ModifyDrawLayers` -> `ModLoader.ModPlayer.HideDrawLayers` (_only_ for hiding (see the above to access all layers if you need to))
* `ModLoader.ModPlayer.ModifyDrawLayers` -> `ModLoader.ModPlayer.ModifyDrawLayerOrdering` (_only_ for changing the ordering)

Porting mostly entails moving your insertion code from `ModifyDrawLayers` to `GetDefaultPosition` (must be unconditional, if you need to change the order based on a condition, use the new hook), moving your immediate draw conditions into the `GetDefaultVisibility` hook, the actual draw code into `Draw`, and adding the created `DrawData` to `drawInfo.DrawDataCache`. See the example in ExampleMod for an implementation.

### Tiles
* The `Tile` type is no longer a by-reference `class`, but a `readonly struct` that acts as a key to data that is stored elsewhere, actually taking in mind the way computers' processors and memory planks work. Usage of the type for users remains somewhat similar.
* Memory usage has been heavily reduced. In the case of a Large world, 242/484 less megabytes of data will be used in 32-bit and 64-bit contexts accordingly.
* Performance has been increased _**up to 30%**_ in some cases.
* For the good of future developments, `LiquidType` is now a 6-bit integer, with a [0, 64] range.
* `Tile` is no longer nullable. All `tile == null` checks are useless and will always return false. `Tile tile = default;` will result in a key that points to the [0,0] tile, NOT a null-like value. If you need nullability - use `Tile?`, (C# short-hand for Nullable<Tile>.)
* You can add custom tile data by declaring a struct that implements the marker `ITileData` interface.
It will also be copied and removed alongside vanilla data. However, it is not automatically saved or synchronized.
Tile data structures must be `unmanaged`, that is, they can only contain value data.
Tile data can be accessed & modified via the `ref T Tile.Get<T>() where T : unmanaged, ITileData` method or via `TileMap.GetData<T>()`.
* Most of the tile get/set methods are now internal, replaced by properties for consistency and ease of use. tModPorter will handle all of them.
  * Except `isTheSameAs(Tile)`. This method is only used by vanilla for network compression, and probably doesn't do what you want. You should compare the fields you care about directly. If you're doing something like schematics, take a look at `Get<TileWallWireStateData>().NonFrameBits`

### ModBiome and ModSceneEffect
`ModSceneEffect` now does the handling of choosing scene effects instead of separate hooks, so that tML can give proper attention to designated priorities. Notably, it has an `IsSceneEffectActive` return method, and a `Priority` property associated to it. It should be derived directly when adding scene effects that were controlled by any of the following hooks, with the exemption of `ModType`s that derive this class already such as `
ModBiome`. Multiple small, derived classes may be required to accomplish the same functionality. This change does not affect existing ModNPC music implementations at the time of writing.

The following changes have been made with respect to `ChooseStyle` Hooks, with possibly multiple `ModSceneEffect` classes being required to fully replace the Hook (If not otherwise specified, `ModSceneEffect` also counts as `ModBiome`, as the latter inherits from the former):

* `ModWorld.ChooseWaterStyle()` -> `ModSceneEffect.WaterStyle`
* `Mod.UpdateMusic()` -> `ModSceneEffect.Music and .Priority`, aswell as `ModSceneEffect.IsSceneEffectActive()`
* `ModSurfaceBgStyle.ChooseStyle()` -> `ModSceneEffect.SurfaceBackgroundStyle`
* `ModUgBgStyle.ChooseStyle()` -> `ModSceneEffect.UndergroundBackgroundStyle`

Likewise, with the introduction of `ModBiome`, the `UpdateBiomes` and `UpdateBiomeVisuals` hooks have been integrated in to the `ModBiome` class. 
* `ModPlayer.UpdateBiomes()` -> `ModBiome.IsBiomeActive()`
* `ModPlayer.UpdateBiomeVisuals()` -> `ModSceneEffect.SpecialVisuals()`

## Hook Changes
* `(ModItem/GlobalItem).UseTimeMultiplier`: Now accepts an actual multiplier instead of divisor. Invert your values by dividing 1.0 by them.
* `(ModItem/GlobalItem).MeleeTimeMultiplier`: Replaced with `UseAnimationMultiplier` and `UseSpeedMultiplier`. Use the former if you want to increase `itemAnimation` value (risking increasing the amount of uses/shots), use the latter if you just want to safely speed up the item/weapon while keeping the ratio between `itemAnimation` and `itemTime` the same.
* `(GlobalTile/GlobalWall/ModTile/ModWall/ModBuff/ModDust/ModMount/ModPrefix).SetDefaults` -> `(X).SetStaticDefaults`. Now all `ModType`s have such a method.
* Ammo changes:
    * `(ModItem/GlobalItem).PickAmmo` changed `ref int damage` to `ref StatModifier damage`, with caveats. Read more on this [here](https://github.com/tModLoader/tModLoader/pull/2288)
    * Proper variable names + additional context for `ConsumeAmmo` (clarify `item` usage to specify `weapon` and `ammo`, and add each one missing as context)
    * `ModPlayer.ConsumeAmmo` -> `ModPlayer.CanConsumeAmmo`
    * `(ModItem/GlobalItem).ConsumeAmmo` -> `(ModItem/GlobalItem).CanConsumeAmmo`, now only invoked on the weapon.
    * New hook for ammo: `(ModItem/GlobalItem).CanBeConsumedAsAmmo`.
    * `(ModItem/GlobalItem).OnConsumeAmmo`: now only invoked on the weapon.
    * New hook for ammo: `(ModItem/GlobalItem).OnConsumedAsAmmo`.
    * New hooks for ammo **selection**: `(ModItem/GlobalItem).CanChooseAmmo` and `(ModItem/GlobalItem).CanBeChosenAsAmmo`
* `(ModNPC/GlobalNPC).OnCatchNPC`: Now has additional context and is no longer used to modify the item the caught NPC is turned into. to modify the caught NPC's item, use `OnSpawn` and check if the source `is EntitySource_CatchEntity`.

## tModLoader .NET Upgrade
{Some info on .NET5 and AnyCPU targetting}
### .NET 5 Install and Visual Studio Setup
{}
### .NET 5 Tutorial Links
{}

## Terraria 1.4+ vanilla changes
{1.4 includes several back end changes that we should point out}
### IEntitySource
Various entity creating methods have received a new parameter denoting its source, read more about it [here](https://github.com/tModLoader/tModLoader/wiki/IEntitySource).

### Equip Textures and Armor Sheets
The Sprite Styling has changed for some aspects of the armor - namely `EquipType.HandsOn/HandsOff/Body` (the latter affecting the 1.3 Arm and FemaleBody textures too). Tools and guides on migrating to the new armor texture format can be found in [Armor Texture Migration Guide](../wiki/Armor-Texture-Migration-Guide).

![](https://i.imgur.com/0iw2Tw2.png)

### Minion spawning
Summon damage (minions, sentries, and minion/sentry-shot projectiles) now scales dynamically instead of fixed on spawn. Modders now have to manually assign `Projectile.originalDamage` to the base damage (usually `Item.damage`) AFTER it is created (NOT in `SetDefaults`, `Shoot` in the item that spawns it is a suitable place). Here are the two most common approaches:
 
```cs
//1: Used mostly for sentries (in combination with `Player.FindSentryRestingSpot`)
int index = Projectile.NewProjectile(parameters);
Main.projectile[index].originalDamage = Item.damage;

//2: Sets originalDamage automatically, used mostly for minions
Player.SpawnMinionOnCursor(parameters); //Make sure to pass Item.damage for the damage parameter
```

### Bestiary
Each mod gets its own filter for the bestiary, by default a "?" icon. You can change it by providing a 30x30 `icon_small.png` in your root folder.

To add drops to NPCs, you now have to use the `Mod/GlobalNPC.ModifyNPCLoot` (and `GlobalNPC.ModifyGlobalLoot`) hooks (instead of the 1.3 analog of `NPCLoot`, `OnKill`, which is for non-loot e.g. marking a boss as defeated, spawning ores or projectiles)). Read more about it [here](https://github.com/tModLoader/tModLoader/wiki/Basic-NPC-Drops-and-Loot-1.4).

You can customize the bestiary entries using the `ModNPC.SetBestiary` hook, and the appearance of the NPC in the preview and full image by adding your data to `NPCID.Sets.NPCBestiaryDrawOffset`.

//TODO bestiary integration with custom preview images, animation, drop rules etc.

### NPC Buff Immunities
The old approach to setting buff immunities on NPCs does not work anymore (Reminder: in `SetDefaults`: `npc.buffImmune[type] = true;`).
The new approach moves it to `SetStaticDefaults` and is using new syntax. Example:

```cs
// Add these to the top of your file
using Terraria.DataStructures;
using Terraria.ID;

// Specify the debuffs it is immune to
NPCDebuffImmunityData debuffData = new NPCDebuffImmunityData {
	SpecificallyImmuneTo = new int[] {
		BuffID.Confused, // Most NPCs have this
		BuffID.Poisoned,
		ModContent.BuffType<MyDebuff>(), // Modded buffs
	}
};
NPCID.Sets.DebuffImmunitySets[Type] = debuffData;
```

If you want to give your NPC immunities to all debuffs (like The Destroyer), use this:
```cs
new NPCDebuffImmunityData {
	ImmuneToAllBuffsThatAreNotWhips = true,
	ImmuneToWhips = true
}
```

### Wings
Wing data is now assigned through an `ArmorIDs` set on load (`ModItem.SetStaticDefaults`) like follows: `ArmorIDs.Wing.Sets.Stats[Item.wingSlot] = new WingStats(wingTimeMax, speed, acceleration);` (Check other constructors for more fine-tuning). Only assign `player.wingTimeMax` or use `ModItem.HorizontalWingSpeeds` if you need to dynamically adjust those. Failure to add the former code will result in the player not moving horizontally while flying.

### Misc
* Flagging a boss as defeated will not have to be manually synced anymore (`MessageID.WorldData` will be sent after the `OnKill` hook), and it will also trigger a lantern night if you use this method:
`NPC.SetEventFlagCleared(ref myDownedBool, -1);`

* Other