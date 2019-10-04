This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here.

# v0.11.5
v0.11.5 introduced ContentInstance, a faster and simpler way to access IDs and instances of modded classes. 

### Do I need to Update
No, all mods should still work, but if you wish to publish on v0.11.5, you will have to update.

## Mod.XType
If you previously used the generic `Mod.XType<T>()` methods, you'll need to change. `mod.ItemType<ExampleBlock>()` and `this.ItemType<ExampleBlock>()` are now simply `ItemType<ExampleBlock>()`, provided you write `using static Terraria.ModLoader.ModContent;` at the top of each .cs file. If you don't, it will be `ModContent.ItemType<ExampleBlock>()`. If you are still using the string versions, you do not need to update, but the new approach is faster, and the generic approach is less error prone. [Examples](https://github.com/tModLoader/tModLoader/commit/4bb65940e7928af8b6bf8fee30f935d37683505c)

## Mod.GetModX and Instance fields
Similar to above, generic `Mod.GetModX<T>()` methods such as `GetModWorld` are now no longer invoked from an instance of the `Mod` class. Replace `mod.GetModWorld<ExampleWorld>();` with `GetInstance<ExampleWorld>();`, provided you write `using static Terraria.ModLoader.ModContent;` at the top of each .cs file. If you don't, it will be `ModContent.GetInstance<ExampleWorld>()`. Also, static instance variables are no longer recommended. Remove things like `public static ExampleConfigServer Instance;` and simply call `GetInstance<ExampleConfigServer>()` instead. Another example: `ExampleMod.Instance` -> `GetInstance<ExampleMod>()`. [Examples](https://github.com/tModLoader/tModLoader/commit/4bb65940e7928af8b6bf8fee30f935d37683505c)

## ModTile.RightClick
ModTile.RightClick is now ModTile.NewRightClick. NewRightClick returns a bool indicating if an interaction has occurred, preventing weapon usage. To migrate, replace `public override void RightClick(int i, int j)` with `public override bool NewRightClick(int i, int j)` and add `return true;` if an interaction has occurred. [Examples](https://github.com/tModLoader/tModLoader/commit/e47dcfb91811a5d1df96ec9d3d598e828adac577)

## Iterating over Buffs
v0.11.5 adds `Player.MaxBuffs`. If you previously iterated over buffs using `for (int n = 0; n < 22; n++)`, you'll want to update the code to `for (int n = 0; n < Player.MaxBuffs; n++)`.

## Reflection
As always, your reflection might have broken, so double check that. Many internal classes and fields have changed namespaces and identifiers.

# v0.11

v0.11 introduced many changes. Here is the [migration guide](https://docs.google.com/document/d/127Gmexzj533xMBC3ISmvWUz1yy6RZYjVjO8H2ZotwGM/edit?usp=sharing).