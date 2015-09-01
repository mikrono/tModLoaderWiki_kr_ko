# ModNPC

This class serves as a place for you to place all your properties and hooks for each NPC. Create instances of ModNPC (preferably overriding this class) to pass as parameters to Mod.AddNPC.

## Properties

### public NPC npc

The NPC object that this ModNPC controls.

### public Mod mod

The mod that added this ModNPC.

## Fields

### public int aiType

Determines which type of vanilla NPC this ModNPC will copy the behavior (AI) of. Leave as 0 to not copy any behavior. Defaults to 0.

### public int animationType

Determines which type of vanilla NPC this ModNPC will copy the animation (FindFrame) of. Leave as 0 to not copy any animation. Defaults to 0.

### public int bossBag

The item type of the boss bag that is dropped when DropBossBags is called for this NPC.

### public int music

The ID of the music that plays when this NPC is on or near the screen. Defaults to -1, which means music plays normally.

### public float drawOffsetY

The vertical offset used for drawing this NPC. Defaults to 0.

### public int banner

The type of NPC that this NPC will be considered as when determining banner drops and banner bonuses. By default this will be 0, which means this NPC is not associated with any banner. To give your NPC its own banner, set this field to the NPC's type.

### public int bannerItem

The type of the item this NPC drops for every 50 times it is defeated. For any ModNPC whose banner field is set to the type of this NPC, that ModNPC will drop this banner.

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically load an NPC instead of using Mod.AddNPC. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this method to either force or stop an autoload, or to change the default display name and texture path.

### public virtual void AutoloadHead(ref string headTexture, ref string bossHeadTexture)

Allows you to control the path to this NPC's head texture when it is autoloaded. The headTexture defaults to the default NPC texture plus "_Head", and the bossHeadTexture defaults to the default headTexture plus "_Head_Boss". If the textures don't exist, the NPC will not be given a head texture. The headTexture should be used for town NPCs, and the bossHeadTexture should be used for any non-town NPC that you want to display on the map.

### public virtual void SetDefaults()

Allows you to set all your NPC's properties, such as width, damage, aiStyle, lifeMax, etc.

### public virtual void ScaleExpertStats(int numPlayers, float bossLifeScale)

Allows you to customize this NPC's stats in expert mode. This is useful because expert mode's doubling of damage and life might be too much sometimes (for example, with bosses). Also useful for scaling life with the number of players in the world.

### public virtual bool PreAI()

Allows you to determine how this NPC behaves. Return false to stop the vanilla AI and the AI hook from being run. Returns true by default.

### public virtual void AI()

Allows you to determine how this NPC behaves. This will only be called if PreAI returns true.

### public virtual void PostAI()

Allows you to determine how this NPC behaves. This will be called regardless of what PreAI returns.

### public virtual void SendExtraAI(BinaryWriter writer)

If you are storing AI information outside of the npc.ai array, use this to send that AI information between clients and servers.

### public virtual void ReceiveExtraAI(BinaryReader reader)

Use this to receive information that was sent in SendExtraAI.

### public virtual void FindFrame(int frameHeight)

Allows you to modify the frame from this NPC's texture that is drawn, which is necessary in order to animate NPCs.

### public virtual void HitEffect(int hitDirection, double damage)

Allows you to make things happen whenever this NPC is hit, such as creating dust or gores.

### public virtual bool PreNPCLoot()

Allows you to determine whether or not this NPC will drop anything at all. Return false to stop the NPC from dropping anything. Returns true by default.

### public virtual void NPCLoot()

Allows you to make things happen when this NPC dies (for example, dropping items).

### public virtual void BossLoot(ref string name, ref int potionType)

Allows you to customize what happens when this boss dies, such as which name is displayed in the defeat message and what type of potion it drops.

### public virtual bool CanHitPlayer(Player target, ref int cooldownSlot)

Allows you to determine whether this NPC can hit the given player. Return false to block this NPC from hitting the target. Returns true by default. CooldownSlot determines which of the player's cooldown counters to use (-1, 0, or 1), and defaults to -1.

### public virtual void ModifyHitPlayer(Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that this NPC does to a player.

### public virtual void OnHitPlayer(Player target, int damage, bool crit)

Allows you to create special effects when this NPC hits a player (for example, inflicting debuffs).

### public virtual bool? CanHitNPC(NPC target)

Allows you to determine whether this NPC can hit the given friendly NPC. Return true to allow hitting the target, return false to block this NPC from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(NPC target, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that this NPC does to a friendly NPC.

### public virtual void OnHitNPC(NPC target, int damage, float knockback, bool crit)

Allows you to create special effects when this NPC hits a friendly NPC.

### public virtual bool? CanBeHitByItem(Player player, Item item)

Allows you to determine whether this NPC can be hit by the given melee weapon when swung. Return true to allow hitting the NPC, return false to block hitting the NPC, and return null to use the vanilla code for whether the NPC can be hit. Returns null by default.

### public virtual void ModifyHitByItem(Player player, Item item, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that this NPC takes from a melee weapon.

### public virtual void OnHitByItem(Player player, Item item, int damage, float knockback, bool crit)

Allows you to create special effects when this NPC is hit by a melee weapon.

### public virtual bool? CanBeHitByProjectile(Projectile projectile)

Allows you to determine whether this NPC can be hit by the given projectile. Return true to allow hitting the NPC, return false to block hitting the NPC, and return null to use the vanilla code for whether the NPC can be hit. Returns null by default.

### public virtual void ModifyHitByProjectile(Projectile projectile, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that this NPC takes from a projectile.

### public virtual void OnHitByProjectile(Projectile projectile, int damage, float knockback, bool crit)

Allows you to create special effects when this NPC is hit by a projectile.

### public virtual bool StrikeNPC(ref double damage, int defense, ref float knockback, int hitDirection, ref bool crit)

Allows you to use a custom damage formula for when this NPC takes damage from any source. For example, you can change the way defense works or use a different crit multiplier. Return false to stop the game from running the vanilla damage formula; returns true by default.

### public virtual void BossHeadSlot(ref int index)

Allows you to customize the boss head texture used by this NPC based on its state.

### public virtual void BossHeadRotation(ref float rotation)

Allows you to customize the rotation of this NPC's boss head icon on the map.

### public virtual void BossHeadSpriteEffects(ref SpriteEffects spriteEffects)

Allows you to flip this NPC's boss head icon on the map.

### public virtual Color? GetAlpha(Color drawColor)

Allows you to determine the color and transparency in which this NPC is drawn. Return null to use the default color (normally light and buff color). Returns null by default.

### public virtual bool PreDraw(SpriteBatch spriteBatch, Color drawColor)

Allows you to draw things behind this NPC, or to modify the way this NPC is drawn. Return false to stop the game from drawing the NPC (useful if you're manually drawing the NPC). Returns true by default.

### public virtual void PostDraw(SpriteBatch spriteBatch, Color drawColor)

Allows you to draw things in front of this NPC. This method is called even if PreDraw returns false.

### public virtual float CanSpawn(NPCSpawnInfo spawnInfo)

Whether or not this NPC can spawn with the given spawning conditions. Return the weight for the chance of this NPC to spawn compared to vanilla mobs. Vanilla mobs have a weight of 1. Returns 0 by default, which disables natural spawning.

### public virtual int SpawnNPC(int tileX, int tileY)

Allows you to customize how this NPC is created when it naturally spawns (for example, its position or ai array). Return the return value of NPC.NewNPC. By default this method spawns this NPC on top of the tile at the given coordinates.

### public virtual bool CanTownNPCSpawn(int numTownNPCs, int money)

Whether or not the conditions have been met for this town NPC to be able to move into town. For example, the Demolitionist requires that any player has an explosive.

### public virtual bool CheckConditions(int left, int right, int top, int bottom)

Allows you to define special conditions required for this town NPC's house. For example, Truffle requires the house to be in an aboveground mushroom biome.

### public virtual string TownNPCName()

Allows you to give this town NPC any name when it spawns. By default returns something embarrassing.

### public virtual string GetChat()

Allows you to give this town NPC a chat message when a player talks to it. By default returns something embarrassing.