This is a quick list of the most common vanilla fields and a small description to what it does when changed.

Index|
-----|
[Item Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#item-fields)|
[Main Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#main-fields)|
[NPC Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#npc-fields)|
[Player Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#player-fields)|
[Projectile Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#projectile-fields)|
[WorldGen Fields](https://github.com/blushiemagic/tModLoader/wiki/Useful-Vanilla-Fields#worldgen-fields)|

# Item Fields

See [Item Class Documentation](https://github.com/blushiemagic/tModLoader/wiki/Item-Class-Documentation)

# Main Fields

* expertMode; bool

If true, means that the world is in expert mode.

* hardMode; bool

If true, the world is currently in hardmode.

* rain; bool

If true, is is currently raining in the world.

# NPC Fields

* downedBoss1; bool

If true, the Eye of Cthulhu has been killed in that world.

* downedBoss2; bool

If true, the Brain of Cthulhu/Eater of Worlds has been killed in that world.

* downedBoss3; bool

If true, Skeletron has been killed in that world.

* downedQueenBee; bool

If true, Queen Bee has been killed in that world.

* downedSlimeKing; bool

If true, Slime King has been killed in that world.

* downedMechBoss1; bool

The Destroyer

* downedMechBoss2; bool

The Twins

* downedMechBoss3; bool

Skeletron Prime

* downedMechBossAny; bool

If true, at least one mech boss has been killed in that world.

* downedPlantBoss; bool

If true, Plantera has been defeated in that world.

* downedGolemBoss; bool

If true, the Golem has been defeated in that world.

* downedAncientCultist; bool

If true, the Lunatic Cultist has been defeated in that world.

* downedMoonlord; bool

If true, the Moon Lord has been defeated in that world.

* downedGoblins; bool
* downedFrost; bool
* downedPirates; bool
* downedMartians; bool

If various events have been completed.

* savedTaxCollector; bool
* savedGoblin; bool
* savedWizard; bool
* savedMech; bool
* savedAngler; bool
* savedStylist; bool
* savedBartender; bool

If the named NPC has been rescued in this world yet.

# Player Fields

* dash; int

Enables a dash ability depending on the value of the variable. 2 = Eye of Cthulhu Shield Dash. 3 = Solar Armor Dash. 4 = Ninja Master Gear Dash.

* endurance; float (%)

Damage reduction modifier. 0 means no reduction, 1 is max reduction (minimum of 1 damage taken).

* immune; bool

Gives the player immunity. He cannot be hurt by being hit.

* immuneAlpha; int

Set to 0 to prevent blinking when immune.

* lifeRegen; int

Increases the player's life regen. The number is applied over 2 seconds.

* lifeRegenTime; int

Increases the player's life regen, but in a different way.

* magicCrit; int

The player's magic critical chance.

* magicDamage; float (%)

The player's magic damage.

* manaCost; float (%)

Lower the value to lower the mana cost of spells. You can augment it too to make them cost more mana.

* manaRegen; int

Increases the amount of mana the player regenerates. The amount is added over 2 seconds.

* manaRegenBonus; int

Increases the speed at which the player regenerates mana.

* maxMinions; int

The maximum allowed number of minions, default value is 1.

* maxRunSpeed; float

The maximum movement speed that the player can accelerate to.

* meleeCrit; int

The player's melee critical chance.

* meleeDamage; float (%)

The player's melee damage.

* meleeSpeed; float (%)

The player's melee swinging speed.

* minionDamage; float (%)

The player's minions damage.

* minionKB; float (%)

The player's minions knockback strength.

* moveSpeed; float

The player's movement speed. Caps at 1.6.

* noKnockback; bool

If true, the player isn't affected by knockback.

* noFallDmg; bool

If true, the player isn't affected by fall damage.

* numMinions; int

The current number of minions. Set via projectiles flagged as minion.

* rangedCrit; int

The player's ranged critical chance.

* rangedDamage; float (%)

The player's ranged damage.

* runAcceleration; float

The speed at which the player achieves their maximum movement speed.

* runSlowdown; float

The speed at which the player slowdowns when they stop moving.

* slotsMinions; float

The current filled slots of minions. Set via projectiles flagged as minion. Some minion projectiles only count as half a minion slot (think Retanimini and Spazmamini).

* statDefense; int

The player's defense.

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

* thrownCrit; int

The player's throwing critical chance.

* thrownDamage; float (%)

The player's throwing damage.

* wings; int

Set to the wingSlot value of the currently equipped wing item. Mainly used for visual wing feedback (dust spawning, etc).

* wingsLogic; int

Set to the wingSlot value of the currently equipped wing item. Used to determine wingTimeMax and additional wing mechanics.

* wingTime; int

The current wing time. Is substracted from when the player uses their wings. Defaults to wingTimeMax when grounded or grappled.

* wingTimeMax; int

Determines the player's max flight time with wings when not mounted. Defaults to 0.

# Projectile Fields

* ignoreWater; bool

The projectile will not be affected by water.

# Worldgen Fields

* crimson; bool

If true, crimson was generated for the world. Check if false for corruption instead.