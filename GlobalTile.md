# GlobalTile

This class allows you to modify the behavior of any tile in the game. Create an instance of an overriding class then call Mod.SetGlobalTile to use this.

## Properties

### public Mod mod

The mod to which this GlobalTile belongs to.

## Methods

### public void AddToArray(ref int[] array, int type)

A convenient method for adding an integer to the end of an array. This can be used with the arrays in TileID.Sets.RoomNeeds.

### public virtual void SetDefaults()

Allows you to modify the properties of any tile in the game. Most properties are stored as arrays throughout the Terraria code.

### public virtual bool KillSound(int i, int j, int type)

Allows you to customize which sound you want to play when the tile at the given coordinates is hit. Return true to stop the game from playing its default sound for the tile. Returns false by default.

### public virtual void NumDust(int i, int j, int type, ref int num)

Allows you to change how many dust particles are created when the tile at the given coordinates is hit.

### public virtual bool CreateDust(int i, int j, int type, ref int dustType)

Allows you to modify the default type of dust created when the tile at the given coordinates is hit. Return true to stop the default dust from being created. Returns false by default.

### public virtual void DropCritterChance(int i, int j, int type, ref int wormChance, ref int grassHopperChance, ref int jungleGrubChance)

Allows you to modify the chance the tile at the given coordinates has of spawning a certain critter when the tile is killed.

### public virtual bool Drop(int i, int j, int type)

Allows you to customize which items the tile at the given coordinates drops. Return true to stop the game from dropping the tile's default item. Returns false by default.

### public virtual void KillTile(int i, int j, int type, ref bool fail, ref bool effectOnly, ref bool noItem)

Allows you to determine what happens when the tile at the given coordinates is killed or hit with a pickaxe. Fail determines whether the tile is mined, effectOnly makes it so that only dust is created, and noItem stops items from dropping.