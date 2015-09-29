# ModBuff

This class serves as a place for you to define a new buff and how that buff behaves. 

## Properties

### public Mod mod

The mod that added this ModBuff.

### public string Name

The internal name of this type of buff.

### public Texture2D Texture

The sprite sheet that this buff uses to display the buff icon while in use.

## Methods

### public virtual bool Autoload(ref string name, ref string texture, IList\<EquipType\> equips)

Allows you to automatically load a buff instead of using Mod.AddBuff. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name and texture is initialized to the namespace and overriding class name with periods replaced with slashes. Use this method to either force or stop an autoload and change the default display name and texture path.

### public virtual void SetDefaults()

This is where all buff related assignments go. Examples: 
* Main.buffName[Type] = "Display Name";
* Main.buffTip[Type] = "Buff Tooltip";
* Main.debuff[Type] = true;
* Main.buffNoTimeDisplay[Type] = true;
* Main.vanityPet[Type] = true;
* Main.lightPet[Type] = true;


### public virtual void Update(Player player, ref int buffIndex)

Called while Buff is active. Used to maintain buff related effects and projectiles.
