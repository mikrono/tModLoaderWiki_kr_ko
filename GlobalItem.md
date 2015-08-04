# GlobalItem

This class allows you to modify and use hooks for all items, including vanilla items. Create an instance of an overriding class then call Mod.SetGlobalItem to use this.

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

### public virtual bool ConsumeAmmo(Item item, Player player)

Whether or not ammo will be consumed upon usage. Called both by the gun and by the ammo; if at least one returns false then the ammo will not be used. By default returns true.

### public virtual bool Shoot(Item item, Player player, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

This is called before the weapon creates a projectile. You can use it to create special effects, such as changing the speed, changing the initial position, and/or firing multiple projectiles. Return false to stop the game from shooting the default projectile (do this if you manually spawn your own projectile). Returns true by default.

### public virtual void UseItemHitbox(Item item, Player player, ref Rectangle hitbox, ref bool noHitbox)

Changes the hitbox of a melee weapon when it is used.

### public virtual void MeleeEffects(Item item, Player player, Rectangle hitbox)

Allows you to give melee weapons special effects, such as creating light or dust.

### public virtual void ModifyHitNPC(Item item, Player player, NPC target, ref int damage, ref float knockBack, ref bool crit)

Allows you to modify the damage, knockbac, etc., that a melee weapon does to an NPC.

### public virtual void OnHitNPC(Item item, Player player, NPC target, int damage, float knockBack, bool crit)

Allows you to create special effects when a melee weapon hits an NPC (for example how the Pumpkin Sword creates pumpkin heads).

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

Allows you to give set bonuses to your armor set with the given name. The name will be the same as returned by IsArmorSet.

### public virtual bool CanRightClick(Item item)

Returns whether or not an item does something when right-clicked in the inventory. Returns false by default.

### public virtual void RightClick(Item item, Player player)

Allows you to make things happen when an item is right-clicked in the inventory. Useful for goodie bags.

### public virtual void DrawHair(Item item, ref bool drawHair, ref bool drawAltHair)

Allows you to determine whether the player's hair or alt (hat) hair will be drawn when a head armor is worn.

### public virtual bool DrawHead(Item item)

Return false to hide the player's head when a head armor is worn. Returns true by default.

### public virtual void VerticalWingSpeeds(Item item, ref float ascentWhenFalling, ref float ascentWhenRising, ref float maxCanAscendMultiplier, ref float maxAscentMultiplier, ref float constantAscend)

Allows you to modify the speeds at which you rise and fall when wings are equipped.

### public virtual void HorizontalWingSpeeds(Item item, ref float speed, ref float acceleration)

Allows you to modify the horizontal flight speed and acceleration of wings.

### public virtual void Update(Item item, ref float gravity, ref float maxFallSpeed)

Allows you to customize an item's movement when lying in the world.

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

### public virtual bool CanEquipAccessory(Item item, Player player, int slot)

Allows you to disallow the player from equipping an accessory. Return false to disallow equipping the accessory. Returns true by default.