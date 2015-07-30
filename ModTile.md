# ModTile

This class represents a _type_ of tile that can be added by a mod. Only one instance of this class will ever exist for each type of tile that is added. Any hooks that are called will be called by the instance corresponding to the tile type. This is to prevent the game from using a massive amount of memory storing tile instances.

## Properties

### public Mod mod

The mod which has added this type of ModTile.

### public string Name

The name of this type of tile.

### public ushort Type

The internal ID of this type of tile.

## Fields

These fields can be set from the SetDefaults method.

### public int soundType

The default type of sound made when this tile is hit. Defaults to 0.

### public int soundStyle

The default style of sound made when this tile is hit. Defaults to 1.

### public int dustType

The default type of dust made when this tile is hit. Defaults to 0.

### public int drop

The default type of item dropped when this tile is killed. Defaults to 0, which means no item.

## Methods

### public void AddToArray(ref int[] array)

A convenient method for adding this tile's Type to the given array. This can be used with the arrays in TileID.Sets.RoomNeeds.

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to modify the name and texture path of this tile when it is autoloaded. Return true to autoload this tile. When a tile is autoloaded, that means you do not need to manually call Mod.AddTile. By default returns the mod's autoload property.

### public virtual void SetDefaults()

Allows you to set the properties of this tile. Many properties are stored as arrays throughout Terraria's code.

### public virtual bool KillSound(int i, int j)

Allows you to customize which sound you want to play when the tile at the given coordinates is hit. Return false to stop the game from playing its default sound for the tile. Returns true by default.

### public virtual void NumDust(int i, int j, bool fail, ref int num)

Allows you to change how many dust particles are created when the tile at the given coordinates is hit.

### public virtual bool CreateDust(int i, int j, ref int type)

Allows you to modify the default type of dust created when the tile at the given coordinates is hit. Return false to stop the default dust (the type parameter) from being created. Returns true by default.

### public virtual void DropCritterChance(int i, int j, ref int wormChance, ref int grassHopperChance, ref int jungleGrubChance)

Allows you to modify the chance the tile at the given coordinates has of spawning a certain critter when the tile is killed.

### public virtual bool Drop(int i, int j)

Allows you to customize which items the tile at the given coordinates drops. Return false to stop the game from dropping the tile's default item. Returns true by default.

### public virtual void KillTile(int i, int j, ref bool fail, ref bool effectOnly, ref bool noItem)

Allows you to determine what happens when the tile at the given coordinates is killed or hit with a pickaxe. Fail determines whether the tile is mined, effectOnly makes it so that only dust is created, and noItem stops items from dropping.

### public virtual void KillMultiTile(int i, int j, int frameX, int frameY)

This hook is called exactly once whenever a block encompassing multiple tiles is destroyed. You can use it to make your multi-tile block drop a single item, for example.