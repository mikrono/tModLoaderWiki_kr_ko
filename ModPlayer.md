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

### public virtual void UpdateDead()

Similar to UpdateDead, except this is only called when the player is dead. If this is called, then ResetEffects will not be called.

### public virtual void SaveCustomData(BinaryWriter writer)

Allows you to save custom data for this player. You are only able to save up to 64 KB of information (I don't imagine anyone will ever need more than that).

### public virtual void LoadCustomData(BinaryReader reader)

Allows you to load custom data you have saved for this player.

### public virtual void SetupStartInventory(IList\<Item\> items)

Allows you to modify the inventory newly created players or killed mediumcore players will start with. To add items to the player's inventory, create a new Item, call its SetDefaults method for whatever ID you want, call its Prefix method with a parameter of -1 if you want to give it a random prefix, then add it to the items list parameter.

### public virtual void UpdateBiomes()

Allows you to set biome variables in your ModPlayer class based on tile counts. This hook honestly won't be that useful until ModWorld is added.

### public virtual void UpdateBiomeVisuals()

Allows you to create special visual effects in the area around the player. For example, the blood moon's red filter on the screen or the slime rain's falling slime in the background. You must create classes that override Terraria.Graphics.Shaders.ScreenShaderData or Terraria.Graphics.Effects.CustomSky, add them in your mod's Load hook, then call Player.ManageSpecialBiomeVisuals. See the ExampleMod if you do not have access to the source code.