# GlobalWall

This class allows you to modify the behavior of any wall in the game (although admittedly walls don't have much behavior). Create an instance of an overriding class then call Mod.AddGlobalWall to use this.

## Properties

### public Mod mod

The mod to which this GlobalWall belongs.

### public string Name

The name of this GlobalWall instance.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalWall instead of using Mod.AddGlobalWall. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void SetDefaults()

Allows you to modify the properties of any wall in the game. Most properties are stored as arrays throughout the Terraria code.

### public virtual bool KillSound(int i, int j, int type)

Allows you to customize which sound you want to play when the wall at the given coordinates is hit. Return false to stop the game from playing its default sound for the wall. Returns true by default.

### public virtual void NumDust(int i, int j, int type, bool fail, ref int num)

Allows you to change how many dust particles are created when the wall at the given coordinates is hit.

### public virtual bool CreateDust(int i, int j, int type, ref int dustType)

Allows you to modify the default type of dust created when the wall at the given coordinates is hit. Return false to stop the default dust (the dustType parameter) from being created. Returns true by default.

### public virtual bool Drop(int i, int j, int type, ref int dropType)

Allows you to customize which items the wall at the given coordinates drops. Return false to stop the game from dropping the wall's default item (the dropType parameter). Returns true by default.

### public virtual void KillWall(int i, int j, int type, ref bool fail)

Allows you to determine what happens when the wall at the given coordinates is killed or hit with a hammer. Fail determines whether the wall is mined (whether it is killed).

### public virtual bool CanExplode(int i, int j, int type)

Whether or not the wall at the given coordinates can be killed by an explosion (ie. bombs). Returns true by default; return false to stop an explosion from destroying it.

### public virtual void ModifyLight(int i, int j, int type, ref float r, ref float g, ref float b)

Allows you to determine how much light the wall emits. This can also let you light up the block in front of the wall.

### public virtual void RandomUpdate(int i, int j, int type)

Called for every wall the world randomly decides to update in a given tick. Useful for things such as growing or spreading.

### public virtual bool PreDraw(int i, int j, int type, SpriteBatch spriteBatch)

Allows you to draw things behind the wall at the given coordinates. Return false to stop the game from drawing the wall normally. Returns true by default.

### public virtual void PostDraw(int i, int j, int type, SpriteBatch spriteBatch)

Allows you to draw things in front of the wall at the given coordinates.