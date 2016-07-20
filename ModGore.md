# ModGore

This class allows you to customize the behavior of a custom gore. Create a new instance of this and pass it as a parameter to Mod.AddGore to customize the gore you are adding. If you are autoloading gore, then give it the same name as the gore texture.

## Fields

### public int updateType

Allows you to copy the Update behavior of a different type of gore. This defaults to 0, which means no behavior is copied.

## Methods

### public static int GetGoreSlot(string texture)

Gets the type of the custom gore corresponding to the given texture. Returns 0 if the texture does not represent a gore.

### public virtual void OnSpawn(Gore gore)

Allows you to modify a gore's fields when it is created.

### public virtual bool Update(Gore gore)

Allows you to customize how you want this type of gore to behave. Return true to allow for vanilla gore updating to also take place; returns true by default.

### public virtual Color? GetAlpha(Gore gore, Color lightColor)

Allows you to override the color this gore will draw in. Return null to draw it in the normal light color; returns null by default.

### public virtual bool DrawBehind(Gore gore)

Allows you to determine whether or not this gore will draw behind tiles, etc. Returns false by default.