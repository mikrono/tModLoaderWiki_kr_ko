This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here.

# v0.12 (1.4 alpha)
v0.12 updates tModLoader to Terraria 1.4. This update changed everything. The first thing you need to do is copy your mod sources into the `ModLoader/Beta/Mod Sources/` folder. Next, visit the mod sources menu in game and click on the upgrade .csproj button. After that, you are ready to open Visual Studio and begin working on updating your code. Here are the most relevant changes.

## Renamed or Moved Members

### Namespaces / Classes
* `Terraria.World.Generation` -> `Terraria.WorldBuilding`
* `Terraria.ItemText` -> `Terraria.PopupText`

### Static Methods
* `Terraria.Main.NPCAddHeight(int)` -> `Terraria.Main.NPCAddHeight(NPC)`
* `Terraria.Main.PlaySound` -> `Terraria.Audio.SoundEngine.PlaySound`
* `Terraria.ItemText.NewText(Item, ...)` -> `Terraria.PopupText.NewText(PopupTextContext, Item, ...)`
* `Terraria.Lighting.BlackOut` -> `Terraria.Lighting.Clear`
* `Terraria.NetMessage.BroadcastChatMessage` -> `Terraria.Chat.ChatHelper.BroadcastChatMessage`

### Static Fields / Constants / Properties
* `Terraria.Main.font*` -> `Terraria.GameContent.FontAssets.*.Value`<br/>
**Regex:** `Main.font(\w+)]` -> `Terraria.GameContent.FontAssets.$1.Value`
* `Terraria.Main.*Texture[i]` -> `Terraria.GameContent.TextureAssets.*[i].Value`<br/>
**Regex for items:** `Main.itemTexture\[([^\]]*)\]` -> `Terraria.GameContent.TextureAssets.Item[$1].Value`
* `Terraria.Main.itemLockoutTime` -> `Main.timeItemSlotCannotBeReusedFor`
* `Terraria.Main.quickBG` -> `Main.instantBGTransitionCounter`
* `NPCID.Sets.TechnicallyABoss` -> `NPCID.Sets.ShouldBeCountedAsBoss`
* `ProjectileID.Sets.Homing` -> `ProjectileID.Sets.CountsAsHoming`
* `Terraria.Localization.GameCulture.*` -> `Terraria.Localization.GameCulture.CultureName.*`  
**Regex for replacing ModTranslation.AddTranslation uses:** `\bGameCulture\.([^,]+)+` -> `GameCulture.FromCultureName(GameCulture.CultureName.$1)`
* `Terraria.Main.maxInventory` -> `Terraria.Main.InventorySlotsTotal`
* `Terraria.ID.ItemUseStyleID` has a few renamed fields (`HoldingOut` becomes `Shoot`) and alot more use styles to choose from
* `Terraria.Main.campfire` and similar environmental flags are now in `Terraria.Main.SceneMetrics` and slightly renamed, e.g. `Terraria.Main.SceneMetrics.HasCampfire`

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
* `Player.extraAccessorySlots` -> `Player.GetAmountOfExtraAccessorySlotsToShow()`;

### tModLoader changes
_All ModX things listed here apply to GlobalX aswell_
* `ModX.mod`, `GlobalX.mod`, and all other lowercase properties (e.g. `ModPlayer.player`) -> `ModX.Mod`, `GlobalX.Mod`, `ModPlayer.Player`
* `Terraria.ModLoader.ModWorld` -> `Terraria.ModLoader.ModSystem` (With some additions from `Mod`. `ModWorld.Load/Save/Initialize` have been changed to accomodate for the world context)
* `Terraria.ModLoader.PlayerHooks` -> `Terraria.ModLoader.PlayerLoader`
* `Terraria.ModLoader.ModHotKey` -> `Terraria.ModLoader.ModKeybind`
* `Terraria.ModLoader.NPCSpawnHelper` -> `Terraria.ModLoader.Utilities.NPCSpawnHelper` (This mainly affects `SpawnConditions`)
* `Terraria.ModLoader.RecipeGroupHelper` -> `Terraria.ModLoader.Utilities.RecipeGroupHelper`
* `Terraria.ModLoader.PlayerDrawInfo` -> `Terraria.DataStructures.PlayerDrawSet`
* `Terraria.ModLoader.ModContent.TextureExists(string)` ->`Terraria.ModLoader.ModContent.HasAsset(string)`
* `Terraria.ModLoader.ModContent.GetTexture(string)` ->`Terraria.ModLoader.ModContent.Request<Texture2D>(string)`, similar for other assets like `Effect`  
**Regex:** `ModContent\.GetTexture\(([^)]+).` -> `ModContent.Request<Texture2D>($1)`
* `Terraria.ModLoader.Mod.GetTexture(string)` -> `Terraria.ModLoader.Mod.Assets.Request<Texture2D>(string)`, similar for other assets like `Effect`  
**Regex:** `mod\.GetTexture\(([^)]+).` -> `Mod.Assets.Request<Texture2D>($1).Value`
* `Terraria.ModLoader.Mod.RegisterKeybind(string, string)` -> `Terraria.ModLoader.KeybindLoader.RegisterKeybind(Mod, string, string)`
* `Terraria.ModLoader.Mod.CreateTranslation(string)` -> `Terraria.ModLoader.LocalizationLoader.CreateTranslation(Mod, string)`
* `Terraria.ModLoader.Mod.AddTranslation(ModTranslation)` -> `Terraria.ModLoader.LocalizationLoader.AddTranslation(ModTranslation)`
* `Terraria.ModLoader.ModPlayer.DrawEffects(PlayerDrawInfo, ...)` -> `Terraria.ModLoader.ModPlayer.DrawEffects(PlayerDrawSet, ...)`
* `Terraria.ModLoader.GetMod(string)` now throws if the mod is not loaded, use `Terraria.ModLoader.TryGetMod(string, out Mod)`
* `Terraria.ModLoader.ModProjectile.PreDraw(SpriteBatch, Color)` -> `Terraria.ModLoader.ModProjectile.PreDraw(ref Color)`, `Terraria.ModLoader.ModProjectile.PostDraw(SpriteBatch, Color)` -> `Terraria.ModLoader.ModProjectile.PostDraw(Color)`, and `PreDrawExtras(SpriteBatch)` -> `PreDrawExtras()`, so use `Main.EntitySpriteDraw` instead of `spriteBatch.Draw` (using the same parameters (except the last one is float -> int, which should stay at 0)).
* `Terraria.ModLoader.ModNPC.PreDraw(SpriteBatch, Color)` -> `Terraria.ModLoader.ModNPC.PreDraw(SpriteBatch, Vector2, Color)` and `Terraria.ModLoader.ModNPC.PostDraw(SpriteBatch, Color)` -> `Terraria.ModLoader.ModNPC.PostDraw(SpriteBatch, Vector2,Color)`, this means you should use the new parameter instead of `Main.screenPosition` so things draw correctly in the bestiary.
* `Terraria.ModLoader.ModItem.Clone` -> `Terraria.ModLoader.ModItem.Clone(Item)`
* `Terraria.ModLoader.ModItem.NetRecieve` -> `Terraria.ModLoader.ModItem.NetReceive` (typo)
* `Terraria.ModLoader.ModItem.NewPreReforge` -> `Terraria.ModLoader.ModItem.PreReforge`
* `Terraria.ModLoader.ModItem.UseStyle(Player)` -> `Terraria.ModLoader.ModItem.UseStyle(Player, Rectangle)`
* `Terraria.ModLoader.ModPlayer/ModItem.ModifyWeaponKnockback/ModifyWeaponDamage` now use `ref StatModifier` instead of `ref float/int`s.
* `Terraria.ModLoader.ModTile.DrawEffects(int, int, SpriteBatch, ref Color, ref int)` -> `Terraria.ModLoader.ModTile.DrawEffects(int, int, SpriteBatch, ref TileDrawInfo)`
* `Terraria.ModLoader.ModTile.NewRightClick` -> `Terraria.ModLoader.ModTile.RightClick`
* `Terraria.ModLoader.ModPrefix.GetPrefix(byte)` -> `Terraria.ModLoader.PrefixLoader.GetPrefix(int)`
* //TODO Shoot hook things

## Big change concepts

### Assets
Every asset is now wrapped inside an `Asset<T>`. You'll need to use `.Value` to access the actual asset. For example, instead of `Texture2D test = ModContent.GetTexture("Test");`, you would write `Texture2D test = ModContent.Request<Texture2D>("Test").Value;` (The `Mod` method is `Mod.Assets.Request<Texture2D>("Test")`). You could also technically do `Texture2D test = (Texture2D)GetTexture("Test");`, which, depending on your style, might be easier to look at. It does the exact same thing as `.Value`, which is load the texture.
In addition to that, tModLoader by default loads textures asynchronously. This means that upon requesting an asset for the first time, the associated value might not be assigned yet. This is usually not a problem (for textures, tModLoader supplies a dummy texture until the real asset is loaded), but it can be for UI things that need texture dimensions on construction (such as `UIImage`). Then, specify `AssetRequestMode.ImmediateLoad` as the second parameter in `Request<T>`.

Texture/Asset paths are now also slightly changed, so any use of something like this: `override string Texture => "Terraria/Item_" + ItemID.IronPickaxe;`, will have to be changed to this: `override string Texture => "Terraria/Images/Item_" + ItemID.IronPickaxe;`

Finally, when summoning vanilla textures, make sure to call the right variant of "Main.instance.LoadItem(type);" before using it in cases such as "TextureAssets.Item[type].Value" to avoid null errors.

### Recipes
Recipes were totally reworked (don't panic, read below). Instead of creating a `ModRecipe` (now just `Recipe`), and calling methods on that, recipes can now use builder syntax. If you don't know what that is, here's an example of what it looked like before:
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
There is a more detailed explanation of how to do this in `ExampleMod/ExampleRecipes.cs`. _Keep in mind that chaining methods is optional, you can still use the old pattern._

### Damage Classes
`Item.melee`, `Projectile.ranged` etc. are replaced by tModLoaders own `DamageClass` implementation. This means `item.ranged = true` turns into `Item.DamageType = DamageClass.Ranged;`, and `if (item.ranged)` turns into `if (Item.CountsAsClass(DamageClass.Ranged))`. You can also make your own custom classes through this system. For more information, visit `ExampleMod/Content/DamageClasses/ExampleDamageClass.cs`, and its items and projectiles in general.

Minion and sentry projectiles will have to have `Projectile.DamageType = DamageClass.Summon;` (in case of minions, in addition to `Projectile.minion = true;`).

### Equip Textures
1.4 includes support for a new streamlined armor texture format. This affects `EquipType.HandsOn/HandsOff/Body`. Tools to help porting:
* [Sprite Transformer](https://forums.terraria.org/index.php?threads/sprite-transformer-quickly-transform-body-sprite-sheets-from-1-3-to-1-4.96210/) (Note: The arm and shoulder related frames will most likely be wrong, you have to manually adjust them for now)
* [Sheet reference](https://cdn.discordapp.com/attachments/176975207800504321/852404448847986718/armor-template-3.png)

### Tiles
TODO mention all the method -> property renames as [per PR](https://github.com/tModLoader/tModLoader/pull/1301) (`Tile.active()` -> `Tile.IsActive`, `Tile.nactive()` -> `Tile.IsActiveUnactuated` etc.)

## Hook Changes
* `(ModItem/GlobalItem).UseTimeMultiplier`: Now accepts an actual multiplier instead of divisor. Invert your values by dividing 1.0 by them.
* `(ModItem/GlobalItem).MeleeTimeMultiplier`: Replaced with `UseAnimationMultiplier` and `UseSpeedMultiplier`. Use the former if you want to increase `itemAnimation` value (risking increasing the amount of uses/shots), use the latter if you just want to safely speed up the item/weapon while keeping the ratio between `itemAnimation` and `itemTime` the same.
* `(GlobalTile/GlobalWall/ModTile/ModWall/ModBuff/ModDust/ModMount/ModPrefix).SetDefaults` -> `(X).SetStaticDefaults`. Now all `ModType`s have such a method.

## tModLoader .NET Upgrade
{Some info on .NET5 and AnyCPU targetting}
### .NET 5 Install and Visual Studio Setup
{}
### .NET 5 Tutorial Links
{}

## Terraria 1.4+ vanilla changes
{1.4 includes several back end changes that we should point out}
### IProjectileSource
With 1.4.2, the `Projectile.NewProjectile` method has additional required parameter at the beginning, denoting the source of the projectile.
{More info on Projectile Sources}
### Minion spawning
Summon damage (minions, sentries, and minion/sentry-shot projectiles) now scales dynamically instead of fixed on spawn. Modders now have to manually assign `Projectile.originalDamage` to the damage the projectile spawns with AFTER it is created (NOT in `SetDefaults`, `Shoot` in the item that spawns it a suitable place). Here are the two most common approaches:
 
```cs
//1: Used mostly for sentries (in combination with `Player.FindSentryRestingSpot`)
int index = Projectile.NewProjectile(parameters);
Main.projectile[index].originalDamage = damage;

//2: Sets originalDamage automatically, used mostly for minions
Player.SpawnMinionOnCursor(parameters);
```
### Bestiary
Each mod gets its own filter for the bestiary, by default a "?" icon. You can change it by providing a 30x30 `icon_small.png` in your root folder.

//TODO bestiary integration with custom preview images, animation, drop rules etc.

### Wings
Wing data is now assigned through an `ArmorIDs` set on load (`ModItem.SetStaticDefaults`) like follows: `ArmorIDs.Wing.Sets.Stats[Item.wingSlot] = new WingStats(wingTimeMax, speed, acceleration);` (Check other constructors for more fine-tuning). Only assign player.wingTimeMax or use `ModItem.HorizontalWingSpeeds` if you need to dynamically adjust those. Failure to add the former code will result in the player not moving horizontally while flying.

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