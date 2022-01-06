# Prerequisites 
* [Basic Recipes](Basic-Recipes)

# Recipe Groups
Recipe groups allow you to create a recipe that accepts any item out of a list of specified ingredients. For example, all varieties of wood are in the vanilla "Wood" Group. Instead of creating 8 separate recipes to use each type of vanilla wood, we would create 1 recipe that uses the "Wood" recipe group. Mods can also add recipe groups to simplify their recipes.

## Using Existing Recipe Groups
Vanilla groups consist of: "Wood", "IronBar", "PresurePlate", "Sand", "Fragment", "Birds", "Scorpions", "Squirrels", "Bugs", "Ducks", "Butterflies", "Fireflies", and "Snails". To use a vanilla recipe group, simply add the following instead of a similar `ModRecipe.AddIngredient` line:
```csharp
recipe.AddRecipeGroup("Wood", 5); // parameter 1 is the group name, parameter 2 is the item stack count
```
Note that, like with `Modrecipe.AddIngredient`, the parameter for providing an explicit stack count is optional and defaults to 1.

## New Recipe Groups
You may have noticed that aside from "Wood" and "IronBar", there aren't many useful vanilla recipe groups. Luckily, we can make our own recipe groups. Let's imagine we want to make a recipe that used a Magic Mirror or Ice Mirror as an ingredient. Without recipe groups, we would have to do this:
```csharp
ModRecipe recipe = new ModRecipe(mod);
recipe.AddIngredient(ItemID.MagicMirror);
recipe.AddIngredient(ItemID.Gel, 10);
recipe.AddTile(TileID.Chairs);
recipe.AddTile(TileID.Tables);
recipe.SetResult(this);
recipe.AddRecipe();

recipe = new ModRecipe(mod);
recipe.AddIngredient(ItemID.IceMirror);
recipe.AddIngredient(ItemID.Gel, 10);
recipe.AddTile(TileID.Chairs);
recipe.AddTile(TileID.Tables);
recipe.SetResult(this);
recipe.AddRecipe();
```
In this example, it isn't too bad, but imagine you have 10 or 20 similar items that you'd like to use for your recipe - it would get unmanageable very quickly. To solve this, first, we must add a new recipe group. We do this in our `Mod` class by overriding the `Mod.AddRecipeGroups` method:
```csharp
// The Terraria.Localization using directive (using Terraria.Localization;) is required for Language methods.
public override void AddRecipeGroups()
{
	RecipeGroup group = new RecipeGroup(() => Language.GetTextValue("LegacyMisc.37") + " Magic Mirror", new int[]
	{
		ItemID.IceMirror,
		ItemID.MagicMirror
	});
	RecipeGroup.RegisterGroup("SummonersAssociation:MagicMirrors", group);
}
```
As seen above, first we construct the group, then we call `RecipeGroup.RegisterGroup` with the desired name. As a convention, please use "`ModName:GroupName`. As a note, `Language.GetTextValue("LegacyMisc.37")` is just the word "Any" in the selected language and requires the `Terraria.Localization` using directive at the top of your code. The syntax here is a little difficult for newcomers (as it makes use of [lambda expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)), so please follow it exactly.

As a note: use `ModContent.ItemType<YourModItem>()` instead of `ItemID.ItemName` for `ModItem`s.

Next, we need to use the `RecipeGroup` instance. We do this just like with vanilla recipe groups, except our recipe group's name will be different:
```csharp
recipe.AddRecipeGroup("SummonersAssociation:MagicMirrors");
```
## Complete Example
```csharp
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;
using Terraria.Localization;

namespace SummonersAssociation
{
	public class SummonersAssociation : Mod
	{
		public SummonersAssociation()
		{
		}

		// Other Code

		public override void AddRecipeGroups()
		{
			RecipeGroup group = new RecipeGroup(() => Language.GetTextValue("LegacyMisc.37") + " Magic Mirror", new int[]
			{
				ItemID.IceMirror,
				ItemID.MagicMirror
			});
			RecipeGroup.RegisterGroup("SummonersAssociation:MagicMirrors", group);
		}

		public override void AddRecipes()
		{
			ModRecipe recipe = new ModRecipe(this);
			recipe.AddIngredient(ItemID.SlimeStaff);
			recipe.AddRecipeGroup("SummonersAssociation:MagicMirrors");
			recipe.AddTile(TileID.Chairs);
			recipe.AddTile(TileID.Tables);
			recipe.SetResult(ItemType<Items.MinionControlRod>());
			recipe.AddRecipe();

			recipe = new ModRecipe(this);
			recipe.AddIngredient(ItemID.Gel, 10);
			// Here, instead of adding 5 ItemID.Wood, we use a RecipeGroup to specify all types of Wood in a single recipe.
			recipe.AddRecipeGroup("Wood", 5);
			recipe.AddTile(TileID.Chairs);
			recipe.AddTile(TileID.Tables);
			recipe.SetResult(ItemType<Items.SummonersAssociationCard>());
			recipe.AddRecipe();
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

# Editing Vanilla Recipes
We can edit vanilla recipes from within our `Mod.AddRecipes` methods. Basically, we use the `RecipeFinder` class to act as search parameters, then go through the results and act on them. From the results of `RecipeFinder`, we can construct a `RecipeEditor` to modify ingredients, tiles, or even delete vanilla recipes entirely.

## Complete Example
This code removes the "Chain" item from all vanilla recipes. The second half of this example finds and deletes an exact recipe and deletes the whole recipe.
```csharp
public override void AddRecipes()
{
	RecipeFinder finder = new RecipeFinder();
	finder.AddIngredient(ItemID.Chain);
	foreach (Recipe recipe in finder.SearchRecipes())
	{
		RecipeEditor editor = new RecipeEditor(recipe);
		editor.DeleteIngredient(ItemID.Chain);
	}

	finder = new RecipeFinder();
	finder.AddRecipeGroup("IronBar");
	finder.AddTile(TileID.Anvils);
	finder.SetResult(ItemID.Chain, 10);
	Recipe recipe2 = finder.FindExactRecipe();
	if (recipe2 != null)
	{
		RecipeEditor editor = new RecipeEditor(recipe2);
		editor.DeleteRecipe();
	}
}
```

# Cross-Mod Recipes
If you wish to use items or tiles from other mods in your recipes, you need to plan ahead. Read [Expert Cross Mod Content](https://github.com/tModLoader/tModLoader/wiki/Expert-Cross-Mod-Content) for an idea of some considerations you'll need to keep in mind. For example, the mod you are referencing might change the item name in an update to their mod, breaking your mod in the process. Another issue is planning for the other mod not being enabled. In that case, you might want to skip the recipe altogether or plan an alternate recipe. The example below shows the alternate ingredients approach. 

```cs
Mod exampleMod = ModLoader.GetMod("ExampleMod");
ModRecipe recipe = new ModRecipe(this);
if (exampleMod != null) {
	recipe.AddIngredient(exampleMod.ItemType("ExampleWings"));
}
else {
	recipe.AddIngredient(ModContent.ItemType<MyItem>());
	recipe.AddIngredient(ItemID.EnchantedSword);
}
recipe.SetResult(ItemID.Wood, 999);
recipe.AddRecipe();
```
As you can see, this recipe takes `ExampleMod`'s `ExampleWings` item if `ExampleMod` is enabled, and the current mod's `MyItem` and vanilla's `EnchantedSword` if `ExampleMod` is not enabled. If you have opted to only load your mod if the other mod is enabled, you can simplify the logic since the other mod will always be available if you are using `modReferences`.

# Common Errors

### `MyModItem.AddRecipeGroups()`: no suitable method found to override
`Mod.AddRecipeGroups` has to go in the `Mod` class, not your `ModItem` class.

# Relevant References
* [Vanilla ItemIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Item-IDs)
* [Vanilla TileIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs)
* [ModRecipe Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_recipe.html)
* [RecipeFinder Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_recipe_finder.html)
* [RecipeEditor Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_recipe_editor.html)
* [Mod Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod.html)

# Not covered in Intermediate level
There are other aspects of ModRecipes that will be covered in more advanced guides:
* ModRecipe Inheritance
  * ModRecipe.OnCraft
  * ModRecipe.ConsumeItem
  * ModRecipe.RecipeAvailable