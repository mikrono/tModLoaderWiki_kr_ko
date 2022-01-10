This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here.

# v0.12 (1.4 alpha)
v0.12 updates tModLoader to Terraria 1.4. This update changed everything. The first thing you need to do is copy your mod sources into the `ModLoader/Beta/Mod Sources/` folder. Next, visit the mod sources menu in game and click on the upgrade .csproj button. After that, you are ready to open Visual Studio and begin working on updating your code. Here are the most relevant changes.

## Renamed or Moved Members

### Namespaces / Classes
* `World.Generation` -> `WorldBuilding`
* `ItemText` -> `PopupText`

### Static Methods
* `Main.NPCAddHeight(int)` -> `Main.NPCAddHeight(NPC)`
* `Main.PlaySound` -> `SoundEngine.PlaySound` -> in the `Terraria.Audio` namespace
* `ItemText.NewText(Item, ...)` -> `PopupText.NewText(PopupTextContext, Item, ...)`
* `Lighting.BlackOut` -> `Lighting.Clear`
* `NetMessage.BroadcastChatMessage` -> `Chat.ChatHelper.BroadcastChatMessage`
* `Main.PlaySoundInstance(SoundEffectInstance)` -> completely removed
```cs
//Example from a ModSound
SoundEffectInstance instance = sound.CreateInstance();
Main.PlaySoundInstance(instance);
return instance;

//Was changed to:
SoundEffectInstance instance = Sound.Value.CreateInstance();
instance.Play();
SoundInstanceGarbageCollector.Track(instance);
return instance;
```

### Static Fields / Constants / Properties
* `Main.font*` -> `GameContent.FontAssets.*.Value`<br/>
**Regex:** `Main.font(\w+)]` -> `GameContent.FontAssets.$1.Value`
* `Main.*Texture[i]` -> `GameContent.TextureAssets.*[i].Value`<br/>
**Regex for items:** `Main.itemTexture\[([^\]]*)\]` -> `GameContent.TextureAssets.Item[$1].Value`
* `Main.itemLockoutTime` -> `Main.timeItemSlotCannotBeReusedFor`
* `Main.dresserX/Y` -> `Main.interactedDresserTopLeftX/Y`
* `Main.quickBG` -> `Main.instantBGTransitionCounter`
* `Main.tileValue` -> `Main.tileOreFinderPriority`
* `NPCID.Sets.TechnicallyABoss` -> `NPCID.Sets.ShouldBeCountedAsBoss`
* `ProjectileID.Sets.Homing` -> `ProjectileID.Sets.CultistIsResistantTo`
* `Localization.GameCulture.*` -> `Localization.GameCulture.CultureName.*`  
**Regex for replacing ModTranslation.AddTranslation uses:** `\bGameCulture\.([^,]+)+` -> `GameCulture.FromCultureName(GameCulture.CultureName.$1)`
* `Main.maxInventory` -> `Main.InventorySlotsTotal`
* `ID.ItemUseStyleID` has a few renamed fields (`HoldingOut` becomes `Shoot`) and alot more use styles to choose from
* `Main.campfire` and similar environmental flags are now in `Main.SceneMetrics` and slightly renamed, e.g. `Main.SceneMetrics.HasCampfire`

### Non-Static Methods
* `Player.Spawn` -> `Player.Spawn(PlayerSpawnContext)`

### Non-Static Fields / Constants / Properties
* `Main.Rasterizer` is now static
* `UIElement.Id` -> `UIElement.UniqueId`<br/>
(changed from string to automatically assigned auto-incrementing int)
* `Player.hideVisual` -> `Player.hideVisibleAccessory`
* `Item.thrown` -> While normally removed in 1.4, it is reimplemented by tML through Damage Classes (detailed further below).
* `Item.prefix` -> `byte` to `int` (Many changes to related methods aswell)
* `Item.dye` -> `byte` to `int` (Many changes to related methods aswell)
* `Item.hairDye` -> `short` to `int` (Many changes to related methods aswell)
* `Item.owner` -> `Item.playerIndexTheItemIsReservedFor`
* `Player.showItemIcon` -> `Player.cursorItemIconEnabled`
* `Player.showItemIcon2` -> `Player.cursorItemIconID`
* `Player.showItemIconText` -> `Player.cursorItemIconText`
* `Player.ZoneHoly` -> `Player.ZoneHallow`
* `Player.doubleJumpCloud` and other jumps -> `Player.hasJumpOption_Cloud` etc.
* `Player.bee` and similar accessory flags that spawn projectiles -> `Player.honeyCombItem` etc. To check if they are enabled: `X != null && !X.IsAir`; To enable them: assign your own accessory to it.
* `Player.talkNPC = X;` -> `Player.SetTalkNPC(X);` (Changed by vanilla due to the bestiary).  Getting the value of `Player.talkNPC` was not changed, only setting it was.
* `Player.flyingPigChest = -1;` -> `Player.piggyBankProjTracker.Clear(); Player.voidLensChest.Clear();` (Changed by vanilla due to the new inventory access projectiles). Setting it is now done using the `Set` method on the respective tracker.
* `Player.extraAccessorySlots` -> `Player.GetAmountOfExtraAccessorySlotsToShow()`

### tModLoader changes
_All ModX things listed here apply to GlobalX aswell_
* All lowercase properties are now capitalized (e.g. `ModX.mod`, `ModProjectile.aiType`, and `ModPlayer.player` -> `ModX.Mod`, `ModProjectile.AIType`, `ModPlayer.Player`)
* `ModLoader.ModWorld` -> `ModLoader.ModSystem` (With some additions from `Mod`. `ModWorld.Load/Save/Initialize` have been changed to accomodate for the world context)
* `ModLoader.Mod.UpdateUI` -> `ModLoader.ModSystem.UpdateUI`
* `ModLoader.Mod.ModifyInterfaceLayers` -> `ModLoader.ModSystem.ModifyInterfaceLayers`
* `ModLoader.Mod.PostAddRecipes` -> `ModLOader.Modsystem.PostAddRecipes`
* `ModLoader.Mod.AddRecipes` -> `ModLOader.Modsystem.AddRecipes`
* `ModLoader.Mod.AddRecipesGroups` -> `ModLoader.Modsystem.AddRecipesGroups`
* `ModLoader.Mod.PostSetupContent` -> `ModLoader.ModSystem.PostSetupContent`
* `ModLoader.PlayerHooks` -> `ModLoader.PlayerLoader`
* `ModLoader.ModHotKey` -> `ModLoader.ModKeybind`
* `ModLoader.NPCSpawnHelper` -> `ModLoader.Utilities.NPCSpawnHelper` (This mainly affects `SpawnConditions`)
* `ModLoader.RecipeGroupHelper` -> `ModLoader.Utilities.RecipeGroupHelper`
* `ModLoader.PlayerDrawInfo` -> `DataStructures.PlayerDrawSet`
* `ModLoader.ModContent.TextureExists(string)` ->`ModLoader.ModContent.HasAsset(string)`
* `ModLoader.ModContent.GetTexture(string)` ->`ModLoader.ModContent.Request<Texture2D>(string)`, similar for other assets like `Effect`  
**Regex:** `ModContent\.GetTexture\(([^)]+).` -> `ModContent.Request<Texture2D>($1)`
* `ModLoader.Mod.GetTexture(string)` -> `ModLoader.Mod.Assets.Request<Texture2D>(string)`, similar for other assets like `Effect`  
**Regex:** `mod\.GetTexture\(([^)]+).` -> `Mod.Assets.Request<Texture2D>($1).Value`
* `ModLoader.Mod.GetMod(string)` now throws if the mod is not loaded, use `ModLoader.TryGetMod(string, out Mod)`
* `ModLoader.Mod.AddBossHeadTexture(string, int)` now returns `int` which is the head texture slot.
* `ModLoader.Mod.AddTranslation(ModTranslation)` -> `ModLoader.LocalizationLoader.AddTranslation(ModTranslation)`
* `ModLoader.Mod.CreateTranslation(string)` -> `ModLoader.LocalizationLoader.CreateTranslation(Mod, string)`
* `ModLoader.Mod.RegisterKeybind(string, string)` -> `ModLoader.KeybindLoader.RegisterKeybind(Mod, string, string)`
* `ModLoader.ModPlayer.CatchFish(Item, Item, int, int, int, int, int, ref int)` -> `ModLoader.ModPlayer.CatchFish(FishingAttempt, ref int, ref int, ref AdvancedPopupRequest, ref Vector2)`
* `ModLoader.ModPlayer.DrawEffects(PlayerDrawInfo, ...)` -> `ModLoader.ModPlayer.DrawEffects(PlayerDrawSet, ...)`
* `ModLoader.ModProjectile.PreDraw(SpriteBatch, Color)` -> `ModLoader.ModProjectile.PreDraw(ref Color)`, `ModLoader.ModProjectile.PostDraw(SpriteBatch, Color)` -> `ModLoader.ModProjectile.PostDraw(Color)`, and `PreDrawExtras(SpriteBatch)` -> `PreDrawExtras()`, so use `Main.EntitySpriteDraw` instead of `spriteBatch.Draw` (using the same parameters (except the last one is float -> int, which should stay at 0)).
* `ModLoader.ModNPC.PreDraw(SpriteBatch, Color)` -> `ModLoader.ModNPC.PreDraw(SpriteBatch, Vector2, Color)` and `ModLoader.ModNPC.PostDraw(SpriteBatch, Color)` -> `ModLoader.ModNPC.PostDraw(SpriteBatch, Vector2,Color)`, this means you should use the new parameter instead of `Main.screenPosition` so things draw correctly in the bestiary.
* `ModLoader.ModNPC.NPCLoot` -> `ModLoader.ModNPC.OnKill` (Drops will now have to be added in `ModifyNPCLoot`, see the [Bestiary](#Bestiary)<a name="Bestiary"></a> section)
* `ModLoader.ModItem.Clone` -> `ModLoader.ModItem.Clone(Item)`
* `ModLoader.ModItem.NetRecieve` -> `ModLoader.ModItem.NetReceive` (typo)
* `ModLoader.ModItem.NewPreReforge` -> `ModLoader.ModItem.PreReforge`
* `ModLoader.ModItem.UseStyle(Player)` -> `ModLoader.ModItem.UseStyle(Player, Rectangle)`
* `ModLoader.ModItem.DrawX` -> now use `ArmorIDs.X.Sets.Draw/Hide/etc[equipSlotID] = true` to specify these qualities of an equip texture.
* `ModLoader.ModPlayer/ModItem.ModifyWeaponKnockback/ModifyWeaponDamage` now use `ref StatModifier` instead of `ref float/int`s.
* `ModLoader.ModTile/ModWall.drop` -> `ModLoader.ModTile/ModWall.ItemDrop`
* `ModLoader.ModTile.DrawEffects(int, int, SpriteBatch, ref Color, ref int)` -> `ModLoader.ModTile.DrawEffects(int, int, SpriteBatch, ref TileDrawInfo)`
* `ModLoader.ModTile.NewRightClick` -> `ModLoader.ModTile.RightClick`
* `ModLoader.ModTile.disableSmartCursor` -> `TileID.Sets.DisableSmartCursor[Type]`
* `ModLoader.ModTile.disableSmartInteract` -> `TileID.Sets.DisableSmartInteract[Type]`
* `ModLoader.ModTile.dresser` -> `TileID.Sets.BasicDresser[Type]`
* `ModLoader.ModTile.sapling` -> `TileID.Sets.TreeSapling[Type]`
* `ModLoader.ModTile.torch` -> `TileID.Sets.Torch[Type]`
* `ModLoader.ModPrefix.GetPrefix(byte)` -> `ModLoader.PrefixLoader.GetPrefix(int)`
* `ModLoader.Mod.AddItem(string, ModItem)`, `ModLoader.Mod.AddProjectile(string, ModProjectile)` and other similar methods -> `ModLoader.Mod.AddContent(ILoadable)`
* `ModLoader.BuffLoader.CanBeCleared(int)` -> removed
* `ModLoader.ModBuff.CanBeCleared` -> `BuffID.Sets.NurseCannotRemoveDebuff[Type]`
* `ModLoader.ModBuff.LongerExpertDebuff` -> `BuffID.Sets.LongerExpertDebuff[Type]`
* `ModLoader.ModX.Load(TagCompound)` -> `ModLoader.ModX.LoadData(TagCompound)`
* `ModLoader.ModX.Save()` -> `ModLoader.ModX.SaveData(TagCompound)` - now returns `void`
* //TODO Shoot hook things


## Big change concepts

### Assets
Every asset is now wrapped inside an `Asset<T>`. You'll need to use `.Value` to access the actual asset. For example, instead of `Texture2D test = ModContent.GetTexture("Test");`, you would write `Texture2D test = ModContent.Request<Texture2D>("Test").Value;` (The `Mod` method is `Mod.Assets.Request<Texture2D>("Test")`). You could also technically do `Texture2D test = (Texture2D)GetTexture("Test");`, which, depending on your style, might be easier to look at. It does the exact same thing as `.Value`, which is load the texture.
In addition to that, tModLoader by default loads textures asynchronously. This means that upon requesting an asset for the first time, the associated value might not be assigned yet. This is usually not a problem (for textures, tModLoader supplies a dummy texture until the real asset is loaded), but it can be for UI things that need texture dimensions on construction (such as `UIImageButton`). Then, specify `AssetRequestMode.ImmediateLoad` as the second parameter in `Request<T>`.

Texture/Asset paths are now also slightly changed, so any use of something like this: `"Terraria/Item_" + ItemID.IronPickaxe;`, will have to be changed to this: `"Terraria/Images/Item_" + ItemID.IronPickaxe;`

Finally, when summoning vanilla textures, make sure to call the right variant of "Main.instance.LoadItem(type);" before using it in cases such as "TextureAssets.Item[type].Value" to avoid null errors.

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

### Damage Classes
`Item.melee`, `Projectile.ranged` etc. are replaced by tModLoaders own `DamageClass` implementation. This means `item.ranged = true` turns into `Item.DamageType = DamageClass.Ranged;`, and `if (item.ranged)` turns into `if (Item.CountsAsClass(DamageClass.Ranged))`. You can also make your own custom classes through this system. For more information, visit `ExampleMod/Content/DamageClasses/ExampleDamageClass.cs`, and its items and projectiles in general.

Minion and sentry projectiles will have to have `Projectile.DamageType = DamageClass.Summon;` (in case of minions, in addition to `Projectile.minion = true;`).

With the inclusion of throwing damage, thrown weapons/damage class bonuses will go from `thrown` to `Throwing` for example `Player.GetCritChance(DamageClass.Throwing)`.

Accessories giving damage bonuses are changed from `player.minionDamage += 0.1f;` to `Player.GetDamage(DamageClass.Summon) += 0.1f;`.

### Equip Textures
1.4 includes support for a new streamlined armor texture format. This affects `EquipType.HandsOn/HandsOff/Body`. Tools to help porting:
* [Sprite Transformer](https://forums.terraria.org/index.php?threads/sprite-transformer-quickly-transform-body-sprite-sheets-from-1-3-to-1-4.96210/) (Note: The arm and shoulder related frames will most likely be wrong, you have to manually adjust them for now)
* [Sheet reference](https://cdn.discordapp.com/attachments/176975207800504321/852404448847986718/armor-template-3.png)

### Tiles
TODO mention all the method -> property renames as [per PR](https://github.com/tModLoader/tModLoader/pull/1301) (`Tile.active()` -> `Tile.IsActive`, `Tile.nactive()` -> `Tile.IsActiveUnactuated` etc.)

### ModBiome and ModSceneEffect
ModSceneEffect now does the handling of choosing scene effects instead of separate hooks, so that tML can give proper attention to designated priorities. Notably, it has an IsSceneEffectActive return method, and Priority property associated to it. It should be derived directly when adding scene effects that were controlled by any of the following hooks, with the exemption of ModTypes that derive this class already such as ModBiome. Multiple small, derived classes may be required to accomplish the same functionality. This change does not affect existing ModNPC music implementations at time of writing.

The following changes have been made with respect to ChooseStyle Hooks, with possibly multiple ModSceneEffect classes being required to fully replace the Hook:

 - ModSystem.ChooseWaterStyle Hook -> ModSceneEffect.WaterStyle Property
 - ModSystem.ChooseMusic Hook -> ModSceneEffect.Music Property
 - ModSurfaceBgStyle.ChooseStyle Hook -> ModSceneEffect.SurfaceBackgroundStyle Property
 - ModUgBgStyle.ChooseStyle Hook -> ModSceneEffect.UndergroundBackgroundStyle Property

Likewise, with the introduction of ModBiome, the UpdateBiomes and UpdateBiomeVisuals hooks have been integrated in to the ModBiome class. 
 - ModPlayer.UpdateBiomes -> ModBiome.IsActive()
 - ModPlayer.UpdateBiomeVisuals -> ModBiome.UpdateBiomeVisuals()

## Hook Changes
* `(ModItem/GlobalItem).UseTimeMultiplier`: Now accepts an actual multiplier instead of divisor. Invert your values by dividing 1.0 by them.
* `(ModItem/GlobalItem).MeleeTimeMultiplier`: Replaced with `UseAnimationMultiplier` and `UseSpeedMultiplier`. Use the former if you want to increase `itemAnimation` value (risking increasing the amount of uses/shots), use the latter if you just want to safely speed up the item/weapon while keeping the ratio between `itemAnimation` and `itemTime` the same.
* `(GlobalTile/GlobalWall/ModTile/ModWall/ModBuff/ModDust/ModMount/ModPrefix).SetDefaults` -> `(X).SetStaticDefaults`. Now all `ModType`s have such a method.
* ConsumeAmmo changes:
    * Proper variable names (clarify `item` usage to specify `weapon` and `ammo`)
    * `ModPlayer.ConsumeAmmo` -> `ModPlayer.CanConsumeAmmo`
    * `(ModItem/GlobalItem).ConsumeAmmo` -> `(ModItem/GlobalItem).CanConsumeAmmo`, now only invoked on the weapon.
    * New hook for ammo: `(ModItem/GlobalItem).CanBeConsumedAsAmmo`.
    * `(ModItem/GlobalItem).OnConsumeAmmo`: now only invoked on the weapon.
    * New hook for ammo: `(ModItem/GlobalItem).OnConsumedAsAmmo`.

## tModLoader .NET Upgrade
{Some info on .NET5 and AnyCPU targetting}
### .NET 5 Install and Visual Studio Setup
{}
### .NET 5 Tutorial Links
{}

## Terraria 1.4+ vanilla changes
{1.4 includes several back end changes that we should point out}
### IProjectileSource
With 1.4.2, the `Projectile.NewProjectile` method has additional required parameter at the beginning, denoting the source of the projectile. This parameter is useful for maintaining proper compatibility with other mods, so you should make sure to use it correctly. Here are some common conditinos and the `IProjectileSource` to use:

* `ModItem.Shoot`: use the `source` passed into the method.    
* `NPC` spawning projectile in `ModNPC.AI`: use `NPC.GetProjectileSpawnSource()`    
* Spawning minions or pets in `ModBuff.Update`: use `player.GetProjectileSource_Buff(buffIndex)`    
* Spawning a projectile from another projectile: use `Projectile.GetProjectileSource_FromThis()`    
* An accessory spawning a projectile: use `Player.GetProjectileSource_Accessory(iteminstancehere)`

### Minion spawning
Summon damage (minions, sentries, and minion/sentry-shot projectiles) now scales dynamically instead of fixed on spawn. Modders now have to manually assign `Projectile.originalDamage` to the base damage (usually `Item.damage`) AFTER it is created (NOT in `SetDefaults`, `Shoot` in the item that spawns it a suitable place). Here are the two most common approaches:
 
```cs
//1: Used mostly for sentries (in combination with `Player.FindSentryRestingSpot`)
int index = Projectile.NewProjectile(parameters);
Main.projectile[index].originalDamage = Item.damage;

//2: Sets originalDamage automatically, used mostly for minions
Player.SpawnMinionOnCursor(parameters); //Make sure to pass Item.damage for the damage parameter
```

### Bestiary
Each mod gets its own filter for the bestiary, by default a "?" icon. You can change it by providing a 30x30 `icon_small.png` in your root folder.

To add drops to NPCs, you now have to use the `Mod/GlobalNPC.ModifyNPCLoot` (and `GlobalNPC.ModifyGlobalLoot`) hooks (instead of the 1.3 analog of `NPCLoot`, `OnKill`, which is for non-loot (e.g. marking a boss as defeated, spawning ores or projectiles)).

You can customize the bestiary entries using the `ModNPC.SetBestiary` hook, and the appearance of the NPC in the preview and full image by adding your data to `NPCID.Sets.NPCBestiaryDrawOffset`.

//TODO bestiary integration with custom preview images, animation, drop rules etc.

### Wings
Wing data is now assigned through an `ArmorIDs` set on load (`ModItem.SetStaticDefaults`) like follows: `ArmorIDs.Wing.Sets.Stats[Item.wingSlot] = new WingStats(wingTimeMax, speed, acceleration);` (Check other constructors for more fine-tuning). Only assign `player.wingTimeMax` or use `ModItem.HorizontalWingSpeeds` if you need to dynamically adjust those. Failure to add the former code will result in the player not moving horizontally while flying.

### Misc
* Flagging a boss as defeated will not have to be manually synced anymore (`MessageID.WorldData` will be sent after the `OnKill` hook), and it will also trigger a lantern night if you use this method:
`NPC.SetEventFlagCleared(ref myDownedBool, -1);`

* Other

# v0.11.7.5

## Reflection
As always, your reflection might have broken, so double check that. In particular, the `Texture2D` fields in the `UICommon` class, see [this commit](https://github.com/tModLoader/tModLoader/commit/57de6f9650aa4f05e36735a93683df0522e42422)

# v0.11.7 (Steam Release)
It is required to have a fresh Terraria v1.4 installation.
tModLoader from now on _must not_ be installed into the Terraria folder, but in a separate folder in case of manual installation.
No additional changes to mods should be required.

# v0.11.5
v0.11.5 introduced `ContentInstance`, a faster and simpler way to access IDs and instances of modded classes. 

### Do I need to Update
No, all mods should still work, but if you wish to publish on v0.11.5, you will have to update.

## Mod.XType
If you previously used the generic `Mod.XType<T>()` methods, you'll need to change. `mod.ItemType<ExampleBlock>()` and `this.ItemType<ExampleBlock>()` are now simply `ItemType<ExampleBlock>()`, provided you write `using static Terraria.ModLoader.ModContent;` at the top of each .cs file. If you don't, it will be `ModContent.ItemType<ExampleBlock>()`. You'll need to keep the `ModContent.XType` approach for method calls located in your `Mod` class because there will be a namespace conflict otherwise. If you are still using the string versions, you do not need to update, but the new approach is faster, and the generic approach is less error prone. [Examples](https://github.com/tModLoader/tModLoader/commit/4bb65940e7928af8b6bf8fee30f935d37683505c)

### Regex Auto-Fix
You can migrate your whole mod easily with the following Find and Replace command in Visual Studio. Make sure `Use Regular Expressions` is enabled.    

Find: `(this|mod)\.(.*)Type<(.*)>\(\)`    
Replace: `ModContent.$2Type<$3>()`    
Example:    
![](https://i.imgur.com/DGycKCu.png)     

If you'd prefer the `using static Terraria.ModLoader.ModContent;` approach, do the following Find and Replace commands instead. These also require that `Use Regular Expressions` is enabled. You'll need to fix the calls in your Mod class to use `ModContent.XType<>()` manually:    

Find: `using Terraria.ModLoader;`    
Replace: `using Terraria.ModLoader;\nusing static Terraria.ModLoader.ModContent;`    
Find: `mod\.(.*)Type<(.*)>\(\)`    
Replace: `$1Type<$2>()`    

## Mod.GetModX and Instance fields
Similar to above, generic `Mod.GetModX<T>()` methods such as `GetModWorld` are now no longer invoked from an instance of the `Mod` class. Replace `mod.GetModWorld<ExampleWorld>();` with `GetInstance<ExampleWorld>();`, provided you write `using static Terraria.ModLoader.ModContent;` at the top of each .cs file. If you don't, it will be `ModContent.GetInstance<ExampleWorld>()`. Also, static instance variables are no longer recommended. Remove things like `public static ExampleConfigServer Instance;` and simply call `GetInstance<ExampleConfigServer>()` instead. Another example: `ExampleMod.Instance` -> `GetInstance<ExampleMod>()`. [Examples](https://github.com/tModLoader/tModLoader/commit/4bb65940e7928af8b6bf8fee30f935d37683505c)

## Player.GetModPlayer<T>(Mod)
Syntax such as `player.GetModPlayer<ExamplePlayer>(mod)` is now obsolete, use `Player.GetModPlayer<T>()` aka remove the mod instance from the backets, like this: `player.GetModPlayer<ExamplePlayer>()`.

## ModTile.RightClick
`ModTile.RightClick` is now `ModTile.NewRightClick`. `NewRightClick` returns a bool indicating if an interaction has occurred, preventing weapon usage. To migrate, replace `public override void RightClick(int i, int j)` with `public override bool NewRightClick(int i, int j)` and add `return true;` if an interaction has occurred. [Examples](https://github.com/tModLoader/tModLoader/commit/e47dcfb91811a5d1df96ec9d3d598e828adac577)

## Iterating over Buffs
v0.11.5 adds `Player.MaxBuffs`. If you previously iterated over buffs using `for (int n = 0; n < 22; n++)`, you'll want to update the code to `for (int n = 0; n < Player.MaxBuffs; n++)`.

## Reflection
As always, your reflection might have broken, so double check that. Many internal classes and fields have changed namespaces and identifiers.

For example, here are the changes needed for reflection into the load mods progress bar.
```cs
//var type = assembly.GetType("Terraria.ModLoader.Interface");
//FieldInfo loadModsField = type.GetField("loadModsProgress", BindingFlags.Static | BindingFlags.NonPublic);
//Type UILoadModsProgressType = assembly.GetType("Terraria.ModLoader.UI.DownloadManager.UILoadModsProgress");

var type = assembly.GetType("Terraria.ModLoader.UI.Interface");
FieldInfo loadModsField = type.GetField("loadMods", BindingFlags.Static | BindingFlags.NonPublic);
Type UILoadModsProgressType = assembly.GetType("Terraria.ModLoader.UI.UILoadMods");
```

# v0.11

v0.11 introduced many changes. Here is the [migration guide](https://docs.google.com/document/d/127Gmexzj533xMBC3ISmvWUz1yy6RZYjVjO8H2ZotwGM/edit?usp=sharing).

# v0.10

Here is the [migration guide](https://docs.google.com/document/d/1GY6Jyj0IkqfvQlXJUwXg60d2V8tIzumoNVgh5OWzGIc/edit?usp=sharing)