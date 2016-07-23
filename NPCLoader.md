# NPCLoader

This serves as the central class from which NPC-related functions are carried out. It also stores a list of mod NPCs by ID.

## Fields

### public static readonly IList\<int\> blockLoot

Allows you to stop an NPC from dropping loot by adding item IDs to this list. This list will be cleared whenever NPCLoot ends. Useful for either removing an item or change the drop rate of an item in the NPC's loot table. To change the drop rate of an item, use the PreNPCLoot hook, spawn the item yourself, then add the item's ID to this list.

## Methods

### public static ModNPC GetNPC(int type)

Gets the ModNPC instance corresponding to the specified type.

### public static string DisplayName(int type)

### public static void ScaleExpertStats(NPC npc, int numPlayers, float bossLifeScale)

### public static void ResetEffects(NPC npc)

### public static void NPCAI(NPC npc)

### public static bool PreAI(NPC npc)

### public static void AI(NPC npc)

### public static void PostAI(NPC npc)

### public static void SendExtraAI(NPC npc, BinaryWriter writer)

### public static void ReceiveExtraAI(NPC npc, BinaryReader reader)

### public static void FindFrame(NPC npc, int frameHeight)

### public static void HitEffect(NPC npc, int hitDirection, double damage)

### public static void UpdateLifeRegen(NPC npc, ref int damage)

### public static bool CheckActive(NPC npc)

### public static bool CheckDead(NPC npc)

### public static bool PreNPCLoot(NPC npc)

### public static void NPCLoot(NPC npc)

### public static void BossLoot(NPC npc, ref string name, ref int potionType)

### public static void BossBag(NPC npc, ref int bagType)

### public static bool CanHitPlayer(NPC npc, Player target, ref int cooldownSlot)

### public static void ModifyHitPlayer(NPC npc, Player target, ref int damage, ref bool crit)

### public static void OnHitPlayer(NPC npc, Player target, int damage, bool crit)

### public static bool? CanHitNPC(NPC npc, NPC target)

### public static void ModifyHitNPC(NPC npc, NPC target, ref int damage, ref float knockback, ref bool crit)

### public static void OnHitNPC(NPC npc, NPC target, int damage, float knockback, bool crit)

### public static bool? CanBeHitByItem(NPC npc, Player player, Item item)

### public static void ModifyHitByItem(NPC npc, Player player, Item item, ref int damage, ref float knockback, ref bool crit)

### public static void OnHitByItem(NPC npc, Player player, Item item, int damage, float knockback, bool crit)

### public static bool? CanBeHitByProjectile(NPC npc, Projectile projectile)

### public static void ModifyHitByProjectile(NPC npc, Projectile projectile, ref int damage, ref float knockback, ref bool crit)

### public static void OnHitByProjectile(NPC npc, Projectile projectile, int damage, float knockback, bool crit)

### public static bool StrikeNPC(NPC npc, ref double damage, int defense, ref float knockback, int hitDirection, ref bool crit)

### public static void BossHeadSlot(NPC npc, ref int index)

### public static void BossHeadRotation(NPC npc, ref float rotation)

### public static void BossHeadSpriteEffects(NPC npc, ref SpriteEffects spriteEffects)

### public static Color? GetAlpha(NPC npc, Color lightColor)

### public static void DrawEffects(NPC npc, ref Color drawColor)

### public static bool PreDraw(NPC npc, SpriteBatch spriteBatch, Color drawColor)

### public static void PostDraw(NPC npc, SpriteBatch spriteBatch, Color drawColor)

### public static void EditSpawnRate(Player player, ref int spawnRate, ref int maxSpawns)

### public static void EditSpawnRange(Player player, ref int spawnRangeX, ref int spawnRangeY, ref int safeRangeX, ref int safeRangeY)

### public static int? ChooseSpawn(NPCSpawnInfo spawnInfo)

### public static int SpawnNPC(int type, int tileX, int tileY)

### public static void CanTownNPCSpawn(int numTownNPCs, int money)

### public static bool CheckConditions(int type)

### public static string TownNPCName(int type)

### public static bool UsesPartyHat(NPC npc)

### public static void GetChat(NPC npc, ref string chat)

### public static void SetChatButtons(ref string button, ref string button2)

### public static void OnChatButtonClicked(bool firstButton)

### public static void SetupShop(int type, Chest shop, ref int nextSlot)

### public static void SetupTravelShop(int[] shop, ref int nextSlot)

### public static void BuffTownNPC(ref float damageMult, ref int defense)

### public static void TownNPCAttackStrength(NPC npc, ref int damage, ref float knockback)

### public static void TownNPCAttackCooldown(NPC npc, ref int cooldown, ref int randExtraCooldown)

### public static void TownNPCAttackProj(NPC npc, ref int projType, ref int attackDelay)

### public static void TownNPCAttackProjSpeed(NPC npc, ref float multiplier, ref float gravityCorrection, ref float randomOffset)

### public static void TownNPCAttackShoot(NPC npc, ref bool inBetweenShots)

### public static void TownNPCAttackMagic(NPC npc, ref float auraLightMultiplier)

### public static void TownNPCAttackSwing(NPC npc, ref int itemWidth, ref int itemHeight)

### public static void DrawTownAttackGun(NPC npc, ref float scale, ref int item, ref int closeness)

### public static void DrawTownAttackSwing(NPC npc, ref Texture2D item, ref int itemSize, ref float scale, ref Vector2 offset)