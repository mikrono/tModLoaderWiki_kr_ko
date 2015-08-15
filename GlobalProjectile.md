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

### public virtual bool PreAI(Projectile projectile)

Allows you to determine how any projectile behaves. Return false to stop the vanilla AI and the AI hook from being run. Returns true by default.

### public virtual void AI(Projectile projectile)

Allows you to determine how any projectile behaves. This will only be called if PreAI returns true.

### public virtual void PostAI(Projectile projectile)

Allows you to determine how any projectile behaves. This will be called regardless of what PreAI returns.

### public virtual void TileCollideStyle(Projectile projectile, ref int width, ref int height, ref bool fallThrough)

Allows you to determine how a projectile interacts with tiles. Width and height determine the projectile's hitbox for tile collision, and default to -1. Leave them as -1 to use the projectile's real size. Fallthrough determines whether the projectile can fall through platforms, etc., and defaults to true.

### public virtual bool OnTileCollide(Projectile projectile, Vector2 oldVelocity)

Allows you to determine what happens when a projectile collides with a tile. OldVelocity is the velocity before tile collision. The velocity that takes tile collision into account can be found with projectile.velocity. Return true to allow the vanilla tile collision code to take place (which normally kills the projectile). Returns true by default.