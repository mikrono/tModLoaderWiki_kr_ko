# ProjectileLoader

This serves as the central class from which projectile-related functions are carried out. It also stores a list of mod projectiles by ID.

## Methods

### public static ModProjectile GetProjectile(int type)

Gets the ModProjectile instance corresponding to the specified type.

### public static void ProjectileAI(Projectile projectile)

### public static bool PreAI(Projectile projectile)

### public static void AI(Projectile projectile)

### public static void PostAI(Projectile projectile)

### public static void SendExtraAI(Projectile projectile, BinaryWriter writer, ref BitsByte flags)

### public static byte[] ReadExtraAI(BinaryReader reader, BitsByte flags)

### public static void ReceiveExtraAI(Projectile projectile, byte[] extraAI)

### public static bool ShouldUpdatePosition(Projectile projectile)

### public static void TileCollideStyle(Projectile projectile, ref int width, ref int height, ref bool fallThrough)

### public static bool OnTileCollide(Projectile projectile, Vector2 oldVelocity)

### public static bool PreKill(Projectile projectile, int timeLeft)

### public static void Kill(Projectile projectile, int timeLeft)

### public static bool CanDamage(Projectile projectile)

### public static bool MinionContactDamage(Projectile projectile)

### public static bool? CanHitNPC(Projectile projectile, NPC target)

### public static void ModifyHitNPC(Projectile projectile, NPC target, ref int damage, ref float knockback, ref bool crit)

### public static void OnHitNPC(Projectile projectile, NPC target, int damage, float knockback, bool crit)

### public static bool CanHitPvp(Projectile projectile, Player target)

### public static void ModifyHitPvp(Projectile projectile, Player target, ref int damage, ref bool crit)

### public static void OnHitPvp(Projectile projectile, Player target, int damage, bool crit)

### public static bool CanHitPlayer(Projectile projectile, Player target)

### public static void ModifyHitPlayer(Projectile projectile, Player target, ref int damage, ref bool crit)

### public static void OnHitPlayer(Projectile projectile, Player target, int damage, bool crit)

### public static bool? Colliding(Projectile projectile, Rectangle projHitbox, Rectangle targetHitbox)

### public static void DrawHeldProjInFrontOfHeldItemAndArms(Projectile projectile, ref bool flag)

### public static Color? GetAlpha(Projectile projectile, Color lightColor)

### public static void DrawOffset(Projectile projectile, ref int offsetX, ref int offsetY, ref float originX)

### public static bool PreDrawExtras(Projectile projectile, SpriteBatch spriteBatch)

### public static bool PreDraw(Projectile projectile, SpriteBatch spriteBatch, Color lightColor)

### public static void PostDraw(Projectile projectile, SpriteBatch spriteBatch, Color lightColor)

### public virtual bool? CanCutTiles()

### public static bool? CanUseGrapple(int type, Player player)

### public static bool? SingleGrappleHook(int type, Player player)

### public static void UseGrapple(Player player, ref int type)

### public static bool GrappleOutOfRange(float distance, Projectile projectile)

### public static void NumGrappleHooks(Projectile projectile, Player player, ref int numHooks)

### public static void GrappleRetreatSpeed(Projectile projectile, Player player, ref float speed)