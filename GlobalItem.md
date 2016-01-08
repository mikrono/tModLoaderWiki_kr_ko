# GlobalItem

This class allows you to modify and use hooks for all items, including vanilla items. Create an instance of an overriding class then call Mod.AddGlobalItem to use this.

## Properties

### public Mod mod

The mod to which this GlobalItem belongs.

### public string Name

The name of this GlobalItem instance.

## Methods

### public void AddTooltip(Item item, string tooltip)

Adds a line of text to an item's first group of tooltips.

### public void AddTooltip2(Item item, string tooltip)

Adds a line of text to an item's second group of tooltips.

### public virtual bool Autoload(ref string name)

Allows you to automatically load a GlobalItem instead of using Mod.AddGlobalItem. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name. Use this method to either force or stop an autoload or to control the internal name.

### public virtual void SetDefaults(Item item)

Allows you to set the properties of any and every item that gets created.

### public virtual bool CanUseItem(Item item, Player player)

Returns whether or not any item can be used. Returns true by default. The inability to use a specific item overrides this, so use this to stop an item from being used.

### public virtual void UseStyle(Item item, Player player)

Allows you to modify the location and rotation of any item in its use animation.

### public virtual void HoldStyle(Player player)

Allows you to modify the location and rotation of the item the player is currently holding.

### public virtual void HoldItem(Item item, Player player)

Allows you to make things happen when the player is holding an item (for example, torches make light and water candles increase spawn rate).

### public virtual void GetWeaponDamage(Item item, Player player, ref int damage)

Allows you to temporarily modify a weapon's damage based on player buffs, etc. This is useful for creating new classes of damage, or for making subclasses of damage (for example, Shroomite armor set boosts).

### public virtual void GetWeaponKnockback(Item item, Player player, ref float knockback)

Allows you to temporarily modify a weapon's knockback based on player buffs, etc. This allows you to customize knockback beyond the Player class's limited fields.

### public virtual bool ConsumeAmmo(Item item, Player player)

Whether or not ammo will be consumed upon usage. Called both by the gun and by the ammo; if at least one returns false then the ammo will not be used. By default returns true.

### public virtual bool Shoot(Item item, Player player, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

This is called before the weapon creates a projectile. You can use it to create special effects, such as changing the speed, changing the initial position, and/or firing multiple projectiles. Return false to stop the game from shooting the default projectile (do this if you manually spawn your own projectile). Returns true by default.

### public virtual void UseItemHitbox(Item item, Player player, ref Rectangle hitbox, ref bool noHitbox)

Changes the hitbox of a melee weapon when it is used.

### public virtual void MeleeEffects(Item item, Player player, Rectangle hitbox)

Allows you to give melee weapons special effects, such as creating light or dust.

### public virtual bool? CanHitNPC(Item item, Player player, NPC target)

Allows you to determine whether a melee weapon can hit the given NPC when swung. Return true to allow hitting the target, return false to block the weapon from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(Item item, Player player, NPC target, ref int damage, ref float knockBack, ref bool crit)

Allows you to modify the damage, knockbac, etc., that a melee weapon does to an NPC.

### public virtual void OnHitNPC(Item item, Player player, NPC target, int damage, float knockBack, bool crit)

Allows you to create special effects when a melee weapon hits an NPC (for example how the Pumpkin Sword creates pumpkin heads).

### public virtual bool CanHitPvp(Item item, Player player, Player target)

Allows you to determine whether a melee weapon can hit the given opponent player when swung. Return false to block the weapon from hitting the target. Returns true by default.

### public virtual void ModifyHitPvp(Item item, Player player, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that a melee weapon does to a player.

### public virtual void OnHitPvp(Item item, Player player, Player target, int damage, bool crit)

Allows you to create special effects when a melee weapon hits a player.

### public virtual bool UseItem(Item item, Player player)

Allows you to make things happen when an item is used. Return true if using the item actually does stuff. Returns false by default.

### public virtual bool ConsumeItem(Item item, Player player)

If the item is consumable and this returns true, then the item will be consumed upon usage. Returns true by default.

### public virtual bool UseItemFrame(Item item, Player player)

Allows you to modify the player's animation when an item is being used. Return true if you modify the player's animation. Returns false by default.

### public virtual bool HoldItemFrame(Item item Player player)

Allows you to modify the player's animation when the player is holding an item. Return true if you modify the player's animation. Returns false by default.

### public virtual void UpdateInventory(Item item, Player player)

Allows you to make things happen when an item is in the player's inventory (for example, how the cell phone makes information display).

### public virtual void UpdateEquip(Item item, Player player)

Allows you to give effects to armors and accessories, such as increased damage.

### public virtual void UpdateAccessory(Item item, Player player)

Allows you to give effects to accessories. Frankly, this is just a more limited version of UpdateEquip, but I included this hook to be more in-line with the mysterious source code.

### public virtual string IsArmorSet(Item head, Item body, Item legs)

Allows you to determine whether the player is wearing an armor set, and return a name for this set. If there is no armor set, return the empty string. Returns the empty string by default.

### public virtual void UpdateArmorSet(Player player, string set)

Allows you to give set bonuses to your armor set with the given name. The set name will be the same as returned by IsArmorSet.

### public virtual string IsVanitySet(int head, int body, int legs)

Returns whether or not the head armor, body armor, and leg armor textures make up a set. This hook is used for the PreUpdateVanitySet, UpdateVanitySet, and ArmorSetShadow hooks, and will use items in the social slots if they exist. By default this will return the same value as the IsArmorSet hook, so you will not have to use this hook unless you want vanity effects to be entirely separate from armor sets.

### public virtual void PreUpdateVanitySet(Player player, string set)

Allows you to create special effects (such as the necro armor's hurt noise) when the player wears the vanity set with the given name returned by IsVanitySet. This hook is called regardless of whether the player is frozen in any way.

### public virtual void UpdateVanitySet(Player player, string set)

Allows you to create special effects (such as dust) when the player wears the vanity set with the given name returned by IsVanitySet. This hook will only be called if the player is not frozen in any way.

### public virtual void ArmorSetShadows(Player player, string set, ref bool longTrail, ref bool smallPulse, ref bool largePulse, ref bool shortTrail)

Allows you to determine special visual effects a vanity has on the player without having to code them yourself.

### public virtual void SetMatch(int type, ref int equipSlot, ref bool robes)

Allows you to modify the equipment that the player appears to be wearing. This hook will only be called for body armor and leg armor. The type parameter is the equipment type of the item that the player is wearing. Note that type and equipSlot are *not* the same as the item type of the armor the player will appear to be wearing. Worn equipment has a separate set of IDs. You can find the vanilla equipment IDs by looking at the headSlot, bodySlot, and legSlot fields for items, and modded equipment IDs by looking at EquipLoader.   
If this hook is called on body armor, equipSlot allows you to modify the leg armor the player appears to be wearing. If you modify it, make sure to set robes to true. If this hook is called on leg armor, equipSlot allows you to modify the leg armor the player appears to be wearing, and the robes parameter is useless.

### public virtual bool CanRightClick(Item item)

Returns whether or not an item does something when right-clicked in the inventory. Returns false by default.

### public virtual void RightClick(Item item, Player player)

Allows you to make things happen when an item is right-clicked in the inventory. Useful for goodie bags.

### public virtual bool PreOpenVanillaBag(string context, Player player, int arg)

Allows you to make vanilla bags drop your own items and stop the default items from being dropped. Return false to stop the default items from being dropped; returns true by default. Context will either be "present", "bossBag", "crate", "lockBox", "herbBag", or "goodieBag". For boss bags and crates, arg will be set to the type of the item being opened.

### public virtual void OpenVanillaBag(string context, Player player, int arg)

Allows you to make vanilla bags drop your own items in addition to the default items. This method will not be called if any other GlobalItem returns false for PreOpenVanillaBag. Context will either be "present", "bossBag", "crate", "lockBox", "herbBag", or "goodieBag". For boss bags and crates, arg will be set to the type of the item being opened.

### public virtual void DrawHands(int body, ref bool drawHands, ref bool drawArms)

Allows you to determine whether the skin/shirt on the player's arms and hands are drawn when a body armor is worn. Note that if drawHands is false, the arms will not be drawn either.

### public virtual void DrawHair(int head, ref bool drawHair, ref bool drawAltHair)

Allows you to determine whether the player's hair or alt (hat) hair will be drawn when a head armor is worn.

### public virtual bool DrawHead(int head)

Return false to hide the player's head when a head armor is worn. Returns true by default.

### public virtual bool DrawBody(int body)

Return false to hide the player's body when a body armor is worn. Returns true by default.

### public virtual bool DrawLegs(int legs, int shoes)

Return false to hide the player's legs when a leg armor or shoe accessory is worn. Returns true by default.

### public virtual void DrawArmorColor(EquipType type, int slot, ref Color color, ref int glowMask, ref Color glowMaskColor, ref int armGlowMask, ref Color armGlowMaskColor)

Allows you to modify the colors in which the player's armor and their surrounding accessories are drawn, in addition to which glow masks and in what colors are drawn. The armGlowMask and armGlowMaskColor parameters will only do anything when the type parameter is EquipType.Body.

### public virtual void VerticalWingSpeeds(Item item, ref float ascentWhenFalling, ref float ascentWhenRising, ref float maxCanAscendMultiplier, ref float maxAscentMultiplier, ref float constantAscend)

Allows you to modify the speeds at which you rise and fall when wings are equipped.

### public virtual void HorizontalWingSpeeds(Item item, ref float speed, ref float acceleration)

Allows you to modify the horizontal flight speed and acceleration of wings.

### public virtual void WingUpdate(int wings, Player player, bool inUse)

Allows for Wings to do various things while in use. "inUse" is whether or not the jump button is currently pressed. Called when wings visually appear on the player. Use to animate wings, create dusts, invoke sounds, and create lights.

### public virtual void Update(Item item, ref float gravity, ref float maxFallSpeed)

Allows you to customize an item's movement when lying in the world. Note that this will not be called if the item is currently being grabbed by a player.

### public virtual void PostUpdate(Item item)

Allows you to make things happen when an item is lying in the world. This will always be called, even when the item is being grabbed by a player. This hook should be used for adding light, or for increasing the age of less valuable items.

### public virtual void GrabRange(Item item, Player player, ref int grabRange)

Allows you to modify how close an item must be to the player in order to move towards the player.

### public virtual bool GrabStyle(Item item, Player player)

Allows you to modify the way an item moves towards the player. Return false to allow the vanilla grab style to take place. Returns false by default.

### public virtual bool OnPickup(Item item, Player player)

Allows you to make special things happen when the player picks up an item. Return false to stop the item from being added to the player's inventory; returns true by default.

### public virtual Color? GetAlpha(Item item, Color lightColor)

Allows you to determine the color and transparency in which an item is drawn. Return null to use the default color (normally light color). Returns null by default.

### public virtual bool PreDrawInWorld(Item item, SpriteBatch spriteBatch, Color lightColor, Color alphaColor, ref float rotation, ref float scale)

Allows you to draw things behind an item, or to modify the way an item is drawn in the world. Return false to stop the game from drawing the item (useful if you're manually drawing the item). Returns true by default.

### public virtual void PostDrawInWorld(Item item, SpriteBatch spriteBatch, Color lightColor, Color alphaColor, float rotation, float scale)

Allows you to draw things in front of an item. This method is called even if PreDrawInWorld returns false.

### public virtual bool PreDrawInInventory(Item item, SpriteBatch spriteBatch, Vector2 position, Rectangle frame, Color drawColor, Color itemColor, Vector2 origin, float scale)

Allows you to draw things behind an item in the inventory. Return false to stop the game from drawing the item (useful if you're manually drawing the item). Returns true by default.

### public virtual bool PostDrawInInventory(Item item, SpriteBatch spriteBatch, Vector2 position, Rectangle frame, Color drawColor, Color itemColor, Vector2 origin, float scale)

Allows you to draw things in front of an item in the inventory. This method is called even if PreDrawInInventory returns false.

### public virtual Vector2? HoldoutOffset(int type)

Allows you to determine the offset of an item's sprite when used by the player. This is only used for items with a useStyle of 5 that aren't staves. Return null to use the item's default holdout offset; returns null by default.

### public virtual Vector2? HoldoutOrigin(int type)

Allows you to determine the point on an item's sprite that the player holds onto when using the item. The origin is from the bottom left corner of the sprite. This is only used for staves with a useStyle of 5. Return null to use the item's default holdout origin; returns null by default.

### public virtual bool CanEquipAccessory(Item item, Player player, int slot)

Allows you to disallow the player from equipping an accessory. Return false to disallow equipping the accessory. Returns true by default.

### public virtual void ExtractinatorUse(int extractType, ref int resultType, ref int resultStack)

Allows you to modify what item, and in what quantity, is obtained when an item of the given type is fed into the Extractinator. An extractType of 0 represents the default extraction (Silt and Slush). By default the parameters will be set to the output of feeding Silt/Slush into the Extractinator.

### public virtual void CaughtFishStack(int type, ref int stack)

Allows you to modify how many of an item a player obtains when the player fishes that item.

### public virtual bool IsAnglerQuestAvailable(int type)

Whether or not specific conditions have been satisfied for the Angler to be able to request the given item. (For example, Hardmode.) Returns true by default.

### public virtual void AnglerChat(bool turningInFish, bool anglerQuestFinished, int type, ref string chat, ref string catchLocation)

Allows you to set what the Angler says when the Quest button is clicked in his chat. The turningInFish parameter is whether the player is turning in the quest fish at that moment. The anglerQuestFinished parameter is whether the player has already turned in the quest fish earlier that day. The chat parameter is his dialogue, and catchLocation should be set to "\n(Caught at [location])" for the given type.