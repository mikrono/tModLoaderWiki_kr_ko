# ModRecipe

This class extends Terraria.Recipe, meaning you can use it in a similar manner to vanilla recipes. However, it provides methods that simplify recipe creation. Recipes are added by creating new instances of ModRecipe, then calling the AddRecipe method.

## Fields inherited from Terraria.Recipe

- public bool needsHoney
- public bool needsWater
- public bool needsLava
- public bool needSnowBiome
- public bool anyWood
- public bool anyIronBar
- public bool anyPressurePlate
- public bool anySand
- public bool anyFragment
- public bool alchemy

## Fields

### public readonly Mod mod

The mod which created this recipe.

## Properties

### public int RecipeIndex

_Deprecated_ - Ever since v0.8.2 with RecipeEditor.DeleteRecipe, this will not always be accurate.

The index in Main.recipe in which this recipe is stored.

## Constructors

### public ModRecipe(Mod mod)

Creates a new ModRecipe instance created by the given Mod.

## Methods

### public void SetResult(int itemID, int stack = 1)

Sets the result of this recipe with the given item type and stack size.

### public void SetResult(string itemName, int stack = 1)

Sets the result of this recipe with the given vanilla item name and stack size.

### public void SetResult(Mod mod, string itemName, int stack = 1)

Sets the result of this recipe with the given item name from the given mod, and with the given stack stack. If the mod parameter is null, then it will automatically use an item from the mod creating this recipe.

### public void SetResult(ModItem item, int stack = 1)

Sets the result of this recipe to the given type of item and stack size. Useful in ModItem.AddRecipes.

### public void AddIngredient(int itemID, int stack = 1)

Adds an ingredient to this recipe with the given item type and stack size.

### public void AddIngredient(string itemName, int stack = 1)

Adds an ingredient to this recipe with the given vanilla item name and stack size.

### public void AddIngredient(Mod mod, string itemName, int stack = 1)

Adds an ingredient to this recipe with the given item name from the given mod, and with the given stack stack. If the mod parameter is null, then it will automatically use an item from the mod creating this recipe.

### public void AddIngredient(ModItem item, int stack = 1)

Adds an ingredient to this recipe of the given type of item and stack size.

### public void AddRecipeGroup(string name, int stack = 1)

Adds a recipe group ingredient to this recipe with the given RecipeGroup name and stack size. Vanilla recipe groups consist of "Wood", "IronBar", "PresurePlate", "Sand", and "Fragment".

### public void AddTile(int tileID)

Adds a required crafting station with the given tile type to this recipe.

### public void AddTile(Mod mod, string tileName)

Adds a required crafting station to this recipe with the given tile name from the given mod. If the mod parameter is null, then it will automatically use a tile from the mod creating this recipe.

### public void AddTile(ModTile tile)

Adds a required crafting station to this recipe of the given type of tile.

### public virtual bool RecipeAvailable()

Whether or not the conditions are met for this recipe to be available for the player to use. This hook can be used for conditions unrelated to items or tiles (for example, biome or time).

### public virtual void OnCraft(Item item)

Allows you to make anything happen when the player uses this recipe. The item parameter is the item the player has just crafted.

### public virtual int ConsumeItem(int type, int numRequired)

Allows you to determine how many of a certain ingredient is consumed when this recipe is used. Return the number of ingredients that will actually be consumed. By default returns numRequired.

### public void AddRecipe()

Adds this recipe to the game. Call this after you have finished setting the result, ingredients, etc.