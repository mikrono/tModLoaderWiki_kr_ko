# ModPlayer

A ModPlayer instance represents an extension of a Player instance. You can store fields in the ModPlayer classes, much like how the Player class abuses field usage, to keep track of mod-specific information on the player that a ModPlayer instance represents. It also contains hooks to insert your code into the Player class.

## Properties

### public Mod mod

The mod that added this type of ModPlayer.

### public string Name

The name of this ModPlayer. Used for distinguishing between multiple ModPlayers added by a single Mod, in addition to the argument passed to Player.GetModPlayer.

### public Player player

The Player instance that this ModPlayer instance is attached to.

### public virtual bool Autoload(ref string name)

Allows you to automatically add a ModPlayer instead of using Mod.AddPlayer. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this to either force or stop an autoload, or change the name that identifies this type of ModPlayer.

### public virtual void ResetEffects()

This is where you reset any fields you add to your ModPlayer subclass to their default states. This is necessary in order to reset your fields if they are conditionally set by a tick update but the condition is no longer satisfied.

### public virtual void SaveCustomData(BinaryWriter writer)

Allows you to save custom data for this player. You are only able to save up to 64 KB of information (I don't imagine anyone will ever need more than that).

### public virtual void LoadCustomData(BinaryReader reader)

Allows you to load custom data you have saved for this player.

### public virtual void OnHitNPC(float x, float y, NPC target)

Allows you to make things happen when an NPC is hit. x and y are where the NPC was hit.

### public virtual bool CanHitNPC(NPC npc)

Allows you to determine whether the specified NPC can be hit by the player.

### public virtual void UpdateBiomes()

Allows you to make things happen when the player enters a biome. Biome List here: (https://github.com/bluemagic123/tModLoader/wiki/BiomeList)

### public virtual void UpdateBiomeVisuals(string biomeName, bool inZone, Vector2 activationSource)

Allows you to make visual things happen when UpdateBiome is called.

### public virtual void UpdateDead()

Allows you to make things happen when the player dies.