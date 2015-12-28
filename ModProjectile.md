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

Determines which type of vanilla projectile this ModProjectile will copy the behavior (AI) of. Leave as 0 to not copy any behavior. Defaults to 0.

### public int cooldownSlot

Determines which of the player's cooldown counters to use (-1, 0, or 1) when this projectile damages it. Defaults to -1.

### public int drawOffsetX

How far to the right of its position this projectile should be drawn. Defaults to 0.

### public int drawOriginOffsetY

The vertical origin offset from the projectile's center when it is drawn. The origin is essentially the point of rotation. This field will also determine the vertical drawing offset of the projectile.

### public float drawOriginOffsetX

The horizontal origin offset from the projectile's center when it is drawn.

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

### public virtual void SendExtraAI(BinaryWriter writer)

If you are storing AI information outside of the projectile.ai array, use this to send that AI information between clients and servers.

### public virtual void ReceiveExtraAI(BinaryReader reader)

Use this to receive information that was sent in SendExtraAI.

### public virtual void TileCollideStyle(ref int width, ref int height, ref bool fallThrough)

Allows you to determine how this projectile interacts with tiles. Width and height determine the projectile's hitbox for tile collision, and default to -1. Leave them as -1 to use the projectile's real size. Fallthrough determines whether the projectile can fall through platforms, etc., and defaults to true.

### public virtual bool OnTileCollide(Vector2 oldVelocity)

Allows you to determine what happens when this projectile collides with a tile. OldVelocity is the velocity before tile collision. The velocity that takes tile collision into account can be found with projectile.velocity. Return true to allow the vanilla tile collision code to take place (which normally kills the projectile). Returns true by default.

### public virtual bool PreKill(int timeLeft)

Allows you to determine whether the vanilla code for Kill and the Kill hook will be called. Return false to stop them from being called. Returns true by default. Note that this does _not_ stop the projectile from dying.

### public virtual void Kill(int timeLeft)

Allows you to control what happens when this projectile is killed (for example, creating dust or making sounds).

### public virtual bool? CanHitNPC(NPC target)

Allows you to determine whether this projectile can hit the given NPC. Return true to allow hitting the target, return false to block this projectile from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(NPC target, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that this projectile does to an NPC.

### public virtual void OnHitNPC(NPC target, int damage, float knockback, bool crit)

Allows you to create special effects when this projectile hits an NPC (for example, inflicting debuffs).

### public virtual bool CanHitPvp(Player target)

Allows you to determine whether this projectile can hit the given opponent player. Return false to block this projectile from hitting the target. Returns true by default.

### public virtual void ModifyHitPvp(Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that this projectile does to an opponent player.

### public virtual void OnHitPvp(Player target, int damage, bool crit)

Allows you to create special effects when this projectile hits an opponent player.

### public virtual bool CanHitPlayer(Player target)

Allows you to determine whether this hostile projectile can hit the given player. Return false to block this projectile from hitting the target. Returns true by default.

### public virtual void ModifyHitPlayer(Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that this hostile projectile does to a player.

### public virtual void OnHitPlayer(Player target, int damage, bool crit)

Allows you to create special effects when this hostile projectile hits a player.

### public virtual bool? Colliding(Rectangle projHitbox, Rectangle targetHitbox)

Allows you to use custom collision detection between this projectile and a player or NPC that this projectile can damage. Useful for things like diagonal lasers, projectiles that leave a trail behind them, etc.

### public virtual Color? GetAlpha(Color lightColor)

Allows you to determine the color and transparency in which this projectile is drawn. Return null to use the default color (normally light and buff color). Returns null by default.

### public virtual bool PreDrawExtras(SpriteBatch spriteBatch)

Allows you to draw things behind this projectile. Returns false to stop the game from drawing extras textures related to the projectile (for example, then chains for grappling hooks), useful if you're manually drawing the extras. Returns true by default.

### public virtual bool PreDraw(SpriteBatch spriteBatch, Color lightColor)

Allows you to draw things behind this projectile, or to modify the way this projectile is drawn. Return false to stop the game from drawing the projectile (useful if you're manually drawing the projectile). Returns true by default.

### public virtual void PostDraw(SpriteBatch spriteBatch, Color lightColor)

Allows you to draw things in front of this projectile. This method is called even if PreDraw returns false.

## Grappling-Related Methods

### public virtual bool? CanUseGrapple(Player player)

### public virtual bool? SingleGrappleHook(Player player)

### public virtual void UseGrapple(Player player, ref int type)

### public virtual float GrappleRange()

### public virtual void NumGrappleHooks(Player player, ref int numHooks)

### public virtual void GrappleRetreatSpeed(Player player, ref float speed)