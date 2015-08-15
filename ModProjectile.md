# ModProjectile

This class serves as a place for you to place all your properties and hooks for each projectile. Create instances of ModProjectile (preferably overriding this class) to pass as parameters to Mod.AddProjectile.

## Properties

### public Projectile projectile

The projectile object that this ModProjectile controls.

### public Mod mod

The mod that added this ModProjectile.

### public string Name

The internal name of this ModProjectile.

## Fields

### public int aiType

Determines which type of NPC this NPC will copy the behavior (AI) of. Leave as 0 to not copy any behavior. Defaults to 0.

### public int animationType

Determines which type of NPC this NPC will copy the animation (FindFrame) of. Leave as 0 to not copy any animation. Defaults to 0.

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically load a projectile instead of using Mod.AddProjectile. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this method to either force or stop an autoload, or to change the default display name and texture path.

### public virtual void SetDefaults()

Allows you to set all your projectile's properties, such as width, damage, aiStyle, penetrate, etc.

### public virtual bool PreAI()

Allows you to determine how this projectile behaves. Return false to stop the vanilla AI and the AI hook from being run. Returns true by default.

### public virtual void AI()

Allows you to determine how this projectile behaves. This will only be called if PreAI returns true.

### public virtual void PostAI()

Allows you to determine how this projectile behaves. This will be called regardless of what PreAI returns.