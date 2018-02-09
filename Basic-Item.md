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

## rare (int)
Range from -1 to 13. 
Check wiki link for respective colors: https://terraria.gamepedia.com/Rarity

## maxStack (int)
The maximum number of items that can be contained within a single stack.

## width (int)
The width of the item's hitbox in pixels.

## height (int)
The height of the item's hitbox in pixels.

## scale (float)
The size multiplier of the item's sprite while the item is being used. Also increases range for melee weapons.

## createTile (int)
The ID of the tile this item places on use.

## placeStyle (int)
The style of the tile being placed. Used for tiles that have a different look depending on the item used to place them.

## createWall (int)
The ID of the tile this item places on use.

## UseSound (LegacySoundStyle)
The sound that your item makes when used.
Ex: `item.UseSound = SoundID.Item1;`

## damage (int)
The base damage inflicted by the item.

## knockBack (int)
The force of the knock back. Max value is 20. 

## shoot (int)
The ID of the projectile that is fired by the item on use.

## shootSpeed (float)
The velocity in pixels the projectile fired by the item will have. Actual velocity depends on the projectile being fired.

## noMelee (bool)
If true, the item's using animation will not deal damage.

## accessory (bool)
Whether the item is an accessory.

## melee (bool)
## magic (bool)
## ranged (bool)
## thrown (bool)
## summon (bool)
The type of damage this item deals, if it is a weapon. Setting more than one is useless.

## defense (int)
The amount of defense this item provides when equipped, either as an accessory or armor.

## crit (int)
The base critical chance for this item.

## noUseGraphic (bool)
If true, the item's sprite will not be visible while the item is in use.

## useAmmo (int)
The ID of the ammo used by this item. 

## ammo (int)
The ID of the ammo class this item is part of.

## notAmmo (bool)
If true and the item is ammo, the item will not count as ammo for certain ammo-specific behavior, such as the tooltip mentioning the item is ammo, or ammo items going into ammo slots first when picked up.

## mana (int)
The amount of mana this item consumes on use.

## channel (bool)
Used for items that have special behavior when the attack button is held.

## axe (int)
## pick (int)
## hammer (int)
These 3 correspond to the power, but axe power is multiplied by 5, so adjust accordingly.

## potion (bool)
If true, this item will inflict potion sickness on use. Also determines whether the item cannot be used when the player has potion sickness, and if the item can be used with the Quick Heal key.

## healLife (int)
The amount of life this item heals on use.

## healMana (int)
The amount of mana this item heals on use.

## buffType (int)
The ID of the buff given by this item on use.

## buffTime (int)
The duration in ticks of the buff given by this item on use.

## expert (bool)
Signifies that this item is an expert mode item.

## bait (int)
## fishingPole (int)
Correspond to fishing power of bait or poles respectively.

## questItem (bool)
Whether this item's tooltip will mention it is a quest item.

## mech (bool)
Whether this item will show wires when held.