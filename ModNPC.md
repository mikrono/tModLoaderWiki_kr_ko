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

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically load an NPC instead of using Mod.AddNPC. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this method to either force or stop an autoload, or to change the default display name and texture path.

### public virtual void SetDefaults()

Allows you to set all your NPC's properties, such as width, damage, aiStyle, lifeMax, etc.

### public virtual bool PreAI()

Allows you to determine how this NPC behaves. Return false to stop the vanilla AI and the AI hook from being run. Returns true by default.

### public virtual void AI()

Allows you to determine how this NPC behaves. This will only be called if PreAI returns true.

### public virtual void PostAI()

Allows you to determine how this NPC behaves. This will be called regardless of what PreAI returns.

### public virtual void FindFrame(int frameHeight)

Allows you to modify the frame from this NPC's texture that is drawn, which is necessary in order to animate NPCs.

### public virtual void HitEffect(int hitDirection, double damage)

Allows you to make things happen whenever this NPC is hit, such as creating dust or gores.

### public virtual bool PreNPCLoot(NPC npc)

Allows you to determine whether or not this NPC will drop anything at all. Return false to stop the NPC from dropping anything. Returns true by default.

### public virtual void NPCLoot(NPC npc)

Allows you to make things happen when this NPC dies (for example, dropping items).

### public virtual void BossLoot(ref string name, ref int potionType)

Allows you to customize what happens when this boss dies, such as which name is displayed in the defeat message and what type of potion it drops.