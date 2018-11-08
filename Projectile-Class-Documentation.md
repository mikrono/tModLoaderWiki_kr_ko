# Projectile Class Documentation
This page lists methods and fields pertaining to the Projectile class. This page is useful for understanding what to use in `ModProjectile.SetDefaults` and `ModProjectile.AI`. Only common fields and methods are listed. Feel free to add to this list if you find a field that is missing.

Index|
-----|
[Fields](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#fields-and-properties)|
[Methods](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#methods)|

# Fields and Properties
You can assign these fields to give your ModProjectile various values. Typically you'll want to refer to this page when writing code for `ModProjectile.SetDefaults`. Be sure to visit [Vanilla Projectile Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Projectile-Field-Values) to see what values vanilla projectiles use for these fields. All fields listed are public unless otherwise noted. 

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |
| [](#)<a name=""></a>| | |  |

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DD2Summon](#dd2summon)<a name="dd2summon"></a>|bool |false |  |
| [ToolTip](#tooltip)<a name="tooltip"></a>|ItemTooltip|null |  |
| [active](#active)<a name="active"></a>|bool |true | If false, this item is empty regardless of other values set. |
| [alpha](#alpha)<a name="alpha"></a>| int|0 |  |
| [buy](#buy)<a name="buy"></a>| bool|false |  |
| [cartTrack](#carttrack)<a name="carttrack"></a>| bool|false |  |
| [color](#color)<a name="color"></a>| Color|Transparent| Draws the item sprite with a colored tint. Gel and Sharkfin use this to spawn different colored items from the same ItemID. `NetMessage.SendData(88, ...` needs to be used to sync this if not done in SetDefaults.  |
| [dye](#dye)<a name="dye"></a>|byte |0 |  |
| [expertOnly](#expertonly)<a name="expertonly"></a>| bool|false |  |
| [favorited](#favorited)<a name="favorited"></a>| bool|false    | If the item has been marked as favorited in the inventory. |
| [flame](#flame)<a name="flame"></a>| bool|false | |
| [glowMask](#glowmask)<a name="glowmask"></a>|short |-1 |  |
| [hairDye](#hairdye)<a name="hairdye"></a>|short |-1 |  |
| [instanced](#instanced)<a name="instanced"></a>|bool |false |  |
| [lavaWet](#lavawet)<a name="lavawet"></a>|bool |false |  |
| [lifeRegen](#liferegen)<a name="liferegen"></a>|int | 0|  |
| [makeNPC](#makenpc)<a name="makenpc"></a>|short |0 | Spawns the specified NPCID. |
| [manaIncrease](#manaincrease)<a name="manaincrease"></a>|int |0 |  |
| [mountType](#mounttype)<a name="mounttype"></a>|int | -1| Specifies which mount to equip when the item is used. See [CarKey](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Items/CarKey.cs) |
| [netID](#netid)<a name="netid"></a>|int |0 | Don't use. |
| [noWet](#nowet)<a name="nowet"></a>|bool |false |  |
| [paint](#paint)<a name="paint"></a>|byte |0 |  |
| [prefix](#prefix)<a name="prefix"></a>|byte |0 |  |
| [release](#release)<a name="release"></a>|int |0 |  |
| [sentry](#sentry)<a name="sentry"></a>| bool|false |  |
| [shopCustomPrice](#shopcustomprice)<a name="shopcustomprice"></a>|int? |null | Use in `ModNPC/GlobalNPC.SetupShop` to assign a custom price for an item regardless of the value field. Use with shopSpecialCurrency to use a custom currency rather than coins. See [ExampleGlobalNPC.SetupShop](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/NPCs/ExampleGlobalNPC.cs#L190) |
| [shopSpecialCurrency](#shopspecialcurrency)<a name="shopspecialcurrency"></a>|int |-1 | Used in conjunction with [shopCustomPrice](#shopCustomPrice) to specify a custom currency. See [ExampleMod.Load](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/ExampleMod.cs#L71) and [ExampleCustomCurrency](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/ExampleCustomCurrency.cs) |
| [stack](#stack)<a name="stack"></a>|int |1 | Current stack of the Item. |
| [tileBoost](#tileboost)<a name="tileboost"></a>|int |-1 |  |
| [tileWand](#tilewand)<a name="tilewand"></a>|int |0 |  |
| [type](#type)<a name="type"></a>|int |0 | This is the ItemID, automatically set.  |
| [uniqueStack](#uniquestack)<a name="uniquestack"></a>| bool|false |  |
| [vanity](#vanity)<a name="vanity"></a>| bool|false | Specifies that an armor is a vanity item. |
| [wet](#wet)<a name="wet"></a>| bool|false |  |
| [wetCount](#wetcount)<a name="wetcount"></a>|int|0 |  |
| [](#)<a name=""></a>| | |  |

## Static Fields

Static fields are accessed by the classname, not the instance. For example, we write `Item.staff[item.type]` to check if an item is marked as a staff. Set these values in `ModItem.SetStaticDefaults`.

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [staff ](#staff)<a name="staff"></a>| bool[] | false | Indexed by ItemID. This makes the useStyle animate as a staff instead of as a gun. See [ExampleStaff](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Items/Weapons/ExampleStaff.cs). If the staff doesn't rotate, make sure `item.shootSpeed` is not 0. |
| [](#)<a name=""></a>| | |  |

## tModLoader Only

| Field    | Type | Default Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|----------|------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [modItem](#moditem)<a name="moditem"></a>| ModItem | null | The ModItem instance that controls the behavior of this item. This property is null if this is not a modded item. |
| [globalItems](#globalitems)<a name="globalitems"></a>| internal GlobalItem[] | new GlobalItem[0] | Do not touch. Use `Item.GetGlobalItem` |
| [](#)<a name=""></a>| | |  |

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