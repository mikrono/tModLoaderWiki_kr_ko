This is a quick list of the most common vanilla fields and a small description to what it does when changed.

# Item Fields

# NPC Fields

# Player Fields
* int statLifeMax;

The base amount of max health. Do not change it unless you know what you're doing, Use statLifeMax2 instead.
For example, this is the variable that Life Hearts increase. It is permanent and is saved automatically.

* int statLifeMax2;

The temporary max health of the player. 
For example, this is used for things like accessories or potions. It is temporary and resets upon world load.
* 
* 

# Projectile Fields