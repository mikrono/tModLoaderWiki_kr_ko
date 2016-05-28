# CraftGroup

**This has been removed as of v0.8.1. Use the vanilla RecipeGroup instead (see ExampleMod for usage).**

This class represents a "group" of crafting items; in other words, a list of items that can be used interchangeably in a crafting recipe. To add a crafting group, call Mod.AddCraftGroup in the Mod.AddCraftGroups hook.

## Fields

### public readonly string Name

The internal name of this crafting group.

### public readonly string DisplayName

The display name of this crafting group in recipes.

### public readonly IList<int> Items

A list of the items that make up this crafting group. Normally the first item in the list will be the one displayed in recipes.

## Static Properties

### public static CraftGroup Wood

Initially consists of all types of wood in vanilla.

### public static CraftGroup IronBar

Initially consists of Iron Bar and Lead Bar.

### public static CraftGroup PressurePlate

Initially consists of all types of pressure plates in vanilla.

### public static CraftGroup Sand

Initially consists of Sand, Ebonsand, Pearlsand, and Crimsand.

### public static CraftGroup Fragment

Initially consists of the four types of lunar fragments.