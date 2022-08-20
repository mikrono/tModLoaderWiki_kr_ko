***
This Guide has been updated to 1.4. If you need to view the old 1.3 version of this wiki page, click [here](https://github.com/tModLoader/tModLoader/wiki/Basic-Recipes/f1a9c7a29f517a696631670bf58f75e7a7e4ccb9)
***

# Basic Recipe
Recipes can be added to the game in 3 places. In `Mod.AddRecipes`, `ModItem.AddRecipes`, and `ModSystem.AddRecipes`. Where you add your recipes is up to your organizational preferences, but do note that the `ModItem.CreateRecipe` method cannot be used everywhere as is, use `Recipe.Create` where it is not possible. 

Recipes consist of Ingredients (Items consumed to craft the result), Tiles (Tiles you need to stand by), and Results (Item created).

# Basic Recipe Structure
## Required Using Statements
First, make sure that you have the following using statements in the .cs file. These using statements will let us access recipe related functions:    
```cs
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;
```
## Create Recipe and Assign Recipe Result
To start a recipe we create an instance of the `Recipe` class. We do this through the `Recipe.Create` method or the `ModItem.CreateRecipe` method. When creating a recipe, we need to assign the recipe result type and result stack. The `ModItem.CreateRecipe` method assumes that the recipe results in the current ModItem, so only the stack size is needed. The stack size is optional and defaults to 1:

In `Mod` class, we type "Recipe.Create" to use the `Recipe.Create` method. Here are various examples, showing vanilla and modded ingredients as well as default stack sizes and custom stack sizes:
```cs
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueZ); 
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueZ, 5); 
Recipe recipe = Recipe.Create(ModContent.ItemType<Content.Items.ExampleItem>());
Recipe recipe = Recipe.Create(ModContent.ItemType<Content.Items.ExampleItem>(), 10);
```
In `ModSystem` class, we need to use "Recipe.Create":
```cs
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueZ); 
```
In `ModItem` class, we can use "Recipe.Create" to create a recipe that doesn't have this ModItem as a result:
```cs
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueZ); 
// ... And we can use "CreateRecipe" directly to create a recipe that results in this ModItem. We can optionally provide a stack size:
Recipe recipe = CreateRecipe(); 
Recipe recipe = CreateRecipe(10); 
```
## Add Recipe Ingredients
Now, we add the ingredients. These are the items we want the recipe to be consumed when the recipe is crafted:

```cs
recipe.AddIngredient(ItemID.DirtBlock);
recipe.AddIngredient(ItemID.Ruby);
```
AddIngredient also takes an optional argument for specifying a stack size:

```cs
recipe.AddIngredient(ItemID.Chain, 10);
```
The previous examples added vanilla items to the recipe by referencing the ItemID class. With a capable IDE such as Visual Studio, you will find autocomplete and intellisense very useful, but you can also [look up ItemID names or values here](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Item-IDs). 

We can also add modded items added by this mod. There are several ways we can do this. Go for whatever approach you like. The 1st approach is the cleanest. All of these methods can add the stack parameter. These examples all point to the `ExampleItem` class in the `ExampleMod.Content.Items` namespace:

```cs
recipe.AddIngredient<Content.Items.ExampleItem>();
recipe.AddIngredient<Content.Items.ExampleItem>(10);
recipe.AddIngredient(ModContent.ItemType<Content.Items.ExampleItem>());
recipe.AddIngredient(ModContent.GetInstance<Content.Items.ExampleItem>());
recipe.AddIngredient(Mod, "ExampleItem");
```

Next, we can specify the crafting stations. This follows the same patterns as items. You can [look up TileIDs here](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs).

```cs
recipe.AddTile(TileID.WorkBenches);
recipe.AddTile(TileID.Anvils);
recipe.AddTile<Content.Tiles.Furniture.ExampleWorkbench>();
recipe.AddTile(ModContent.TileType<Content.Tiles.Furniture.ExampleWorkbench>());
recipe.AddTile(ModContent.GetInstance<Content.Tiles.Furniture.ExampleWorkbench>());
recipe.AddTile(Mod, "ExampleWorkbench");
```
Finally, we need to tell tModLoader that our Recipe is complete and add it to the game:
```cs
recipe.Register();
```

### Using Vanilla vs Modded Ingredients and Tiles
As a recap, vanilla items and tiles use the `TileID` and `ItemID` classes, while modded items and items use the `ModContent.TileType` and `ModContent.ItemType` methods:

```cs
recipe.AddTile(TileID.WorkBenches); // Vanilla Tile
recipe.AddTile(ModContent.TileType<Content.Tiles.Furniture.ExampleWorkbench>()); // Modded Tile
recipe.AddIngredient(ItemID.Meowmere); // Vanilla Item
recipe.AddIngredient(ModContent.ItemType<Content.Items.ExampleItem>()); // Modded Item
```

## Full Basic Recipe Example
Here are a few full basic recipe example. This first example resides in a ModSystem class. The recipe takes 1 Chain and 10 Stone Blocks, it is crafted at a workbench and anvil, and the resulting item is 1 AlphabetStatueA.

```cs
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueA);
recipe.AddIngredient(ItemID.StoneBlock, 10);
recipe.AddIngredient(ItemID.Chain);
recipe.AddTile(TileID.WorkBenches);
recipe.AddTile(TileID.Anvils);
recipe.Register();
```

This example is in a `ModItem` class. This recipe crafts 3 of the ModItem from 5 `ExampleItem`s:

```cs
Recipe recipe = CreateRecipe(3);
recipe.AddIngredient<Content.Items.ExampleItem>(5);
recipe.Register();
```

# Chain Syntax
The code in this guide is quite wordy. Using the chaining syntax, we can reduce the verbosity of our code. This approach may be more pleasing to the modder. Only the final line has a semi-colon.
```cs
Recipe.Create(ItemID.AlphabetStatueA)
	.AddIngredient(ItemID.StoneBlock, 10)
	.AddIngredient(ItemID.Chain)
	.AddTile(TileID.WorkBenches)
	.AddTile(TileID.Anvils)
	.Register();
```


# Recipe Groups
Recipe Groups allow a single ingredient to be satisfied from a selection of similar items. The most common example of this is using Iron Bar or Lead Bar to craft a recipe. Recipe Groups are discussed in [Intermediate Recipes](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Recipes#recipegroups)

# Conditions
In addition to ingredients and crafting stations, recipes can also have conditions. Each condition must be satisfied for the recipe to be able to be crafted.

## Water, Honey, Lava
Water, Honey, and Lava are not technically Tiles, so to make a recipe require standing next to those, use one of the following:

```cs
recipe.AddCondition(Recipe.Condition.NearWater);
recipe.AddCondition(Recipe.Condition.NearLava);
recipe.AddCondition(Recipe.Condition.NearHoney);
```
Note that `NearWater` is also satisfied by Sinks, so don't add the Sink tile separately. Also note that there is a needSnowBiome you can also set, but anything more advanced would utilize ModRecipe.RecipeAvailable.

## Custom Condition
Intermediate?

### RecipeAvailable
Intermediate?

## Multiple Recipes
With multiple Recipes in the same AddRecipes, make sure not to re-declare your variable name. The following will cause errors: 

```cs
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueA); 
// other code
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueB);
// other code
```
You can name your variables recipe1, recipe2, and so on, but a cleaner approach would be to just reuse the same variable:

```cs
Recipe recipe = Recipe.Create(ItemID.AlphabetStatueA); 
// other code
recipe = Recipe.Create(ItemID.AlphabetStatueB); 
// other code
```

If you are using the [Chain Syntax](#Chain-Syntax), then simply do the same approach for each recipe on subsequent lines.

## Making an "upgraded" vanilla tile
As an aside, you may want your ModTile to count as, say, a workbench or anvil. To do this, add the following to your `ModTile.SetStaticDefaults`:

```cs
AdjTiles = new int[]{ TileID.WorkBenches };
```

## Complete Examples
Here are 2 complete examples, one showing recipes added in a `ModItem` class more suitable for recipes involving that `ModItem`, and the other showing adding recipes in a `Mod` class more suitable for recipes involving vanilla items. Technically the recipes can go in either location, but for organization purposes it is sometimes nice to have recipes in ModItem classes.
### ModItem Example
```cs
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod.Items.Abomination
{
	public class FoulOrb : ModItem
	{
		public override void SetDefaults()
		{
			// other code
		}

		public override void AddRecipes()
		{
			Recipe recipe = CreateRecipe();
			recipe.AddIngredient(ItemID.BeetleHusk);
			recipe.AddIngredient(ItemID.Ectoplasm, 5);
			recipe.AddTile(TileID.MythrilAnvil);
			recipe.Register();

			recipe = CreateRecipe(20);
			recipe.AddIngredient<Content.Items.ExampleItem>(10);
			recipe.AddTile<Content.Tiles.Furniture.ExampleWorkbench>();
			recipe.AddCondition(Recipe.Condition.NearLava)
			recipe.Register();
		}
	}
}
```

### Mod Example
Remember that in `Mod` or `ModSystem`, we must pass in the receipe result item type.
```cs
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

namespace ExampleMod
{
	public class ExampleMod : Mod
	{
		// Other methods and fields here...

		public override void AddRecipes()
		{
			// Here is an example of a recipe.
			Recipe recipe = CreateRecipe(ItemID.Wood, 999);
			recipe.AddIngredient<Content.Items.ExampleItem>();
			recipe.Register();

			// Here we reuse 'recipe', meaning we don't need to re-declare that it is a ModRecipe 
			recipe = CreateRecipe(ItemID.PumpkinPie, 2);
			recipe.AddIngredient(ItemID.BlueBerries, 20);
			recipe.AddTile(TileID.WorkBenches);
			recipe.Register();
		}
	}
}
```

## Common Errors
### Error CS0117 'ItemID' (or TileID) does not contain a definition for 'MyModItem'
You tried to use the vanilla item syntax for adding a ModItem, read this tutorial again.
### Error CS0103 The name 'recipe' does not exist in the current context
You forgot to declare your first recipe as `Recipe`. Make sure the first recipe in your code starts with `Recipe recipe = ...`
### Error CS0128 A local variable named 'recipe' is already defined in this scope
Read `Multiple Recipes` above.
### My recipes aren't in game
Check that your AddRecipes method has override not virtual.
### No suitable method to override
Make sure you are only overriding AddRecipes in Mod, ModSystem, or ModItem.

## Relevant References
* [Vanilla ItemIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Item-IDs)
* [Vanilla TileIDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs)
* [ModRecipe Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_recipe.html)
* [Mod Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod.html)

## Not covered in Basic level
There are other aspects of Recipes that will be covered in more advanced guides:
* [RecipeGroups](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Recipes#recipegroups) -- Intermediate -- Allows a single ingredient to be 1 of a large group, like how most recipes involving wood can take Boreal Wood or Pearl Wood. ("any wood")
* [Editing Recipes](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Recipes#editing-recipes) -- Intermediate -- Edit existing recipes or disable existing recipes
* Ordering Recipes
* OnCraftCallback
* ConsumeItemCallback
* RecipeAvailable
* [Cross-Mod Content](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Recipes#cross-mod-recipes) -- Intermediate -- Use Items or Tiles from other mods in your Recipe.