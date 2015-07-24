# ModItem

This class serves as a place for you to place all your properties and hooks for each item. Create instances of ModItem (preferably overriding this class) to pass as parameters to Mod.AddItem.

## Properties

### public Item item

The item object that this ModItem controls.

### public Mod mod

The mod that added this ModItem.

## Constructors

### public ModItem()

Creates a new ModItem instance. You should only use this to add items to your mod.

## Methods

### public virtual bool Autoload(ref string name, ref string texture, ref EquipType? equip)

Allows you to automatically load an item instead of using Mod.AddItem. Return true to allow autoloading; by default returns the mod's autoload property. Name is initialized to the overriding class name, texture is initialized to the namespace and overriding class name with periods replaced with slashes, and equip is initialized to null. Use this method to either force or stop an autoload, change the default display name and texture path, and to allow for autoloading equipment textures.

### public virtual void AutoloadEquip(ref string texture, ref string armTexture, ref string femaleTexture)
This method is called when Autoload sets an equipment type. This allows you to specify equipment texture paths that differ from the defaults. Texture will be initialized to the item texture followed by an underscore and the EquipType, armTexture will be initialized to the item texture followed by "_Arms", and femaleTexture will be initialized to the item texture followed by "_FemaleBody".

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

### public virtual bool ConsumeAmmo(Player player)

Whether or not ammo will be consumed upon usage. Called both by the gun and by the ammo; if at least one returns false then the ammo will not be used. By default returns true.

### public virtual bool Shoot(Player player, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

This is called before the weapon creates a projectile. You can use it to create special effects, such as changing the speed, changing the initial position, and/or firing multiple projectiles. Return false to stop the game from shooting the default projectile (do this if you manually spawn your own projectile). Returns true by default.

### public virtual void UseItemHitbox(Player player, ref Rectangle hitbox, ref bool noHitbox)

Changes the hitbox of this melee weapon when it is used.

### public virtual void MeleeEffects(Player player, Rectangle hitbox)

Allows you to give this melee weapon special effects, such as creating light or dust.

### public virtual void ModifyHitNPC(Player player, NPC target, ref int damage, ref float knockBack, ref bool crit)

Allows you to modify the damage, knockback, etc., that this melee weapon does to an NPC.

### public virtual void OnHitNPC(Player player, NPC target, int damage, float knockBack, bool crit)

Allows you to create special effects when this melee weapon hits an NPC (for example how the Pumpkin Sword creates pumpkin heads).

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

### public virtual void UpdateInventory(Player player)

Allows you to make things happen when this item is in the player's inventory (for example, how the cell phone makes information display).

### public virtual void UpdateEquip(Player player)

Allows you to give effects to this armor or accessory, such as increased damage.

### public virtual void UpdateAccessory(Player player)

Allows you to give effects to this accessory. Frankly, this is just a more limited version of UpdateEquip, but I included this hook to be more in-line with the mysterious source code.

### public virtual bool IsArmorSet(Item head, Item body, Item legs)

Returns whether or not the head armor, body armor, and leg armor make up a set. If this returns true, then this item's UpdateArmorSet method will be called. Returns false by default.

### public virtual void UpdateArmorSet(Player player)

Allows you to give set bonuses to the armor set that this armor is in.

### public virtual bool CanRightClick()

Returns whether or not this item does something when it is right-clicked in the inventory. Returns false by default.

### public virtual void RightClick(Player player)

Allows you to make things happen when this item is right-clicked in the inventory. Useful for goodie bags.

### public virtual void DrawHair(ref bool drawHair, ref bool drawAltHair)

Allows you to determine whether the player's hair or alt (hat) hair draws when this head armor is worn. By default both flags will be false.

### public virtual bool DrawHead()

Return false to hide the player's head when this head armor is worn. Returns true by default.

### public virtual void VerticalWingSpeeds(ref float ascentWhenFalling, ref float ascentWhenRising, ref float maxCanAscendMultiplier, ref float maxAscentMultiplier, ref float constantAscend)

Allows you to modify the speeds at which you rise and fall when these wings are equipped.

### public virtual void HorizontalWingSpeeds(ref float speed, ref float acceleration)

Allows you to modify these wing's horizontal flight speed and acceleration.

### public virtual void Update(ref float gravity, ref float maxFallSpeed)

Allows you to customize this item's movement when lying in the world.

### public virtual Color? GetAlpha(Color lightColor)

Allows you to determine the color and transparency in which this item is drawn. Return null to use the default color (normally light color). Returns null by default.

### public virtual bool PreDrawInWorld(SpriteBatch spriteBatch, Color lightColor, Color alphaColor, ref float rotation, ref float scale)

Allows you to draw things behind this item, or to modify the way this item is drawn in the world. Return false to stop the game from drawing the item (useful if you're manually drawing the item). Returns true by default.

### public virtual void PostDrawInWorld(SpriteBatch spriteBatch, Color lightColor, Color alphaColor, float rotation, float scale)

Allows you to draw things in front of this item. This method is called even if PreDrawInWorld returns false.