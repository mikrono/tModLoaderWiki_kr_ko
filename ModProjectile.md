# ModProjectile

This class serves as a place for you to place all your properties and hooks for each projectile. Create instances of ModProjectile (preferably overriding this class) to pass as parameters to Mod.AddProjectile.

## Properties

### public Projectile projectile

The projectile object that this projectile controls.

### public Mod mod

The mod that added this ModProjectile.

### public string Name

The internal name of this ModProjectile.

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically load a projectile instead of using Mod.AddProjectile. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this method to either force or stop an autoload, or to change the default display name and texture path.

### public virtual void SetDefaults()

Allows you to set all your projectile's properties, such as width, damage, aiStyle, frameCount, etc.