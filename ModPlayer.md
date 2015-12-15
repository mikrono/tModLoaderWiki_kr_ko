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

### public virtual void UpdateBadLifeRegen()

Allows you to give the player a negative life regeneration based on its state (for example, the "On Fire!" debuff makes the player take damage-over-time). This is typically done by setting player.lifeRegen to 0 if it is positive, setting player.lifeRegenTime to 0, and subtracting a number from player.lifeRegen. The player will take damage at a rate of half the number you subtract per second.

### public virtual void UpdateLifeRegen()

Allows you to increase the player's life regeneration based on its state. This can be done by incrementing player.lifeRegen by a certain number. The player will recover life at a rate of half the number you add per second. You can also increment player.lifeRegenTime to increase the speed at which the player reaches its maximum natural life regeneration.

### public virtual void NaturalLifeRegen(ref float regen)

Allows you to modify the power of the player's natural life regeneration. This can be done by multiplying the regen parameter by any number. For example, campfires multiply it by 1.1, while walking multiplies it by 0.5.

### public virtual void PreUpdate()

This is called at the beginning of every tick update for this player, after checking whether the player exists.

### public virtual void SetControls()

Use this to modify the control inputs that the player receives. For example, the Confused debuff swaps the values of player.controlLeft and player.controlRight. This is called sometime after PreUpdate is called.

### public virtual void PreUpdateBuffs()

This is called sometime after SetControls is called, and right before all the buffs update on this player. This hook can be used to add buffs to the player based on the player's state (for example, the Campfire buff is added if the player is near a Campfire).

### public virtual void PostUpdateBuffs()

This is called right after all of this player's buffs update on the player. This can be used to modify the effects that the buff updates had on this player, and can also be used for general update tasks.

### public virtual void PostUpdateEquips()

This is called right after all of this player's equipment and armor sets update on the player, which is sometime after PostUpdateBuffs is called. This can be used to modify the effects that the equipment had on this player, and can also be used for general update tasks.

### public virtual void PostUpdateMiscEffects()

This is called after miscellaneous update code is called in Player.Update, which is sometime after PostUpdateEquips is called. This can be used for general update tasks.

### public virtual void PostUpdateRunSpeeds()

This is called after the player's horizontal speeds are modified, which is sometime after PostUpdateMiscEffects is called, and right before the player's horizontal position is updated. Use this to modify maxRunSpeed, accRunSpeed, runAcceleration, and similar variables before the player moves forwards/backwards.

### public virtual void PostUpdate()

This is called at the very end of the Player.Update method. Final general update tasks can be placed here.

### public virtual void FrameEffects()

Allows you to modify the armor and accessories that visually appear on the player. In addition, you can create special effects around this character, such as creating dust.

### public virtual bool PreHurt(bool pvp, bool quiet, ref int damage, ref int hitDirection, ref bool crit, ref bool customDamage, ref bool playSound, ref bool genGore, ref string deathText)

This hook is called before every time the player takes damage. The pvp parameter is whether the damage was from another player. The quiet parameter determines whether the damage will be communicated to the server. The damage, hitDirection, and crit parameters can be modified. Set the customDamage parameter to true if you want to use your own damage formula (this parameter will disable automatically subtracting the player's defense from the damage). Set the playSound parameter to false to disable the player's hurt sound, and the genGore parameter to false to disable the dust particles that spawn. (These are useful for creating your own sound or gore.) The deathText parameter can be modified to change the player's death message if the player dies. Return false to stop the player from taking damage. Returns true by default.

### public virtual void Hurt(bool pvp, bool quiet, double damage, int hitDirection, bool crit)

Allows you to make anything happen right before damage is subtracted from the player's health.

### public virtual void PostHurt(bool pvp, bool quiet, double damage, int hitDirection, bool crit)

Allows you to make anything happen when the player takes damage.

### public virtual bool PreKill(double damage, int hitDirection, bool pvp, ref bool playSound, ref bool genGore, ref string deathText)

This hook is called whenever the player is about to be killed after reaching 0 health. Set the playSound parameter to false to stop the death sound from playing. Set the genGore parameter to false to stop the gore and dust from being created. (These are useful for creating your own sound or gore.) Return false to stop the player from being killed. Only return false if you know what you are doing! Returns true by default.

### public virtual void Kill(double damage, int hitDirection, bool pvp, string deathText)

Allows you to make anything happen when the player dies.

### public virtual bool PreItemCheck()

Allows you to do anything before the update code for the player's held item is run. Return false to stop the held item update code from being run (for example, if the player is frozen). Returns true by default.

### public virtual void PostItemCheck()

Allows you to do anything after the update code for the player's held item is run. Hooks for the middle of the held item update code have more specific names in ModItem and ModPlayer.

### public virtual void GetWeaponDamage(Item item, ref int damage)

Allows you to temporarily modify a weapon's damage based on player buffs, etc. This is useful for creating new classes of damage, or for making subclasses of damage (for example, Shroomite armor set boosts).

### public virtual void GetWeaponKnockback(Item item, ref float knockback)

Allows you to temporarily modify a weapon's knockback based on player buffs, etc. This allows you to customize knockback beyond the Player class's limited fields.

### public virtual bool ConsumeAmmo(Item weapon, Item ammo)

Whether or not ammo will be consumed upon usage. Return false to stop the ammo from being depleted. Returns true by default.

### public virtual bool Shoot(Item item, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

This is called before this player's weapon creates a projectile. You can use it to create special effects, such as changing the speed, changing the initial position, and/or firing multiple projectiles. Return false to stop the game from shooting the default projectile (do this if you manually spawn your own projectile). Returns true by default.

### public virtual void MeleeEffects(Item item, Rectangle hitbox)

Allows you to give this player's melee weapon special effects, such as creating light or dust.

### public virtual void OnHitAnything(float x, float y, Entity victim)

This hook is called when a player damages anything, whether it be an NPC or another player, using anything, whether it be a melee weapon or a projectile. The x and y parameters are the coordinates of the victim parameter's center.

### public virtual bool? CanHitNPC(Item item, NPC target)

Allows you to determine whether a player can hit the given NPC by swinging a melee weapon. Return true to allow hitting the target, return false to block this player from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(Item item, NPC target, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that this player does to an NPC by swinging a melee weapon.

### public virtual void OnHitNPC(Item item, NPC target, int damage, float knockback, bool crit)

Allows you to create special effects when this player hits an NPC by swinging a melee weapon (for example how the Pumpkin Sword creates pumpkin heads).

### public virtual bool? CanHitNPCWithProj(Projectile proj, NPC target)

Allows you to determine whether a projectile created by this player can hit the given NPC. Return true to allow hitting the target, return false to block this projectile from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPCWithProj(Projectile proj, NPC target, ref int damage, ref float knockback, ref bool crit)

Allows you to modify the damage, knockback, etc., that a projectile created by this player does to an NPC.

### public virtual void OnHitNPCWithProj(Projectile proj, NPC target, int damage, float knockback, bool crit)

Allows you to create special effects when a projectile created by this player hits an NPC (for example, inflicting debuffs).

### public virtual bool CanHitPvp(Item item, Player target)

Allows you to determine whether a melee weapon swung by this player can hit the given opponent player. Return false to block this weapon from hitting the target. Returns true by default.

### public virtual void ModifyHitPvp(Item item, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that a melee weapon swung by this player does to an opponent player.

### public virtual void OnHitPvp(Item item, Player target, int damage, bool crit)

Allows you to create special effects when this player's melee weapon hits an opponent player.

### public virtual bool CanHitPvpWithProj(Projectile proj, Player target)

Allows you to determine whether a projectile created by this player can hit the given opponent player. Return false to block the projectile from hitting the target. Returns true by default.

### public virtual void ModifyHitPvpWithProj(Projectile proj, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that a projectile created by this player does to an opponent player.

### public virtual void OnHitPvpWithProj(Projectile proj, Player target, int damage, bool crit)

Allows you to create special effects when a projectile created by this player hits an opponent player.

### public virtual void CatchFish(Item fishingRod, Item bait, int liquidType, int poolSize, int worldLayer, int questFish, ref int caughtType, ref bool junk)

Allows you to change the item the player gains from catching a fish. The fishingRod and bait parameters refer to the said items in the player's inventory. The liquidType parameter is 0 if the player is fishing in water, 1 for lava, and 2 for honey. The poolSize parameter is the tile size of the pool the player is fishing in. The worldLayer parameter is 0 if the player is in the sky, 1 if the player is on the surface, 2 if the player is underground, 3 if the player is in the caverns, and 4 if the player is in the underworld. The questFish parameter is the item ID for the day's Angler quest. Modify the caughtType parameter to change the item the player catches. The junk parameter is whether the player catches junk; you can set this to true if you make the player catch a junk item, and is mostly used to pass information (has no effect on the game).

### public virtual void GetFishingLevel(Item fishingRod, Item bait, ref int fishingLevel)

Allows you to modify the player's fishing power. As an example of the type of stuff that should go here, the phase of the moon can influence fishing power.

### public virtual void AnglerQuestReward(float rareMultiplier, List<Item> rewardItems)

Allows you to add to, change, or remove from the items the player earns when finishing an Angler quest. The rareMultiplier is a number between 0.15 and 1 inclusively; the lower it is the higher chance there should be for the player to earn rare items.

### public virtual void GetDyeTraderReward(List<int> rewardPool)

Allows you to modify what items are possible for the player to earn when giving a Strange Plant to the Dye Trader.