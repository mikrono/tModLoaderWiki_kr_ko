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

### public virtual bool PreKill(Projectile projectile, int timeLeft)

Allows you to determine whether the vanilla code for Kill and the Kill hook will be called. Return false to stop them from being called. Returns true by default. Note that this does not stop the projectile from dying.

### public virtual void Kill(Projectile projectile, int timeLeft)

Allows you to control what happens when a projectile is killed (for example, creating dust or making sounds).

### public virtual bool? CanHitNPC(Projectile projectile, NPC target)

Allows you to determine whether a projectile can hit the given NPC. Return true to allow hitting the target, return false to block the projectile from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(Projectile projectile, NPC target, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that a projectile does to an NPC.

### public virtual void OnHitNPC(Projectile projectile, NPC target, int damage, float knockback, bool crit)

Allows you to create special effects when a projectile hits an NPC (for example, inflicting debuffs).

### public virtual bool CanHitPvp(Projectile projectile, Player target)

Allows you to determine whether a projectile can hit the given opponent player. Return false to block the projectile from hitting the target. Returns true by default.

### public virtual void ModifyHitPvp(Projectile projectile, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that a projectile does to an opponent player.

### public virtual void OnHitPvp(Projectile projectile, Player target, int damage, bool crit)

Allows you to create special effects when a projectile hits an opponent player.

### public virtual bool CanHitPlayer(Projectile projectile, Player target)

Allows you to determine whether a hostile projectile can hit the given player. Return false to block the projectile from hitting the target. Returns true by default.

### public virtual void ModifyHitPlayer(Projectile projectile, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that a hostile projectile does to a player.

### public virtual void OnHitPlayer(Projectile projectile, Player target, int damage, bool crit)

Allows you to create special effects when a hostile projectile hits a player.

### public virtual bool? Colliding(Projectile projectile, Rectangle projHitbox, Rectangle targetHitbox)

Allows you to use custom collision detection between a projectile and a player or NPC that the projectile can damage. Useful for things like diagonal lasers, projectiles that leave a trail behind them, etc.

### public virtual Color? GetAlpha(Projectile projectile, Color lightColor)

Allows you to determine the color and transparency in which a projectile is drawn. Return null to use the default color (normally light and buff color). Returns null by default.

### public virtual bool PreDrawExtras(Projectile projectile, SpriteBatch spriteBatch)

Allows you to draw things behind a projectile. Returns false to stop the game from drawing extras textures related to the projectile (for example, the chains for grappling hooks), useful if you're manually drawing the extras. Returns true by default.

### public virtual bool PreDraw(Projectile projectile, SpriteBatch spriteBatch, Color lightColor)

Allows you to draw things behind a projectile, or to modify the way the projectile is drawn. Return false to stop the game from drawing the projectile (useful if you're manually drawing the projectile). Returns true by default.

### public virtual void PostDraw(Projectile projectile, SpriteBatch spriteBatch, Color lightColor)

Allows you to draw things in front of a projectile. This method is called even if PreDraw returns false.

## Grappling-Related Methods

### public virtual bool? CanUseGrapple(int type, Player player)

Whether or not a grappling hook that shoots the given type of projectile can be used by the given player. Return null to use the vanilla code. Returns null by default.

### public virtual bool? SingleGrappleHook(int type, Player player)

Whether or not a grappling hook can only have one hook per player in the world at a time. Return null to use the vanilla code. Returns null by default.

### public virtual void UseGrapple(Player player, ref int type)

This code is called whenever the player uses a grappling hook that shoots the given type of projectile. Use it to change what kind of hook is fired (for example, the Dual Hook does this), to kill old hook projectiles, etc.

### public virtual void NumGrappleHooks(Projectile projectile, Player player, ref int numHooks)

How many of the given type of grappling hook the given player can latch onto blocks before the hooks start disappearing. Change the numHooks parameter to determine this.

### public virtual void GrappleRetreatSpeed(Projectile projectile, Player player, ref float speed)

Use this to change the speed with which a grappling hook retracts back towards the player when it travels too far without finding a block.