# Item Class Documentation
This page lists methods and fields pertaining to the Item class. This page is useful for understanding what to use in ModItem.SetDefaults.

Index|
-----|
[Fields](https://github.com/blushiemagic/tModLoader/wiki/Item-Class-Documentation#fields-and-properties)|
[Methods](https://github.com/blushiemagic/tModLoader/wiki/Item-Class-Documentation#methods)|

# Fields and Properties
You can assign these fields to give your ModItem various values. Typically you'll want to refer to this page when writing code for ModItem.SetDefaults. Be sure to visit [Vanilla Item Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Item-Field-Values) to see what values vanilla items use for these fields.

## value (int)
Value is the number of copper coins the item is worth (aka, cost to buy from a merchant). Setting it to 10462 would mean the item cost 1 gold, 4 silver, and 62 copper. The sell price of an item is one fifth of its value. Value also influences reforge costs with the goblin tinkerer. For convenience, you can also use the `Item.buyPrice()` method for setting values: `item.value = Item.buyPrice(0, 1, 4, 62);`
You can also use the `Item.sellPrice()` method if you would rather think about an items value the other way.  Both `Item.buyPrice(0, 0, 10, 55)` and `Item.sellPrice(0, 0, 2, 11)` would set the value to 1055.

## useStyle (int)
The use style of your item:   
1 for swinging,   
2 for drinking,   
3 act like shortsword,   
4 for use like life crystal,   
5 for use staffs or guns  

## useTurn (bool)
Whether the player can turn around while the using animation is happening.

## autoReuse (bool)
Whether the item is in continuous use while the mouse button is held down.

## holdStyle (int)
The hold style of your item:
1 for holding out (torches and glowsticks)
2 for holding up (Breathing Reed)
3 for a different version of holding out (Magical Harp)

## useAnimation (int)
The time span of the using animation for the weapon. Recommended to be the same at useTime as this is only the animation.

## useTime (int)
The time span of using the weapon in frames. Note: Terraria runs at 60 frames per second.

## reuseDelay (int)
A delay in frames added at the end of the using animation for the item, during which the player wont be able to use any items.

## consumable (bool)
Whether the item is consumed after use.

### rare (int)
Range from -1 to 13. 
Check wiki link for respective colors: https://terraria.gamepedia.com/Rarity. You can use ItemRarityID for convenience: [ItemRarityID.cs](https://github.com/blushiemagic/tModLoader/blob/master/patches/tModLoader/Terraria.ID/ItemRarityID.cs), [Sample Usage](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Items/Infinity.cs#L23)

### maxStack (int)
The maximum number of items that can be contained within a single stack.

### width (int)
The width of the item's hitbox in pixels.

### height (int)
The height of the item's hitbox in pixels.

### scale (float)
The size multiplier of the item's sprite while the item is being used. Also increases range for melee weapons.

### createTile (int)
The ID of the tile this item places on use.

### placeStyle (int)
The style of the tile being placed. Used for tiles that have a different look depending on the item used to place them.

### createWall (int)
The ID of the tile this item places on use.

### UseSound (LegacySoundStyle)
The sound that your item makes when used.
Ex: `item.UseSound = SoundID.Item1;`

### damage (int)
The base damage inflicted by the item.

### knockBack (int)
The force of the knock back. Max value is 20. 

### shoot (int)
The ID of the projectile that is fired by the item on use.

### shootSpeed (float)
The velocity in pixels the projectile fired by the item will have. Actual velocity depends on the projectile being fired.

### noMelee (bool)
If true, the item's using animation will not deal damage.

### accessory (bool)
Whether the item is an accessory.

### melee (bool)
### magic (bool)
### ranged (bool)
### thrown (bool)
### summon (bool)
The type of damage this item deals, if it is a weapon. Setting more than one is useless.

### defense (int)
The amount of defense this item provides when equipped, either as an accessory or armor.

### crit (int)
The base critical chance for this item.

### noUseGraphic (bool)
If true, the item's sprite will not be visible while the item is in use.

### useAmmo (int)
The ID of the ammo used by this item. See [Ammo Guide](https://github.com/blushiemagic/tModLoader/wiki/Basic-Ammo)

### ammo (int)
The ID of the ammo class this item is part of. See [Ammo Guide](https://github.com/blushiemagic/tModLoader/wiki/Basic-Ammo)

### notAmmo (bool)
If true and the item is ammo, the item will not count as ammo for certain ammo-specific behavior, such as the tooltip mentioning the item is ammo, or ammo items going into ammo slots first when picked up.

### mana (int)
The amount of mana this item consumes on use.

### channel (bool)
Used for items that have special behavior when the attack button is held.

### axe (int)
### pick (int)
### hammer (int)
These 3 correspond to the power, but axe power is multiplied by 5, so adjust accordingly.

### potion (bool)
If true, this item will inflict potion sickness on use. Also determines whether the item cannot be used when the player has potion sickness, and if the item can be used with the Quick Heal key.

### healLife (int)
The amount of life this item heals on use.

### healMana (int)
The amount of mana this item heals on use.

### buffType (int)
The ID of the buff given by this item on use.

### buffTime (int)
The duration in ticks of the buff given by this item on use.

### expert (bool)
Signifies that this item is an expert mode item.

### bait (int)
### fishingPole (int)
Correspond to fishing power of bait or poles respectively.

### questItem (bool)
Whether this item's tooltip will mention it is a quest item.

### mech (bool)
Whether this item will show wires when held.

## tModLoader Only
### public ModItem modItem

The ModItem instance that controls the behavior of this item. This property is null if this is not a modded item.

# Methods
Remember that static methods are called by writing the classname and non-static methods use the instance name. `Item.NewItem(...)` vs `item.CloneDefaults(...)`
### public static int NewItem(int X, int Y, int Width, int Height, int Type, int Stack = 1, bool noBroadcast = false, int pfix = 0, bool noGrabDelay = false, bool reverseLookup = false)
Spawns an item in the world. Commonly seen used in ModNPC.NPCLoot. X, Y, Width, and Height are commonly derived from the npc. 
Example: `Item.NewItem((int)npc.position.X, (int)npc.position.Y, npc.width, npc.height, mod.ItemType("ExampleItem"));`

### public static int NewItem(Rectangle rectangle, int Type, int Stack = 1, bool noBroadcast = false, int prefixGiven = 0, bool noGrabDelay = false, bool reverseLookup = false)
This alternate method signature can simplify code.  
Example: `Item.NewItem(npc.getRect(), mod.ItemType("ExampleItem"));`

## tModLoader Only
### public ItemInfo GetModInfo(Mod mod, string name)

Gets the ItemInfo instance (with the given name and added by the given mod) associated with this item instance.

### public T GetModInfo\<T\>(Mod mod) where T : ItemInfo 

Same as the other GetModInfo, but assumes that the class name and internal name are the same.

### public void CloneDefaults(int type)

Allows you to copy the defaults of a different type of item.