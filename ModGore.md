# ModGore

This class represents a type of gore that is added by a mod. Only one instance of this class will ever exist for each type of gore you add.

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