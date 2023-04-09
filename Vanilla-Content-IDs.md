# IDs

Terraria uses numbers to reference various pieces of content in the game. Each of these numbers usually has a corresponding ID name. Modders can use these ID names to easily reference vanilla content. For example, a modder can type `Player.QuickSpawnItem(source, 22);` to spawn an Iron Bar item at the players location. `22` is confusing, however, so the modder can type `Player.QuickSpawnItem(source, ItemID.IronBar);` instead. There exist ID classes for many different types of content, such as Items, Projectiles, Tiles, and so on. Using any ID class requires `using Terraria.ID;` at the top of the file.

Do note that the values of these IDs overlap. Both `ItemID.IronBar` and `TileID.Demonite` have a value of `22`. It is important to make sure you are using the correct ID class for the type of content you are attempting to use. Mixing these up will result in bugs. For example, `Player.QuickSpawnItem(source, TileID.Demonite);` would spawn the `Iron Bar` item instead of the `Demonite` item.

This page lists info on using different types of IDs and links to the official Terraria wiki for the actual values. On these lists, there is typically an `Internal name` column corresponding to the actual ID name that a modder can type to get the numerical value. These lists can be useful for a quick reference, but don't waste your time looking up each ID while making your mod. We expect modders to be using [the autocomplete feature of their IDE](https://github.com/tModLoader/tModLoader/wiki/Why-Use-an-IDE#autocomplete--intellisense) to automatically type out these IDs. Typically the IDs mirror the English name of the items, so autocomplete should be sufficient for modding. On rare occasions you might not find the ID you are looking for, if that is the case you can use the wiki links to discover the actual ID name. Also note that if Terraria recently updated and tModLoader has yet to update, the newly added ID values corresponding to content added in the most recent updates will not exist in tModLoader.

The [Data IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Data_IDs) has links to many listings of content IDs. The rest of this page will go into more depth about content IDs commonly used in tModLoader modding..

### Searching Websites
You can use the `Ctrl-f` shortcut to open up your browsers find function. Once it is open, type your search term and the matching results will be highlighted.

### Count
Most ID classes have a `Count` field corresponding to end of the listing of IDs. You can iterate from `0` to `SomethingID.Count` in a `for` loop if you need to run some code for each piece of content. 

### Search
Many ID classes have a `Search` field that has methods that can be used to retrieve ID names from the corresponding number value, and vice versa. This can be useful for making debugging tools. Modded internal names can also be retrieved in the same manner, but remember that modded IDs can change between game reloads. If you are trying to track content between game sessions, use classes like `ItemDefinition`. 

### Sets
Contains in many ID classes is an inner class called `Sets`, these classes store data corresponding to the content. For example the `TileID.Sets.HasOutlines` set tracks which tiles have outlines, hinting to the game to load and draw the outlines when the tile is hovered. Modders typically assign to these `Sets` in `SetStaticDefaults` methods. For example, a modder would write `TileID.Sets.HasOutlines[Type] = true;` in the `SetStaticDefaults` method of their `ModTile` class to tell the game that this tile has outlines. See ExampleMod for various usages of other Sets.

# Item IDs

[Item IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Item_IDs)

Some common items with not obvious names: `Luminite Bar` is actually `LunarBar`, `Luminite Bullet` is actually `MoonlordBullet`, and `Sky Dragon's Fury` is actually `MonkStaffT3`.       

As a reminder you use these values either by writing `ItemID.Name` or just `Number`, do NOT write `ItemID.Number`:
```cs
recipe.AddIngredient(ItemID.Gel);  // Good
recipe.AddIngredient(23);          // Works, but is hard to read
recipe.AddIngredient(ItemID.23);   // Wrong
```

# Projectile IDs

[Projectile IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Projectile_IDs)

Projectile IDs are typically used in `Projectile.NewProjectile` methods to spawn projectiles into the world. Many projectiles have multiple versions with `Hostile` or `Friendly` attached to the name of some of the duplicates. These indicate that the projectile is either [`friendly`](https://github.com/tModLoader/tModLoader/wiki/Projectile-Class-Documentation#friendly) or [`hostile`](https://github.com/tModLoader/tModLoader/wiki/Projectile-Class-Documentation#hostile), use the one that matches the damage behavior you intend. If there isn't a version of the projectile you need, you may have to make your own version as a `ModProjectile`.

# Projectile AI Style IDs

[ProjAIStyleID.cs on the tModLoader GitHub](https://github.com/tModLoader/tModLoader/blob/1.4.4/patches/tModLoader/Terraria/ID/ProjAIStyleID.cs)    

Projectile AI styles (use for `Projectile.aiStyle`) do not have official names. Included in tModLoader is the `Terraria.ID.ProjAIStyleID` class which gives names to each.

# NPC IDs

[NPC IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/NPC_IDs)

Negative IDs represent NPCs that share a `type` with an NPC with a positive NPC. If you need to differentiate between these variants, you'll need to check `netID` to determine which variant is being checked. 

# NPC AI Style IDs

[NPCAIStyleID.cs on the tModLoader GitHub](https://github.com/tModLoader/tModLoader/blob/1.4.4/patches/tModLoader/Terraria/ID/NPCAIStyleID.cs)    
[AI page on the official Terraria wiki](https://terraria.wiki.gg/wiki/AI)    

NPC AI styles (use for `NPC.aiStyle`) do not have official names. Included in tModLoader is the `Terraria.ID.NPCAIStyleID` class which which gives names to each. The official wiki has another listing with more detailed descriptions. Use both pages to find the information desired.

# Tile IDs

[Tile IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Tile_IDs)

Tile IDs represent the type of tile. Tile IDs correspond to the `Tile.TileType` at a coordinate. Many furniture tiles listed have a number in the `Sub ID` column, this corresponds with the `style` concept explained in the [Basic Tile Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-Tile#tileobjectdata-or-frameimportantmultitiles). For example, most chests in the game are listed as the Tile ID of 21, but have different `Sub ID`/`style` values to differentiate them.

The wiki does not list the internal names, those can be found on the [Vanilla Tile IDs](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Tile-IDs) page.

# Wall IDs

[Wall IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Wall_IDs)

Wall IDs are used in `Tile.WallType`. Many walls have 2 variants, and unsafe and a safe variant. Safe variants are player placed walls that don't have the same enemy spawning properties of the unsafe variants.

# Buff IDs

[Buff IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Buff_IDs)

Use these in methods like `Player.AddBuff` and `NPC.AddBuff`. Note that many buffs only affect players or enemies, but not both. If Terraria doesn't have a way to give a buff to an enemy, it likely isn't programmed to work with NPCs. The same is true for buffs that aren't ever applied to players.

# Sound IDs

[Sound IDs page on the official Terraria wiki](https://terraria.wiki.gg/wiki/Sound_IDs)

Sound IDs can be used to play specific existing sounds in a mod. The [Adapting Vanilla Code section of the Basic Sounds Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-Sounds#adapting-vanilla-code-or-code-from-past-tmodloader-versions) goes over how to use the Sound ID information on the wiki to find and play specific Terraria sounds.

# others here...