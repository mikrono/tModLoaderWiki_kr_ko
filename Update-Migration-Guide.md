This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here.

# v0.12 (WIP, 0.12 isn't out yet, don't read this)
v0.12 updates tModLoader to Terraria 1.4. This update changed everything. Here are the most relevant changes.

## Renamed or Moved Members

### Namespaces
* `Terraria.World.Generation` -> `Terraria.WorldBuilding`

### Static Methods
* `Terraria.Main.PlaySound` -> `Terraria.Audio.SoundEngine.PlaySound`
* `Terraria.NetMessage.BroadcastChatMessage` -> `Terraria.Chat.ChatHelper.BroadcastChatMessage`

### Static Fields / Constants / Properties
* `Terraria.Main.font*` -> `Terraria.GameContent.FontAssets.*.Value`<br/>
**Regex:** `Main.font(\w+)]` -> `Terraria.GameContent.FontAssets.$1.Value`
* `Terraria.Main.*Texture[i]` -> `Terraria.GameContent.TextureAssets.*[i].Value`<br/>
**Regex for items:** `Main.itemTexture\[([^\]]*)\]` -> `Terraria.GameContent.TextureAssets.Item[$1].Value`
* `Terraria.Localization.GameCulture.*` -> `Terraria.Localization.GameCulture.CultureName.*`
* `Main.maxInventory` -> `Main.InventorySlotsTotal`
* `ItemUseStyleID` has a few renamed fields (`HoldingOut` becomes `Shoot`) and alot more use styles to choose from

### Non-Static Fields / Constants / Properties
* `UIElement.Id` -> `UIElement.UniqueId`<br/>
(changed from string to automatically assigned auto-incrementing int)
* `ModX.mod` -> `ModX.Mod`
* `GlobalX.mod` -> `GlobalX.Mod`
* `Player.hideVisual` -> `Player.hideVisibleAccessory`
* `Item.thrown` -> doesn't exist anymore.
* `Player.showItemIcon` -> `Player.cursorItemIconEnabled`
* `Player.showItemIcon2` -> `Player.cursorItemIconID`

## Big change concepts
Every asset is wrapped now inside an `Asset<T>`. You'll need to use `.Value` to access the actual asset. For example, instead of `Texture2D test = GetTexture("Test");`, you would write `Texture2D test = GetTexture("Test").Value;` You could also technically do `Texture2D test = (Texture2D)GetTexture("Test);`, which, depending on your style, might be easier to look at. It does the exact same thing as `.Value`, which is load the texture. 

Recipes were totally reworked. Instead of creating a `ModRecipe`, and calling methods on that, recipes now use builder syntax. If you don't know what that is, here's an example of what it looked like before:
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
There is a more detailed explanation of how to do this in ExampleMod/ExampleRecipes.cs

## tModLoader .NET Upgrade
{Some info on .NET5 and AnyCPU targetting}
### .NET 5 Install and Visual Studio Setup
{}
### .NET 5 Tutorial Links
{}

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