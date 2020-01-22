# Prerequisites 
* [Basic Recipes](Basic-Recipes)

# RecipeGroups
RecipeGroups allow you create a recipe that accepts items from a group of similar ingredients. For example, all varieties of Wood are in the vanilla "Wood" Group. Instead of creating 8 separate recipes to use wood, we would create 1 recipe that uses the "Wood" recipe group. Mods can also add RecipeGroups to simplify their recipes.

## Using Existing RecipeGroups
Vanilla groups consist of: "Wood", "IronBar", "PresurePlate", "Sand", "Fragment", "Birds", "Scorpions", "Squirrels", "Bugs", "Ducks", "Butterflies", "Fireflies", and "Snails". To use a vanilla RecipeGroup, simply add the following instead of a similar AddIngredient line:
```csharp
recipe.AddRecipeGroup("Wood", 5);
```
Note that, like with AddIngredient, there is an optional parameter for specifying a stack of more than 1.

## New RecipeGroups
You may have noticed that aside from "Wood" and "IronBar", there aren't many useful vanilla RecipeGroups. Luckily, we can make our own RecipeGroups. Let's imagine we want to make a recipe that used a Magic Mirror or Ice Mirror as an ingredient. Without RecipeGroups, we would have to do this:
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
This is actually not too bad, but imagine you have 10 or 20 similar items that you'd like to use for your recipe, it would get unmanageable very quickly. To solve this, first we must add a new RecipeGroup. We do this in our Mod class by overriding the AddRecipeGroups method:
```csharp
// Don't forget using Terraria.Localization; up top.
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
As seen above, first we construct the group, then we call RegisterGroup with the desired name. As a convention, please use "ModName:GroupName". As a note, Language.GetTextValue("LegacyMisc.37") is just the word "any" in the selected language and requires writing `using Terraria.Localization;` at the top of your code. The syntax here is a little difficult for newcomers, so please follow it exactly. Note: use ItemType("ItemName") instead of ItemID.ItemName for ModItems.

Next, we need to use the RecipeGroup. We do this just like with vanilla RecipeGroups, except our RecipeGroup's name will be different:
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
## Editing Vanilla RecipeGroups
In some situations you might want to add your own items to existing RecipeGroups. In this example, we will add another wood type to the "Wood" RecipeGroup.
```csharp
public override void AddRecipeGroups()
{
	if(RecipeGroup.recipeGroupIDs.ContainsKey("Wood"))
	{
		int index = RecipeGroup.recipeGroupIDs["Wood"];
		RecipeGroup group = RecipeGroup.recipeGroups[index];
		group.ValidItems.Add(ItemType("MyWood"));
	}
}
```
Note that checking `if(RecipeGroup.recipeGroupIDs.ContainsKey(...))` is not necessary, but it will prevent errors if some other mod completely removes that RecipeGroup for some reason.

# Editing Vanilla Recipes
We can edit vanilla recipes from within out AddRecipes methods. Basically, we use the RecipeFinder class to act as search parameters, then go through the results and act on them. From the results of RecipeFinder, we can construct a RecipeEditor to modify ingredients, tiles, or even delete vanilla recipes.

## Complete Example
This code removes the ingredient Chain from all vanilla recipes. The second half of this example finds and deletes an exact recipe and deletes the whole recipe.
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
	recipe.AddIngredient(this.ItemType("MyItem"));
	recipe.AddIngredient(ItemID.EnchantedSword);
}
recipe.SetResult(ItemID.Wood, 999);
recipe.AddRecipe();
```
As you can see, this recipe takes ExampleMod's ExampleWings item if ExampleMod is enabled, and the current mods MyItem and vanilla's EnchantedSword if ExampleMod is not enabled. If you have opted to only load your mod if the other mod is enabled, you can simplify the logic since the other mod will always be available if you are using modReferences.

# Common Errors

### MyModItem.AddRecipeGroups(): no suitable method found to override
`AddRecipeGroups` has to go in the `Mod` class, not your `ModItem` class.

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