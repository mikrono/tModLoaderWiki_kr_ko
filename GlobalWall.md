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