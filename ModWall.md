# ModWall

This class represents a type of wall that can be added by a mod. Only one instance of this class will ever exist for each type of wall that is added. Any hooks that are called will be called by the instance corresponding to the wall type.

## Properties

### public Mod mod

The mod which has added this type of ModWall.

### public string Name

The name of this type of wall.

### public ushort Type

The internal ID of this type of wall.

## Fields

These fields can be set from the SetDefaults method.

### public int soundType

The default type of sound made when this wall is hit. Defaults to 0.

### public int soundStyle

The default style of sound made when this wall is hit. Defaults to 1.

### public int dustType

The default type of dust made when this wall is hit. Defaults to 0.

### public int drop

The default type of item dropped when this wall is killed. Defaults to 0, which means no item.

## Methods

### public void AddMapEntry(Color color, string name = "")

Adds an entry to the minimap for this wall with the given color and display name. This should be called in SetDefaults.

### public void AddMapEntry(Color color, string name, Func\<string, int, int, string\> nameFunc)

Adds an entry to the minimap for this wall with the given color, default display name, and display name function. The parameters for the function are the default display name, x-coordinate, and y-coordinate. This should be called in SetDefaults.

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to modify the name and texture path of this wall when it is autoloaded. Return true to autoload this wall. When a wall is autoloaded, that means you do not need to manually call Mod.AddWall. By default returns the mod's autoload property.

### public virtual void SetDefaults()

Allows you to set the properties of this wall. Many properties are stored as arrays throughout Terraria's code.

### public virtual bool KillSound(int i, int j)

Allows you to customize which sound you want to play when the wall at the given coordinates is hit. Return false to stop the game from playing its default sound for the wall. Returns true by default.

### public virtual void NumDust(int i, int j, bool fail, ref int num)

Allows you to change how many dust particles are created when the wall at the given coordinates is hit.

### public virtual bool CreateDust(int i, int j, ref int type)

Allows you to modify the default type of dust created when the wall at the given coordinates is hit. Return false to stop the default dust (the type parameter) from being created. Returns true by default.

### public virtual bool Drop(int i, int j, ref int type)

Allows you to customize which items the wall at the given coordinates drops. Return false to stop the game from dropping the tile's default item (the type parameter). Returns true by default.

### public virtual void KillWall(int i, int j, ref bool fail)

Allows you to determine what happens when the tile at the given coordinates is killed or hit with a hammer. Fail determines whether the tile is mined (whether it is killed).

### public virtual bool CanExplode(int i, int j)

Whether or not the wall at the given coordinates can be killed by an explosion (ie. bombs). Returns true by default; return false to stop an explosion from destroying it.

### public virtual ushort GetMapOption(int i, int j)

Allows you to choose which minimap entry the wall at the given coordinates will use. 0 is the first entry added by AddMapEntry, 1 is the second entry, etc. Returns 0 by default.

### public virtual void ModifyLight(int i, int j, ref float r, ref float g, ref float b)

Allows you to determine how much light this wall emits. This can also let you light up the block in front of this wall.

### public virtual void RandomUpdate(int i, int j)

Called whenever the world randomly decides to update the tile containing this wall in a given tick. Useful for things such as growing or spreading.

### public virtual void AnimateWall(ref byte frame, ref byte frameCounter)

Allows you to animate your wall. Use frameCounter to keep track of how long the current frame has been active, and use frame to change the current frame.

### public virtual bool PreDraw(int i, int j, SpriteBatch spriteBatch)

Allows you to draw things behind the wall at the given coordinates. Return false to stop the game from drawing the wall normally. Returns true by default.

### public virtual void PostDraw(int i, int j, SpriteBatch spriteBatch)

Allows you to draw things in front of the wall at the given coordinates.