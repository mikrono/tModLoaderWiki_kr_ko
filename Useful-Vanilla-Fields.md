This is a quick list of the most common vanilla fields and a small description to what it does when changed.

Index|
-----|
[Item Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#item-fields)|
[Main Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#main-fields)|
[NPC Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#npc-fields)|
[Player Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#player-fields)|
[Projectile Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#projectile-fields)|

# Item Fields

# Main Fields

* expertMode; bool

If true, means that the world is in expert mode.

* hardMode; bool

If true, the world is currently in hardmode.

# NPC Fields

* downedBoss1; bool

If true, the Eye of Cthulhu has been killed in that world.

* downedBoss2; bool

If true, the Brain of Cthulhu/Eater of Worlds has been killed in that world.

* downedBoss3; bool

If true, Skeletron has been killed in that world.

* downedMechBossAny; bool

If true, at least one mech boss has been killed in that world.

* downedPlantBoss; bool

If true, Plantera has been defeated in that world.

* downedGolemBoss; bool

If true, the Golem has been defeated in that world.

# Player Fields

* dash; int

Enables a dash ability depending on the value of the variable. 2 = Eye of Cthulhu Shield Dash. 3 = Solar Armor Dash. 4 = Ninja Master Gear Dash.

* maxRunSpeed; float

The maximum movement speed that the player can accelerate to.

* moveSpeed; float

The player's movement speed. Caps at 1.6.

* runAcceleration; float

The speed at which the player achieves their maximum movement speed.

* runSlowdown; float

The speed at which the player slowdowns when they stop moving.

* statLifeMax; int

The base amount of max health. Do not change it unless you know what you're doing, Use statLifeMax2 instead.
For example, this is the variable that Life Hearts increase. It is permanent and is saved automatically.

* statLifeMax2; int

The temporary max health of the player. 
For example, this is used for things like accessories or potions. It is temporary and resets upon world load.

* statManaMax; int

Similar to statLifeMax, but modifies mana instead.

* statManaMax2; int

Similar to statLifeMax2, but modifies mana instead.

* endurance; float

Damage reduction modifier. 0 means no reduction, 1 is max reduction (minimum of 1 damage taken).

* maxMinions; int

The maximum allowed number of minions, default value is 1.

* numMinions; int

The current number of minions. Set via projectiles flagged as minion.

* slotsMinions; float

The current filled slots of minions. Set via projectiles flagged as minion. Some minion projectiles only count as half a minion slot (think Retanimini and Spazmamini).

* wings; int

Set to the wingSlot value of the currently equipped wing item. Mainly used for visual wing feedback (dust spawning, etc).

* wingsLogic; int

Set to the wingSlot value of the currently equipped wing item. Used to determine wingTimeMax and additional wing mechanics.

* wingTime; int

The current wing time. Is substracted from when the player uses their wings. Defaults to wingTimeMax when grounded or grappled.

* wingTimeMax; int

Determines the player's max flight time with wings when not mounted. Defaults to 0.

# Projectile Fields