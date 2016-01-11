# ModItem

This class serves as a place for you to place all your properties and hooks for each item. Create instances of ModItem (preferably overriding this class) to pass as parameters to Mod.AddItem.

## Properties

### public Item item

The item object that this ModItem controls.

### public Mod mod

The mod that added this ModItem.

## Fields

### public bool projOnSwing

Setting this to true makes it so that this weapon can shoot projectiles only at the beginning of its animation. Set this to true if you want a sword and its projectile creation to be in sync (for example, the Terra Blade). Defaults to false.

### public int bossBagNPC

The type of NPC that drops this boss bag. Used to determine how many coins this boss bag contains. Defaults to 0, which means this isn't a boss bag.

## Methods

### public void AddTooltip(string tooltip)

Adds a line of text to this item's first group of tooltips.

### public void AddTooltip2(string tooltip)

Adds a line of text to this item's second group of tooltips.

### public virtual bool Autoload(ref string name, ref string texture, IList\<EquipType\> equips)

Allows you to automatically load an item instead of using Mod.AddItem. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, texture is initialized to the namespace and overriding class name with periods replaced with slashes, and equip is initialized to an empty list. Use this method to either force or stop an autoload, change the default display name and texture path, and to allow for autoloading equipment textures.

### public virtual void AutoloadEquip(EquipType equip, ref string texture, ref string armTexture, ref string femaleTexture)

This method is called when Autoload adds an equipment type. This allows you to specify equipment texture paths that differ from the defaults. Texture will be initialized to the item texture followed by an underscore and equip.ToString(), armTexture will be initialized to the item texture followed by "_Arms", and femaleTexture will be initialized to the item texture followed by "_FemaleBody".

### public virtual void AutoloadFlame(ref string texture)

Allows you to specify a texture path to the flame texture for this item when it is autoloaded. The flame texture is used when the player is holding this item and its "flame" field is set to true. At the moment torches are the only use of flame textures. By default the parameter will be set to the item texture followed by "_Flame". If the texture does not exist, then this item will not be given a flame texture.

### public virtual DrawAnimation GetAnimation()

Override this method to create an animation for your item. In general you will return a new Terraria.DataStructures.DrawAnimationVertical(int frameDuration, int frameCount).

### public virtual void SetDefaults()

This is where you set all your item's properties, such as width, damage, shootSpeed, defense, etc. For those that are familiar with tAPI, this has the same function as .json files.

### public virtual bool CanUseItem(Player player)

Returns whether or not this item can be used. By default returns true.

### public virtual void UseStyle(Player player)

Allows you to modify the location and rotation of this item in its use animation.

### public virtual void HoldStyle(Player player)

Allows you to modify the location and rotation of this item when the player is holding it.

### public virtual void HoldItem(Player player)

Allows you to make things happen when the player is holding this item (for example, torches make light and water candles increase spawn rate).

### public virtual void GetWeaponDamage(Player player, ref int damage)

Allows you to temporarily modify this weapon's damage based on player buffs, etc. This is useful for creating new classes of damage, or for making subclasses of damage (for example, Shroomite armor set boosts).

### public virtual void GetWeaponKnockback(Player player, ref float knockback)

Allows you to temporarily modify this weapon's knockback based on player buffs, etc. This allows you to customize knockback beyond the Player class's limited fields.

### public virtual bool ConsumeAmmo(Player player)

Whether or not ammo will be consumed upon usage. Called both by the gun and by the ammo; if at least one returns false then the ammo will not be used. By default returns true.

### public virtual bool Shoot(Player player, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

This is called before the weapon creates a projectile. You can use it to create special effects, such as changing the speed, changing the initial position, and/or firing multiple projectiles. Return false to stop the game from shooting the default projectile (do this if you manually spawn your own projectile). Returns true by default.

### public virtual void UseItemHitbox(Player player, ref Rectangle hitbox, ref bool noHitbox)

Changes the hitbox of this melee weapon when it is used.

### public virtual void MeleeEffects(Player player, Rectangle hitbox)

Allows you to give this melee weapon special effects, such as creating light or dust.

### public virtual bool? CanHitNPC(Player player, NPC target)

Allows you to determine whether this melee weapon can hit the given NPC when swung. Return true to allow hitting the target, return false to block this weapon from hitting the target, and return null to use the vanilla code for whether the target can be hit. Returns null by default.

### public virtual void ModifyHitNPC(Player player, NPC target, ref int damage, ref float knockBack, ref bool crit)

Allows you to modify the damage, knockback, etc., that this melee weapon does to an NPC.

### public virtual void OnHitNPC(Player player, NPC target, int damage, float knockBack, bool crit)

Allows you to create special effects when this melee weapon hits an NPC (for example how the Pumpkin Sword creates pumpkin heads).

### public virtual bool CanHitPvp(Player player, Player target)

Allows you to determine whether this melee weapon can hit the given opponent player when swung. Return false to block this weapon from hitting the target. Returns true by default.

### public virtual void ModifyHitPvp(Player player, Player target, ref int damage, ref bool crit)

Allows you to modify the damage, etc., that this melee weapon does to a player.

### public virtual void OnHitPvp(Player player, Player target, int damage, bool crit)

Allows you to create special effects when this melee weapon hits a player.

### public virtual bool UseItem(Player player)

Allows you to make things happen when this item is used. Return true if using this item actually does stuff. Returns false by default.

### public virtual bool ConsumeItem(Player player)

If this item is consumable and this returns true, then this item will be consumed upon usage. Returns true by default.

### public virtual bool UseItemFrame(Player player)

Allows you to modify the player's animation when this item is being used. Return true if you modify the player's animation. Returns false by default.

### public virtual bool HoldItemFrame(Player player)

Allows you to modify the player's animation when the player is holding this item. Return true if you modify the player's animation. Returns false by default.

### public virtual bool AltFunctionUse(Player player)

Allows you to make this item usable by right-clicking. Returns false by default. When this item is used by right-clicking, player.altFunctionUse will be set to 2.

### public virtual void UpdateInventory(Player player)

Allows you to make things happen when this item is in the player's inventory (for example, how the cell phone makes information display).

### public virtual void UpdateEquip(Player player)

Allows you to give effects to this armor or accessory, such as increased damage.

### public virtual void UpdateAccessory(Player player)

Allows you to give effects to this accessory. Frankly, this is just a more limited version of UpdateEquip, but I included this hook to be more in-line with the mysterious source code.

### public virtual void UpdateVanity(Player player, EquipType type)

Allows you to create special effects (such as dust) when this item's equipment texture of the given equipment type is displayed on the player. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual bool IsArmorSet(Item head, Item body, Item legs)

Returns whether or not the head armor, body armor, and leg armor make up a set. If this returns true, then this item's UpdateArmorSet method will be called. Returns false by default.

### public virtual void UpdateArmorSet(Player player)

Allows you to give set bonuses to the armor set that this armor is in.

### public virtual bool IsVanitySet(int head, int body, int legs)

Returns whether or not the head armor, body armor, and leg armor textures make up a set. This hook is used for the PreUpdateVanitySet, UpdateVanitySet, and ArmorSetShadow hooks. By default, this will return the same value as the IsArmorSet hook (passing the equipment textures' associated items as parameters), so you will not have to use this hook unless you want vanity effects to be entirely separate from armor sets. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void PreUpdateVanitySet(Player player)

Allows you to create special effects (such as the necro armor's hurt noise) when the player wears this item's vanity set. This hook is called regardless of whether the player is frozen in any way. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void UpdateVanitySet(Player player)

Allows you to create special effects (such as dust) when the player wears this item's vanity set. This hook will only be called if the player is not frozen in any way. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void ArmorSetShadows(Player player, ref bool longTrail, ref bool smallPulse, ref bool largePulse, ref bool shortTrail)

Allows you to determine special visual effects this vanity set has on the player without having to code them yourself. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void SetMatch(ref int equipSlot, ref bool robes)

Allows you to modify the equipment that the player appears to be wearing. This hook will only be called for body armor and leg armor. Note that equipSlot is *not* the same as the item type of the armor the player will appear to be wearing. Worn equipment has a separate set of IDs. You can find the vanilla equipment IDs by looking at the headSlot, bodySlot, and legSlot fields for items, and modded equipment IDs by looking at EquipLoader.   
If this hook is called on body armor, equipSlot allows you to modify the leg armor the player appears to be wearing. If you modify it, make sure to set robes to true. If this hook is called on leg armor, equipSlot allows you to modify the leg armor the player appears to be wearing, and the robes parameter is useless.

### public virtual bool CanRightClick()

Returns whether or not this item does something when it is right-clicked in the inventory. Returns false by default.

### public virtual void RightClick(Player player)

Allows you to make things happen when this item is right-clicked in the inventory. Useful for goodie bags.

### public virtual void OpenBossBag(Player player)

Allows you to give items to the given player when this item is right-clicked in the inventory if the bossBagNPC field has been set to a positive number. This ignores the CanRightClick and RightClick hooks.

### public virtual void DrawHands(ref bool drawHands, ref bool drawArms)

Allows you to determine whether the skin/shirt on the player's arms and hands are drawn when this body armor is worn. By default both flags will be false. Note that if drawHands is false, the arms will not be drawn either. Also note that this hook is only ever called through this item's associated equipment texture.

### public virtual void DrawHair(ref bool drawHair, ref bool drawAltHair)

Allows you to determine whether the player's hair or alt (hat) hair draws when this head armor is worn. By default both flags will be false. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual bool DrawHead()

Return false to hide the player's head when this head armor is worn. Returns true by default. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual bool DrawBody()

Return false to hide the player's body when this body armor is worn. Returns true by default. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual bool DrawLegs()

Return false to hide the player's legs when this leg armor or shoe accessory is worn. Returns true by default. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void DrawArmorColor(Player drawPlayer, float shadow, ref Color color, ref int glowMask, ref Color glowMaskColor)

Allows you to modify the colors in which this armor and surrounding accessories are drawn, in addition to which glow mask and in what color is drawn. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void ArmorArmGlowMask(Player drawPlayer, float shadow, ref int glowMask, ref Color color)

Allows you to modify which glow mask and in what color is drawn on the player's arms. Note that this is only called for body armor. Also note that this hook is only ever called through this item's associated equipment texture.

### public virtual void VerticalWingSpeeds(ref float ascentWhenFalling, ref float ascentWhenRising, ref float maxCanAscendMultiplier, ref float maxAscentMultiplier, ref float constantAscend)

Allows you to modify the speeds at which you rise and fall when these wings are equipped.

### public virtual void HorizontalWingSpeeds(ref float speed, ref float acceleration)

Allows you to modify these wing's horizontal flight speed and acceleration.

### public virtual void WingUpdate(Player player, bool inUse)

Allows for Wings to do various things while in use. "inUse" is whether or not the jump button is currently pressed. Called when these wings visually appear on the player. Use to animate wings, create dusts, invoke sounds, and create lights. Note that this hook is only ever called through this item's associated equipment texture.

### public virtual void Update(ref float gravity, ref float maxFallSpeed)

Allows you to customize this item's movement when lying in the world. Note that this will not be called if this item is currently being grabbed by a player.

### public virtual void PostUpdate()

Allows you to make things happen when this item is lying in the world. This will always be called, even when it is being grabbed by a player. This hook should be used for adding light, or for increasing the age of less valuable items.

### public virtual void GrabRange(Player player, ref int grabRange)

Allows you to modify how close this item must be to the player in order to move towards the player.

### public virtual bool GrabStyle(Player player)

Allows you to modify the way this item moves towards the player. Return true if you override this hook; returning false will allow the vanilla grab style to take place. Returns false by default.

### public virtual bool OnPickup(Player player)

Allows you to make special things happen when the player picks up this item. Return false to stop the item from being added to the player's inventory; returns true by default.

### public virtual Color? GetAlpha(Color lightColor)

Allows you to determine the color and transparency in which this item is drawn. Return null to use the default color (normally light color). Returns null by default.

### public virtual bool PreDrawInWorld(SpriteBatch spriteBatch, Color lightColor, Color alphaColor, ref float rotation, ref float scale)

Allows you to draw things behind this item, or to modify the way this item is drawn in the world. Return false to stop the game from drawing the item (useful if you're manually drawing the item). Returns true by default.

### public virtual void PostDrawInWorld(SpriteBatch spriteBatch, Color lightColor, Color alphaColor, float rotation, float scale)

Allows you to draw things in front of this item. This method is called even if PreDrawInWorld returns false.

### public virtual bool PreDrawInInventory(SpriteBatch spriteBatch, Vector2 position, Rectangle frame, Color drawColor, Color itemColor, Vector2 origin, float scale)

Allows you to draw things behind this item in the inventory. Return false to stop the game from drawing the item (useful if you're manually drawing the item). Returns true by default.

### public virtual bool PostDrawInInventory(SpriteBatch spriteBatch, Vector2 position, Rectangle frame, Color drawColor, Color itemColor, Vector2 origin, float scale)

Allows you to draw things in front of this item in the inventory. This method is called even if PreDrawInInventory returns false.

### public virtual Vector2? HoldoutOffset()

Allows you to determine the offset of this item's sprite when used by the player. This is only used for items with a useStyle of 5 that aren't staves. Return null to use the vanilla holdout offset; returns null by default.

### public virtual Vector2? HoldoutOrigin()

Allows you to determine the point on this item's sprite that the player holds onto when using this item. The origin is from the bottom left corner of the sprite. This is only used for staves with a useStyle of 5. Return null to use the vanilla holdout origin (zero); returns null by default.

### public virtual bool CanEquipAccessory(Player player, int slot)

Allows you to disallow the player from equipping this accessory. Return false to disallow equipping this accessory. Returns true by default.

### public virtual void ExtractinatorUse(ref int resultType, ref int resultStack)

Allows you to modify what item, and in what quantity, is obtained when this item is fed into the Extractinator. By default the parameters will be set to the output of feeding Silt/Slush into the Extractinator.

### public virtual void AutoLightSelect(ref bool dryTorch, ref bool wetTorch, ref bool glowstick)

Allows you to tell the game whether this item is a torch that cannot be placed in liquid, a torch that can be placed in liquid, or a glowstick. This information is used for when the player is holding down the auto-select hotkey.

### public virtual void CaughtFishStack(ref int stack)

Allows you to determine how many of this item a player obtains when the player fishes this item.

### public virtual bool IsQuestFish()

Whether or not the Angler can ever randomly request this type of item for his daily quest. Returns false by default.

### public virtual bool IsAnglerQuestAvailable()

Whether or not specific conditions have been satisfied for the Angler to be able to request this item. (For example, Hardmode.) Returns true by default.

### public virtual void AnglerQuestChat(ref string description, ref string catchLocation)

Allows you to set what the Angler says when he requests for this item. The description parameter is his dialogue, and catchLocation should be set to "\n(Caught at [location])".

### public virtual ModItem Clone()

Returns a clone of this ModItem. Allows you to decide which fields of your ModItem class are copied over when an item stack is split or something similar happens. By default all fields that you make will be automatically copied for you.

### public virtual void SaveCustomData(BinaryWriter writer)

Allows you to save custom data for this item. You are only able to save up to 64 KB of information per item (I don't imagine anyone will ever need more than that).

### public virtual void LoadCustomData(BinaryReader reader)

Allows you to load custom data that you have saved for this item.

### public virtual void AddRecipes()

This is essentially the same as Mod.AddRecipes. Do note that this will be called for every instance of the overriding ModItem class that is added to the game. This allows you to avoid clutter in your overriding Mod class by adding recipes for which this item is the result.