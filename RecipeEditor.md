# RecipeEditor

This class allows you to make any changes you want to a recipe, whether it be adding/removing ingredients, changing the result, or removing the recipe entirely.

## Constructors

### public RecipeEditor(Recipe recipe)

Creates a recipe editor that acts on the given recipe.

## Methods

### public void AddIngredient(int itemID, int stack = 1)

Adds an ingredient with the given item ID and stack size to the recipe. If the recipe already contains the ingredient, it will increase the stack requirement instead.

### public bool SetIngredientStack(int itemID, int stack)

Sets the stack requirement of the ingredient with the given item ID in the recipe. Returns true if the operation was successful. Returns false if the recipe does not contain the ingredient.

### public bool DeleteIngredient(int itemID)

Deletes the ingredient requirement with the given ID from the recipe. Returns true if the operation was successful. Returns false if the recipe did not contain the ingredient in the first place.

### public bool AcceptRecipeGroup(string groupName)

Adds the recipe group with the given name to the recipe. Note that, unlike ModRecipe and RecipeFinder, this won't actually add an ingredient; it will only allow existing ingredients to be interchangeable with other items. Returns true if the operation was successful. Returns false if the recipe already accepts the given recipe group.

### public bool RejectRecipeGroup(string groupName)

Removes the recipe group with the given name from the recipe. This is the opposite of AcceptRecipeGroup; while it won't remove ingredients, it will make existing ingredients no longer be interchangeable with other items. Returns true if the operation was successful. Returns false if the recipe did not contain the recipe group in the first place.

### public void SetResult(int itemID, int stack = 1)

A convenience method for setting the result of the recipe. Similar to calling recipe.createItem.SetDefaults(itemID), followed by recipe.createItem.stack = stack.

### public void SetResult(string itemName, int stack = 1)

A convenience method for setting the result of the recipe. This uses the vanilla item name instead of the item ID.

### public bool AddTile(int tileID)

Adds the crafting station with the given tile ID to the recipe. Returns true if the operation was successful. Returns false if the recipe already requires the given tile.

### public bool DeleteTile(int tileID)

Removes the crafting station with the given tile ID as a requirement from the recipe. Returns true if the operation was successful. Returns false if the recipe did not require the tile in the first place.

### public void SetNeedWater(bool needWater)

A convenience method for setting recipe.needWater.

### public void SetNeedLava(bool needLava)

A convenience method for setting recipe.needLava.

### public void SetNeedHoney(bool needHoney)

A convenience method for setting recipe.needHoney.

### public bool DeleteRecipe()

Completely removes the recipe from the game, making it unusable. Returns true if the operation was successful. Returns false if the recipe was already not in the game.