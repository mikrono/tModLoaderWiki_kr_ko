# GlobalNPC

This class allows you to modify and use hooks for all NPCs, including vanilla mobs. Create an instance of an overriding class then call Mod.AddGlobalNPC to use this.

## Properties

### public Mod mod

The mod to which this GlobalNPC belongs.

### public string Name

The name of this GlobalNPC instance.

## Methods

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalNPC instead of using Mod.AddGlobalNPC. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void SetDefaults(NPC npc)

Allows you to set the properties of any and every NPC that gets created.

### public virtual void ScaleExpertStats(NPC npc, int numPlayers, float bossLifeScale)

Allows you to customize an NPC's stats in expert mode.

### public virtual bool PreAI(NPC npc)

Allows you to determine how any NPC behaves. Return false to stop the vanilla AI and the AI hook from being run. Returns true by default.

### public virtual void AI(NPC npc)

Allows you to determine how any NPC behaves. This will only be called if PreAI returns true.

### public virtual void PostAI(NPC npc)

Allows you to determine how any NPC behaves. This will be called regardless of what PreAI returns.

### public virtual void FindFrame(NPC npc, int frameHeight)

Allows you to modify the frame from an NPC's texture that is drawn, which is necessary in order to animate NPCs.

### public virtual void HitEffect(NPC npc, int hitDirection, double damage)

Allows you to make things happen whenever an NPC is hit, such as creating dust or gores.

### public virtual bool PreNPCLoot(NPC npc)

Allows you to determine whether or not the NPC will drop anything at all. Return false to stop the NPC from dropping anything. Returns true by default.

### public virtual void NPCLoot(NPC npc)

Allows you to add drops to an NPC when it dies.

### public virtual bool CanHitPlayer(NPC npc, Player target, ref int cooldownSlot)

Allows you to determine whether an NPC can hit the given player. Return false to block the NPC from hitting the target. Returns true by default. CooldownSlot determines which of the player's cooldown counters to use (-1, 0, or 1), and defaults to -1.

### public virtual void ModifyHitPlayer(NPC npc, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that an NPC does to a player.

### public virtual void OnHitPlayer(NPC npc, Player target, int damage, bool crit)

Allows you to create special effects when an NPC hits a player (for example, inflicting debuffs).

### public virtual bool? CanHitNPC(NPC npc, NPC target)

Allows you to determine whether an NPC can hit the given friendly NPC. Return true to allow hitting the target, return false to block the NPC from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(NPC npc, NPC target, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that an NPC does to a friendly NPC.

### public virtual void OnHitNPC(NPC npc, NPC target, int damage, float knockback, bool crit)

Allows you to create special effects when an NPC hits a friendly NPC.

### public virtual bool? CanBeHitByItem(NPC npc, Player player, Item item)

Allows you to determine whether an NPC can be hit by the given melee weapon when swung. Return true to allow hitting the NPC, return false to block hitting the NPC, and return null to use the vanilla code for whether the NPC can be hit. Returns null by default.

### public virtual void ModifyHitByItem(NPC npc, Player player, Item item, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that an NPC takes from a melee weapon.

### public virtual void OnHitByItem(NPC npc, Player player, Item item, int damage, float knockback, bool crit)

Allows you to create special effects when an NPC is hit by a melee weapon.

### public virtual bool? CanBeHitByProjectile(NPC npc, Projectile projectile)

Allows you to determine whether an NPC can be hit by the given projectile. Return true to allow hitting the NPC, return false to block hitting the NPC, and return null to use the vanilla code for whether the NPC can be hit. Returns null by default.

### public virtual void ModifyHitByProjectile(NPC npc, Projectile projectile, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that an NPC takes from a projectile.

### public virtual void OnHitByProjectile(NPC npc, Projectile projectile, int damage, float knockback, bool crit)

Allows you to create special effects when an NPC is hit by a projectile.

### public virtual bool StrikeNPC(NPC npc, ref double damage, int defense, ref float knockback, int hitDirection, ref bool crit)

Allows you to use a custom damage formula for when an NPC takes damage from any source. For example, you can change the way defense works or use a different crit multiplier. Return false to stop the game from running the vanilla damage formula; returns true by default.

### public virtual void BossHeadSlot(NPC npc, ref int index)

Allows you to customize the boss head texture used by an NPC based on its state.

### public virtual void BossHeadRotation(NPC npc, ref float rotation)

Allows you to customize the rotation of an NPC's boss head icon on the map.

### public virtual void BossHeadSpriteEffects(NPC npc, ref SpriteEffects spriteEffects)

Allows you to flip an NPC's boss head icon on the map.

### public virtual Color? GetAlpha(NPC npc, Color drawColor)

Allows you to determine the color and transparency in which an NPC is drawn. Return null to use the default color (normally light and buff color). Returns null by default.

### public virtual bool PreDraw(NPC npc, SpriteBatch spriteBatch, Color drawColor)

Allows you to draw things behind an NPC, or to modify the way the NPC is drawn. Return false to stop the game from drawing the NPC (useful if you're manually drawing the NPC). Returns true by default.

### public virtual void PostDraw(NPC npc, SpriteBatch spriteBatch, Color drawColor)

Allows you to draw things in front of this NPC. This method is called even if PreDraw returns false.

### public virtual void EditSpawnRate(Player player, ref int spawnRate, ref int maxSpawns)

Allows you to modify the chance of NPCs spawning around the given player and the maximum number of NPCs that can spawn around the player. Lower spawnRates mean a higher chance for NPCs to spawn.

### public virtual void EditSpawnRange(Player player, ref int spawnRangeX, ref int spawnRangeY, ref int safeRangeX, ref int safeRangeY)

Allows you to modify the range at which NPCs can spawn around the given player. The spawnRanges determine that maximum distance NPCs can spawn from the player, and the safeRanges determine the minimum distance.

### public virtual void EditSpawnPool(IDictionary\<int, float\> pool, NPCSpawnInfo spawnInfo)

Allows you to control which NPCs can spawn and how likely each one is to spawn. The pool parameter maps NPC types to their spawning weights (likelihood to spawn compared to other NPCs). A type of 0 in the pool represents the default vanilla NPC spawning.

### public virtual void SpawnNPC(int npc, int tileX, int tileY)

Allows you to customize an NPC (for example, its position or ai array) after it naturally spawns and before it is synced between servers and clients.

### public virtual void GetChat(NPC npc, ref string chat)

Allows you to modify the chat message of any town NPC that the player talks to.

### public virtual void SetupShop(int type, Chest shop, ref int nextSlot)

Allows you to add items to a town NPC's shop. The type parameter is the type of the NPC that this shop belongs to. Add an item by setting the defaults of shop.item[nextSlot] then incrementing nextSlot. In the end, nextSlot must have a value of 1 greater than the highest index in shop.item that contains an item. If you want to remove an item, you will have to be familiar with programming.

### public virtual void SetupTravelShop(int[] shop, ref int nextSlot)

Allows you to add items to the travelling merchant's shop. Add an item by setting shop[nextSlot] to the ID of the item you are adding then incrementing nextSlot. In the end, nextSlot must have a value of 1 greater than the highest index in shop that represents an item ID. If you want to remove an item, you will have to be familiar with programming.