# ModGore

This class represents a type of gore that is added by a mod. Only one instance of this class will ever exist for each type of gore you add.

## Properties

### public string Name

The internal name of this type of gore.

### public int Type

The internal ID of this type of gore.

### public Mod mod

The mod which has added this type of gore.

## Methods

### public static ModGore GetGore(int type)

Gets the ModGore instance corresponding to the specified type.

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to modify the name and texture path of this gore when it is autoloaded. Return true to autoload this gore. When a gore is autoloaded, that means you do not need to manually call Mod.AddGore. By default returns the mod's autoload property.

### public virtual void OnSpawn(Gore gore)

Allows you to modify a gore's fields when it is created.

### public virtual bool Update(Gore gore)

Allows you to customize how you want this type of gore to behave. Return true to allow for vanilla gore updating to also take place; returns true by default.

### public virtual Color? GetAlpha(Gore gore, Color lightColor)

Allows you to override the color this gore will draw in. Return null to draw it in the normal light color; returns null by default.

### public virtual bool DrawBehind(Gore gore)

Allows you to determine whether or not this gore will draw behind tiles, etc. Returns false by default.