# GlobalProjectile

This class allows you to modify and use hooks for all projectiles, including vanilla projectiles. Create an instance of an overriding class then call Mod.AddGlobalProjectile to use this.

## Properties

### public Mod mod

The mod to which this GlobalProjectile belongs.

### public string Name

The name of this GlobalProjectile instance.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalProjectile instead of using Mod.AddGlobalProjectile. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void SetDefaults(Projectile projectile)

Allows you to set the properties of any and every projectile that gets created.