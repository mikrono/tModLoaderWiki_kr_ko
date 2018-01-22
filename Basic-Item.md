# Basic Item
This guide serves to explain common things shared among all items.

# What is an Item?
It is important to be aware of the distinction between Items, Projectiles, and Tiles. Starting out, you may inadvertently confuse concepts if you aren't clear how they are different. For example, some people might get confused and try to add a workbench item to a recipe when really they wanted to add the workbench tile. It is also important to realize that many something like a boomerang weapon consists of both the item and the projectile. While a simple concept, try to remember.

# Making an Item
To add an item to Terraria, we must first create a "class" that "inherits" from ModItem. To do so, make a .cs file in your mod's source directory (My Games\Terraria\ModLoader\Mod Sources\MyModName) and then open that file in your text editor. Paste the following into that file, replacing `NameHere` with the internal name of your item and `ModNamespaceHere` with your mod's foldername/namespace. (A common mistake is to use apostrophes or spaces in internal names, don't do this, the computer won't understand.)

    using Terraria;
    using Terraria.ID;
    using Terraria.ModLoader;
    
    namespace ModNamespaceHere
    {
        public class NameHere : ModItem
        {
            public override void SetStaticDefaults()
            {
                Tooltip.SetDefault("This is a modded item.");
            }
    
            public override void SetDefaults()
            {
                item.width = 20;
                item.height = 20;
                item.maxStack = 999;
                item.value = 100;
                item.rare = 1;
                // Set other item.X values here
            }
    
            public override void AddRecipes()
            {
                // Recipes here. See Basic Recipe Guide
            }
        }
    }

Now that you have a .cs file, bring in your texture file (a .png image file that you have made) and put it in the folder with this .cs file. Make sure read [Autoload](https://github.com/blushiemagic/tModLoader/wiki/Basic-Autoload) so you know how to satisfy what the computer expects for its filename and folder structure.

# SetDefaults
The most important part of an Item is the SetDefaults. SetDefaults is where you set values for the item, things like what ammo does the item use, how big is the item, and which tile the item places. Below common properties set in SetDefaults are explained.

## value (int)
Value is the number of copper coins the item is worth (aka, cost to buy from a merchant). Setting it to 10462 would mean the item cost 1 gold, 4 silver, and 62 copper. The sell price of an item is one fifth of its value. Value also influences reforge costs with the goblin tinkerer. For convenience, you can also use the `Item.buyPrice()` method for setting values:

`    item.value = Item.buyPrice(0, 1, 4, 62);`

## useStyle (int)
The use style of your item:   
1 for swinging,   
2 for drinking,   
3 act like shortsword,   
4 for use like life crystal,   
5 for use staffs or guns  

## useTurn (bool)
## autoReuse (bool)
Whether the item is in continuous use while the mouse button is held down.

## holdStyle (int)
## useAnimation (int)
The time span of the using animation for the weapon. Recommended to be the same at useTime as this is only the animation.

## useTime (int)
The time span of using the weapon in frames. Note: Terraria runs at 60 frames per second.

## reuseDelay (int)
## consumable (bool)
## rare (int)
Range from -1 to 13. 
Check wiki link for respective colors: https://terraria.gamepedia.com/Rarity

## maxStack (int)
The maximum number of items that can be contained within a single stack.

## width (int)
The width of the item's texture in pixels.

## height (int)
The height of the item's texture in pixels.

## scale
## createTile
## placeStyle
## createWall
## UseSound
The sound that your item makes when used.
Ex: `item.UseSound = SoundID.Item1;`

## damage (int)
The base damage inflicted by the item.

## knockBack (int)
The force of the knock back. Max value is 20. 

## shoot
## shootSpeed
## noMelee
## accessory (bool)
## melee (bool)
Determines weather or not item  is a melee weapon

## magic (bool)
## ranged (bool)
## thrown (bool)
## summon (bool)
The type of damage this item deals, if it is a weapon. Setting more than one is useless.

## defense
## crit
## noUseGraphic
## useAmmo
## ammo
## mana
## channel
## axe
## pick
## hammer
These 3 correspond to the power, but axe power is multiplied by 5, so adjust accordingly.

## potion
## healLife
## healMana
## buffType
## expert
Signifies that this item is an expert mode item.

## bait
## fishingPole
Correspond to fishing power of bait or poles respectively.
