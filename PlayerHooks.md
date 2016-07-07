# PlayerHooks

This is where all ModPlayer hooks are gathered and called.

## Methods

### public static void ResetEffects(Player player)

### public static void UpdateDead(Player player)

### public static IList<Item> SetupStartInventory(Player player)

### public static void SetStartInventory(Player player, IList<Item> items)

### public static void SetStartInventory(Player player)

### public static void UpdateBiomes(Player player)

### public static bool CustomBiomesMatch(Player player, Player other)

### public static void CopyCustomBiomesTo(Player player, Player other)

### public static void SendCustomBiomes(Player player, BinaryWriter writer)

### public static void ReceiveCustomBiomes(Player player, BinaryReader reader)

### public static void UpdateBiomeVisuals(Player player)

### public static Texture2D GetMapBackgroundImage(Player player)

### public static void UpdateBadLifeRegen(Player player)

### public static void UpdateLifeRegen(Player player)

### public static void NaturalLifeRegen(Player player, ref float regen)

### public static void PreUpdate(Player player)

### public static void SetControls(Player player)

### public static void PreUpdateBuffs(Player player)

### public static void PostUpdateBuffs(Player player)

### public static void PostUpdateEquips(Player player)

### public static void PostUpdateMiscEffects(Player player)

### public static void PostUpdateRunSpeeds(Player player)

### public static void PostUpdate(Player player)

### public static void FrameEffects(Player player)

### public static bool PreHurt(Player player, bool pvp, bool quiet, ref int damage, ref int hitDirection, ref bool crit, ref bool customDamage, ref bool playSound, ref bool genGore, ref string deathText)

### public static void Hurt(Player player, bool pvp, bool quiet, double damage, int hitDirection, bool crit)

### public static void PostHurt(Player player, bool pvp, bool quiet, double damage, int hitDirection, bool crit)

### public static bool PreKill(Player player, double damage, int hitDirection, bool pvp, ref bool playSound, ref bool genGore, ref string deathText)

### public static void Kill(Player player, double damage, int hitDirection, bool pvp, string deathText)

### public static bool PreItemCheck(Player player)

### public static void PostItemCheck(Player player)

### public static void GetWeaponDamage(Player player, Item item, ref int damage)

### public static void GetWeaponKnockback(Player player, Item item, ref float knockback)

### public static bool ConsumeAmmo(Player player, Item weapon, Item ammo)

### public static bool Shoot(Player player, Item item, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

### public static void MeleeEffects(Player player, Item item, Rectangle hitbox)

### public static void OnHitAnything(Player player, float x, float y, Entity victim)

### public static bool? CanHitNPC(Player player, Item item, NPC target)

### public static void ModifyHitNPC(Player player, Item item, NPC target, ref int damage, ref float knockback, ref bool crit)

### public static void OnHitNPC(Player player, Item item, NPC target, int damage, float knockback, bool crit)

### public static bool? CanHitNPCWithProj(Projectile proj, NPC target)

### public static void ModifyHitNPCWithProj(Projectile proj, NPC target, ref int damage, ref float knockback, ref bool crit)

### public static void OnHitNPCWithProj(Projectile proj, NPC target, int damage, float knockback, bool crit)

### public static bool CanHitPvp(Player player, Item item, Player target)

### public static void ModifyHitPvp(Player player, Item item, Player target, ref int damage, ref bool crit)

### public static void OnHitPvp(Player player, Item item, Player target, int damage, bool crit)

### public static bool CanHitPvpWithProj(Projectile proj, Player target)

### public static void ModifyHitPvpWithProj(Projectile proj, Player target, ref int damage, ref bool crit)

### public static void OnHitPvpWithProj(Projectile proj, Player target, int damage, bool crit)

### public static bool CanBeHitByNPC(Player player, NPC npc, ref int cooldownSlot)

### public static void ModifyHitByNPC(Player player, NPC npc, ref int damage, ref bool crit)

### public static void OnHitByNPC(Player player, NPC npc, int damage, bool crit)

### public static bool CanBeHitByProjectile(Player player, Projectile proj)

### public static void ModifyHitByProjectile(Player player, Projectile proj, ref int damage, ref bool crit)

### public static void OnHitByProjectile(Player player, Projectile proj, int damage, bool crit)

### public static void CatchFish(Player player, Item fishingRod, int liquidType, int poolSize, int worldLayer, int questFish, ref int caughtType, ref bool junk)

### public static void GetFishingLevel(Player player, Item fishingRod, Item bait, ref int fishingLevel)

### public static void AnglerQuestReward(Player player, float rareMultiplier, List<Item> rewardItems)

### public static void GetDyeTraderReward(Player player, List<int> rewardPool)

### public static void DrawEffects(PlayerDrawInfo drawInfo, ref float r, ref float g, ref float b, ref float a, ref bool fullBright)

### public static List<PlayerLayer> GetDrawLayers(Player drawPlayer)

### public static List<PlayerHeadLayer> GetDrawHeadLayers(Player drawPlayer)

### public static void ModifyScreenPosition(Player player)

### public static void ModifyZoom(Player player, ref float zoom)