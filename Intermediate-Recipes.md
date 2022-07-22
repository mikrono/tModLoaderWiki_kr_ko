***
This Guide has been updated to 1.4. If you need to view the old 1.3 version of this wiki page, click [here](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Recipes/c5e693d7cf96c9ff58167da11a5da30b759c427c)
***

# Prerequisites 
* [Basic Recipes](Basic-Recipes)

# Recipe Groups
Recipe groups allow you to create a recipe that accepts any item out of a list of specified ingredients. For example, all varieties of wood are in the vanilla "Wood" Group. Instead of creating 8 separate recipes to use each type of vanilla wood, we would create 1 recipe that uses the "Wood" recipe group. Mods can also add recipe groups to simplify their recipes.

## Using Existing Recipe Groups
Vanilla groups consist of: "Wood", "IronBar", "PresurePlate", "Sand", "Fragment", "Birds", "Scorpions", "Squirrels", "Bugs", "Ducks", "Butterflies", "Fireflies", "Snails", "Dragonflies", "Turtles", and "Fruit". To use a vanilla recipe group, simply add the following instead of a similar `Recipe.AddIngredient` line:
```csharp
// Option 1: The name of the group as a string
recipe.AddRecipeGroup("Wood", 5); // parameter 1 is the group name, parameter 2 is the item stack count
// Option 2: Use the RecipeGroupID value.
recipe.AddRecipeGroup(RecipeGroupID.IronBar, 10);
```
Note that, like with `Recipe.AddIngredient`, the parameter for providing an explicit stack count is optional and defaults to 1.

## New Recipe Groups
You may have noticed that aside from "Wood" and "IronBar", there aren't many useful vanilla recipe groups. Luckily, we can make our own recipe groups. Let's imagine we want to make a recipe that used a Magic Mirror or Ice Mirror as an ingredient. Without recipe groups, we would have to do this:
```csharp
Recipe recipe = CreateRecipe();
recipe.AddIngredient(ItemID.MagicMirror);
recipe.AddIngredient(ItemID.Gel, 10);
recipe.AddTile(TileID.Chairs);
recipe.AddTile(TileID.Tables);
recipe.Register();

recipe = CreateRecipe();
recipe.AddIngredient(ItemID.IceMirror);
recipe.AddIngredient(ItemID.Gel, 10);
recipe.AddTile(TileID.Chairs);
recipe.AddTile(TileID.Tables);
recipe.Register();
```
In this example, it isn't too bad, but imagine you have 10 or 20 similar items that you'd like to use for your recipe - it would get unmanageable very quickly. To solve this, first, we must add a new recipe group. We do this in a `ModSystem` class by overriding the `ModSystem.AddRecipeGroups` method:
```csharp
// The Terraria.Localization using directive (using Terraria.Localization;) is required for Language methods.
public override void AddRecipeGroups()
{
	RecipeGroup group = new RecipeGroup(() => $"{Language.GetTextValue("LegacyMisc.37")} {Lang.GetItemNameValue(ItemID.MagicMirror)}", ItemID.IceMirror, ItemID.MagicMirror);
	RecipeGroup.RegisterGroup(nameof(ItemID.MagicMirror), group);
}
```
As seen above, first we construct the group, then we call `RecipeGroup.RegisterGroup` with the desired name. As a convention, please use `"ModName:GroupName"` for recipe groups that mainly concern items or usages specific to your mod. If you are using vanilla items to make a group that could conceivably be used by other mods for the same purpose, use `nameof(ItemID.IconicItem)`, where `IconicItem` is the earliest item to be added to the game in the group (smallest ItemID value). For example, if you were adding a group for SilverBar and TungstenBar, use SilverBar because that was the first item added to the game. Doing this allows multiple mods to collaborate seamlessly. If Mod A adds a SilverBar goup, and Mod B adds a similar group but with a 3rd SilverBar variant, the game will seamlessly merge the recipegroups, allowing all recipes using the groups to allow all variants. 

As a note, `Language.GetTextValue("LegacyMisc.37")` is just the word "Any" in the selected language and requires the `Terraria.Localization` using directive at the top of your code. The syntax here is a little difficult for newcomers (as it makes use of [lambda expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)), so please follow it exactly.

As a note: use `ModContent.ItemType<YourModItem>()` instead of `ItemID.ItemName` for `ModItem`s.

Next, we need to use the `RecipeGroup`. We do this just like with vanilla recipe groups, except our recipe group's name will be different:
```csharp
recipe.AddRecipeGroup("ModName:GroupName");
```
## Complete Example
```csharp
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;
using Terraria.Localization;

namespace ExampleMod
{
	public class ExampleRecipes : ModSystem
	{
		public override void AddRecipeGroups()
		{
			RecipeGroup group = new RecipeGroup(() => $"{Language.GetTextValue("LegacyMisc.37")} {Lang.GetItemNameValue(ItemID.MagicMirror)}", ItemID.IceMirror, ItemID.MagicMirror);
			RecipeGroup.RegisterGroup(nameof(ItemID.MagicMirror), group);
		}

		public override void AddRecipes()
		{
			Recipe recipe = Recipe.Create(ModContent.ItemType<Items.MinionControlRod>());
			recipe.AddIngredient(ItemID.SlimeStaff);
			recipe.AddRecipeGroup(nameof(ItemID.MagicMirror));
			recipe.AddTile(TileID.Chairs);
			recipe.AddTile(TileID.Tables);
			recipe.Register();

			recipe = Recipe.Create(ModContent.ItemType<Items.SummonersAssociationCard>());
			recipe.AddIngredient(ItemID.Gel, 10);
			// Here, instead of adding 5 ItemID.Wood, we use a RecipeGroup to specify all types of Wood in a single recipe.
			recipe.AddRecipeGroup("Wood", 5);
			recipe.AddTile(TileID.Chairs);
			recipe.AddTile(TileID.Tables);
			recipe.Register();
		}
	}
}
```
Note that the name used in `Recipe.AddRecipeGroup` is identical to what we defined when registering our modded `RecipeGroup`.

## Editing Vanilla Recipe Groups
In some situations, you may want to add your own items to existing recipe groups. In this example, we will add another wood type to the "Wood" recipe group.
```csharp
public override void AddRecipeGroups()
{
	if (RecipeGroup.recipeGroupIDs.ContainsKey("Wood"))
	{
		int index = RecipeGroup.recipeGroupIDs["Wood"];
		RecipeGroup group = RecipeGroup.recipeGroups[index];
		group.ValidItems.Add(ModContent.ItemType<MyWood>());
	}
}
```
Note that checking `if (RecipeGroup.recipeGroupIDs.ContainsKey(...))` is not *necessary*, but it will prevent errors if some other mod completely removes that recipe group for one reason or another. This can also be used for [cross-mod compatibility](https://github.com/tModLoader/tModLoader/wiki/Expert-Cross-Mod-Content).

# Editing Recipes
We can edit vanilla recipes or recipes from other mods from within a `ModSystem.PostAddRecipes` method. Basically, we iterate over the recipes to find the recipe we want to tweak, then modify ingredients, tiles, or even disable the recipe entirely.

## Complete Example
This code removes the "Chain" item from all vanilla recipes. The second half of this example finds and disables an exact recipe. Do not remove entries from `Main.recipe`, that will break many things.
```csharp
public override void PostAddRecipes() {
	for (int i = 0; i < Recipe.numRecipes; i++) {
		Recipe recipe = Main.recipe[i];

		if (recipe.TryGetIngredient(ItemID.Chain, out Item ingredient)) {
			recipe.RemoveIngredient(ItemID.Chain);
		}

		if(recipe.HasRecipeGroup("IronBar") && recipe.HasTile(TileID.Anvils) && recipe.HasResult(ItemID.Chain) && recipe.createItem.stack == 10) {
			recipe.DisableRecipe();
		}
	}
}
```

# Cross-Mod Recipes
If you wish to use items or tiles from other mods in your recipes, you need to plan ahead. Read [Expert Cross Mod Content](https://github.com/tModLoader/tModLoader/wiki/Expert-Cross-Mod-Content) for an idea of some considerations you'll need to keep in mind. For example, the mod you are referencing might change the item name in an update to their mod, breaking your mod in the process. Another issue is planning for the other mod not being enabled. In that case, you might want to skip the recipe altogether or plan an alternate recipe. The example below shows the alternate ingredients approach. 

```cs
ModLoader.TryGetMod("ExampleMod", out Mod exampleMod);
Recipe recipe = Recipe.Create(ItemID.Wood, 999);
if (exampleMod != null && Mod.TryFind<ModItem>("ExampleWings", out ModItem ExampleWings)) {
	recipe.AddIngredient(ExampleWings.Type);
}
else {
	recipe.AddIngredient(ModContent.ItemType<MyItem>());
	recipe.AddIngredient(ItemID.EnchantedSword);
}
recipe.Register();
```
As you can see, this recipe takes `ExampleMod`'s `ExampleWings` item if `ExampleMod` is enabled and `ExampleWings` exists in that mod, and the current mod's `MyItem` and vanilla's `EnchantedSword` if `ExampleMod` is not enabled. If you have opted to only load your mod if the other mod is enabled, you can simplify the logic since the other mod will always be available if you are using `modReferences`.

# Common Errors

### `MyModItem.AddRecipeGroups()`: no suitable method found to override
`ModSystem.AddRecipeGroups` has to go in the `ModSystem` class, not your `ModItem` class.

# Relevant References
* [Vanilla ItemIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Item-IDs)
* [Vanilla TileIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs)
* [ModRecipe Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_recipe.html)
* [Mod Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod.html)

# Not covered in Intermediate level
There are other aspects of Recipes that will be covered in more advanced guides:
* Custom recipe conditions
* AddOnCraftCallback
* AddConsumeItemCallback