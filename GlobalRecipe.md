# GlobalRecipe

This class provides hooks that control all recipes in the game.

## Properties

### public Mod mod

The mod which added this GlobalRecipe.

### public string Name

The name of this GlobaRecipe.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalRecipe instead of using Mod.AddGlobalRecipe. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload, and to change the default internal name.

### public virtual bool RecipeAvailable(Recipe recipe)

Whether or not the conditions are met for the given recipe to be available for the player to use. This hook can be used for conditions unrelated to items or tiles (for example, biome or time).

### public virtual void OnCraft(Item item, Recipe recipe)

Allows you to make anything happen when the player uses the given recipe. The item parameter is the item the player has just crafted.