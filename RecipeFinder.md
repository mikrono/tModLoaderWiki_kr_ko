# RecipeFinder

This class will search through all existing recipes for you based on criteria that you give it. It's useful for finding a particular vanilla recipe that you wish to remove or edit. Use this by creating new instances with the empty constructor for each search you perform.

## Fields

### public bool needWater

Adds the requirement of being nearby water to the search criteria. Defaults to false.

### public bool needLava

Adds the requirement of being nearby lava to the search criteria. Defaults to false.

### public bool needHoney

Adds the requirement of being nearby honey to the search criteria. Defaults to false.

## Methods

### public void AddIngredient(int itemID, int stack = 1)

Adds an ingredient with the given item type and stack size to the search criteria.

### public void AddIngredient(string itemName, int stack = 1)

Adds an ingredient with the given vanilla item name and stack size to the search criteria.

### public void AddRecipeGroup(string name, int stack = 1)

Adds a recipe group ingredient with the given RecipeGroup name and stack size to the search criteria.

### public void SetResult(int itemID, int stack = 1)

Sets the search criteria's result to the given item type and stack size.

### public void SetResult(string itemName, int stack = 1)

Sets the search criteria's result to the type corresponding to the given vanilla item name, with the given stack size.

### public void AddTile(int tileID)

Adds a required crafting station with the given tile type to the search criteria.

### public Recipe FindExactRecipe()

Searches for a recipe that matches the search criteria exactly, then returns it. That means the recipe will have exactly the same ingredients, tiles, liquid requirements, recipe groups, and result; even the stack sizes will match. If no recipe with an exact match is found, this will return null.

### public List\<Recipe\> SearchRecipes()

Searches for all recipes that include the search criteria, then returns them in a list. In terms of ingredients, it will search for recipes that include all the search criteria ingredients, with stack sizes greater than or equal to the search criteria. It will also make sure the recipes include all search criteria recipe groups and tiles. If the search criteria includes a result, the recipes will also have the same result with a stack size greater than or equal to the search criteria. Finally, if needWater, needLava, or needHoney are set to true, the found recipes will also have them set to true.