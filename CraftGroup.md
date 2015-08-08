# CraftGroup

This class represents a "group" of crafting items; in other words, a list of items that can be used interchangeably in a crafting recipe. To add a crafting group, call Mod.AddCraftGroup in the Mod.AddCraftGroups hook.

## Fields

### public readonly string Name

The internal name of this crafting group.

### public readonly string DisplayName

The display name of this crafting group in recipes.

### public readonly IList<int> Items

A list of the items that make up this crafting group. Normally the first item in the list will be the one displayed in recipes.

## Methods

### public static CraftGroup GetVanillaGroup(string name)

Gets the vanilla crafting group with the given name. Useful if you want to add modded items to vanilla crafting groups. The current vanilla crafting groups are "Wood", "IronBar", "Sand", "PressurePlate", and "Fragment".