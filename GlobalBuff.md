# GlobalBuff

This class allows you to modify the behavior of any buff in the game.

## Properties

### public Mod mod

The mod to which this GlobalBuff belongs.

### public string Name

The internal name of this GlobalBuff instance.

## Methods

Note: There is no SetDefaults hook, since the same effects can be achieved through your Mod class's Load hook.

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalBuff instead of using Mod.AddGlobalBuff. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void Update(int type, Player player, ref int buffIndex)

Allows you to make the buff with the given ID give certain effects to a player. If you remove the buff from the player, make sure the decrement the buffIndex parameter by 1.

### public virtual void Update(int type, NPC npc, ref int buffIndex)

Allows you to make the buff with the given ID give certain effects to an NPC. If you remove the buff from the NPC, make sure to decrement the buffIndex parameter by 1.

### public virtual bool ReApply(int type, Player player, int time, int buffIndex)

Allows to you make special things happen when adding the given type of buff to a player when the player already has that buff. Return true to block the vanilla re-apply code from being called; returns false by default. The vanilla re-apply code sets the buff time to the "time" argument if that argument is larger than the current buff time. (For Mana Sickness, the vanilla re-apply code adds the "time" argument to the current buff time.)

### public virtual bool ReApply(int type, NPC npc, int time, int buffIndex)

Allows to you make special things happen when adding the given buff type to an NPC when the NPC already has that buff. Return true to block the vanilla re-apply code from being called; returns false by default. The vanilla re-apply code sets the buff time to the "time" argument if that argument is larger than the current buff time.

### public virtual void ModifyBuffTip(int type, ref string tip, ref int rare)

Allows you to modify the tooltip that displays when the mouse hovers over the buff icon, as well as the color the buff's name is drawn in.