# Basic Recipe
Recipes can be added to the game in 2 places. In Mod.AddRecipes and in ModItem.AddRecipes. Where you add your recipes is up to your organizational preferences, but do note that "mod" and "this" in code below reflect code from the ModItem frame of reference. 

Recipes consist of Ingredients (Items consumed to craft the result), Tiles (Tiles you need to stand by), and Results (Item created).
## Basic Recipe Structure
First, make sure you have `using Terraria.ModLoader` at the top of your .cs file so you can use the ModRecipe class. You will also want `using Terraria.ID` as well if you plan to use any vanilla tiles or ingredients.

To start a recipe we create an instance of the ModRecipe class:

    ModRecipe recipe = new ModRecipe(mod);
Now, we add up to 14 different items we want the recipe to be crafted with:

    recipe.AddIngredient(ItemID.DirtBlock);
    recipe.AddIngredient(ItemID.Ruby);
AddIngredient also takes an optional argument for specifying a stack size:

    recipe.AddIngredient(ItemID.Chain, 10);
The previous examples added vanilla items to the recipe by referencing the ItemID class. With a capable IDE such as Visual Studio, you will find autocomplete and intellisense very useful, but you can also [look up ItemID names or values here](https://github.com/bluemagic123/tModLoader/wiki/Vanilla-Item-IDs). 

We can also add modded items added by this mod. There are several ways we can do this. Go for whatever approach you like:

    recipe.AddIngredient(mod, "ExampleItem");
    recipe.AddIngredient(null, "ExampleItem");
    recipe.AddIngredient(mod.GetItem("ExampleItem"));
    recipe.AddIngredient(mod.GetItem<Items.ExampleItem>());
    recipe.AddIngredient(this); // adds the current ModItem
    recipe.AddIngredient(mod.ItemType("ExampleItem"));
    recipe.AddIngredient(mod.ItemType<Items.ExampleItem>());

Next, we can specify up to 14 crafting stations. This follows the same patterns as items. You can [look up TileIDs here](https://github.com/bluemagic123/tModLoader/wiki/Vanilla-Tile-IDs).

    recipe.AddTile(TileID.WorkBenches);
    recipe.AddTile(TileID.Anvils);
    recipe.AddTile(mod.TileType("ExampleWorkbench"));
    recipe.AddTile(mod.TileType<Tiles.ExampleWorkbench>());
    recipe.AddTile(mod.GetTile("ExampleWorkbench"));
    recipe.AddTile(mod.GetTile<Tiles.ExampleWorkbench>());
Next, we need to set the result for the ModRecipe, what the recipe creates. There can only be 1 result per ModRecipe. This is done similar to AddIngredient as well. There is also an optional stack parameter here as well:

    recipe.SetResult(mod.ItemType<Items.ExampleItem>());
    recipe.SetResult(mod.ItemType("ExampleItem"));
    recipe.SetResult(ItemID.Meowmere);
    recipe.SetResult(this, 999);
Finally, we need to tell tModLoader that our ModRecipe is complete and add it to the game:

    recipe.AddRecipe();

### Using Vanilla vs Modded Ingredients and Tiles
As a recap, vanilla items and tiles use the TileID and ItemID classes, while modded items and items use the TileType and ItemType methods:

    recipe.AddTile(TileID.WorkBenches); // Vanilla Tile
    recipe.AddTile(mod.TileType("ExampleWorkbench")); // Modded Tile
    recipe.AddIngredient(ItemID.Meowmere); // Vanilla Item
    recipe.AddIngredient(mod.ItemType("ExampleItem")); // Modded Item
## Water, Honey, Lava
Water, Honey, and Lava are not technically Tiles, so to make a recipe require standing next to those, use one of the following:

    recipe.needWater = true;
    recipe.needHoney = true;
    recipe.needLava = true;
Note that needWater is also satisfied by Sinks, so don't add the Sink tile separately. Also note that there is a needSnowBiome you can also set, but anything more advanced would utilize ModRecipe.RecipeAvailable.

## Alchemy
Use recipe.AddTile(TileID.Bottles); for alchemy recipes. 

## Multiple Recipes
With multiple ModRecipes in the same AddRecipes, make sure not to redeclare your variable name. The following will cause errors: 

    ModRecipe recipe = new ModRecipe(mod);
    // other code
    ModRecipe recipe = new ModRecipe(mod);
    // other code
You can name your variables recipe1, recipe2, and so on, but a cleaner approach would be to just reuse the same variable:

    ModRecipe recipe = new ModRecipe(mod);
    // other code
    recipe = new ModRecipe(mod);
    // other code


## Making an "upgraded" vanilla tile
As an aside, you may want your ModTile to count as, say, a workbench or anvil. To do this, add the following to your ModTile.SetDefaults:

    adjTiles = new int[]{ TileID.WorkBenches };

## Complete Examples
Here are 2 complete examples, one showing recipes added in a ModItem class more suitable for recipes involving that ModItem, and the other showing adding recipes in a Mod class more suitable for recipes involving vanilla items. Technically the recipes can go in either location, but for organization purposes it is sometimes nice to have recipes in ModItem classes. Also pay attention to `this` and `mod`, as they refer to different things depending on which class they occur in.
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
    			ModRecipe recipe = new ModRecipe(mod);
    			recipe.AddIngredient(ItemID.BeetleHusk);
    			recipe.AddIngredient(ItemID.Ectoplasm, 5);
    			recipe.AddTile(TileID.MythrilAnvil);
    			recipe.SetResult(this);
    			recipe.AddRecipe();
    
    			recipe = new ModRecipe(mod);
    			recipe.AddIngredient(null, "BossItem", 10);
    			recipe.AddTile(null, "ExampleWorkbench");
    			recipe.needLava = true;
    			recipe.SetResult(this, 20);
    			recipe.AddRecipe();
    		}
    	}
    }
```

### Mod Example
Notice how the word `this` refers to the `mod` not a particular item as in the ModItem example.
```cs
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
			ModRecipe recipe = new ModRecipe(this);
			recipe.AddIngredient(null, "ExampleItem");
			recipe.SetResult(ItemID.Wood, 999);
			recipe.AddRecipe();

			recipe = new ModRecipe(this);
			recipe.AddIngredient(ItemID.BlueBerries, 20);
			recipe.AddTile(TileID.WorkBenches);
			recipe.SetResult(ItemID.PumpkinPie, 2);
			recipe.AddRecipe();
		}
	}
}
```

## Common Errors
### Error CS0117 'ItemID' (or TileID) does not contain a definition for 'MyModItem'
You tried to use the vanilla item syntax for adding a ModItem, read this tutorial again.
### Error CS0128 A local variable named 'recipe' is already defined in this scope
Read `Multiple Recipes` above.
### My recipes aren't in game
Check that your AddRecipes method has override not virtual.
### No suitable method to override
Make sure you are only overriding AddRecipes in Mod or ModItem.

## Relevant References
* [Vanilla ItemIDs](https://github.com/bluemagic123/tModLoader/wiki/Vanilla-Item-IDs)
* [Vanilla TileIDs](https://github.com/bluemagic123/tModLoader/wiki/Vanilla-Tile-IDs)
* [ModRecipe Documentation](http://bluemagic123.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_recipe.html)
* [Mod Documentation](http://bluemagic123.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod.html)

## Not covered in Basic level
There are other aspects of ModRecipes that will be covered in more advanced guides:
* RecipeGroups -- Intermediate -- Allows a single ingredient to be 1 of a large group, like how most recipes involving wood can take Boreal Wood or Pearl Wood. ("any wood")
* ModRecipe.OnCraft
* ModRecipe.ConsumeItem
* ModRecipe.RecipeAvailable
* RecipeFinder/RecipeEditor -- Intermediate -- Allows you to modify and delete vanilla recipes.