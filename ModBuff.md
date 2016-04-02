# ModBuff

This class serves as a place for you to define a new buff and how that buff behaves. 

## Properties

### public Mod mod

The mod that added this ModBuff.

### public string Name

The internal name of this type of buff.

### public Texture2D Texture

The sprite sheet that this buff uses to display the buff icon while in use.

## Fields

### public bool longerExpertDebuff

If this buff is a debuff, setting this to true will make this buff last twice as long on players in expert mode. Defaults to false.

### public bool canBeCleared

Whether or not it is always safe to call Player.DelBuff on this buff. Setting this to false will prevent the nurse from being able to remove this debuff. Defaults to true.

## Methods

### public virtual bool Autoload(ref string name, ref string texture)

Allows you to automatically load a buff instead of using Mod.AddBuff. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this method to either force or stop an autoload, and to change the default display name and texture path.

### public virtual void SetDefaults()

This is where all buff related assignments go. Examples: 
* Main.buffName[Type] = "Display Name";
* Main.buffTip[Type] = "Buff Tooltip";
* Main.debuff[Type] = true;
* Main.buffNoTimeDisplay[Type] = true;
* Main.vanityPet[Type] = true;
* Main.lightPet[Type] = true;


### public virtual void Update(Player player, ref int buffIndex)

Allows you to make this buff give certain effects to the given player. If you remove the buff from the player, make sure the decrement the buffIndex parameter by 1.

### public virtual void Update(NPC npc, ref int buffIndex)

Allows you to make this buff give certain effects to the given NPC. If you remove the buff from the NPC, make sure to decrement the buffIndex parameter by 1.

### public virtual bool ReApply(Player player, int time, int buffIndex)

Allows to you make special things happen when adding this buff to a player when the player already has this buff. Return true to block the vanilla re-apply code from being called; returns false by default. The vanilla re-apply code sets the buff time to the "time" argument if that argument is larger than the current buff time.

### public virtual bool ReApply(NPC npc, int time, int buffIndex)

Allows to you make special things happen when adding this buff to an NPC when the NPC already has this buff. Return true to block the vanilla re-apply code from being called; returns false by default. The vanilla re-apply code sets the buff time to the "time" argument if that argument is larger than the current buff time.

### public virtual void ModifyBuffTip(ref string tip, ref int rare)

Allows you to modify the tooltip that displays when the mouse hovers over the buff icon, as well as the color the buff's name is drawn in.