# Basic Item
This guide serves to explain common things shared among all items.

# What is an Item?
It is important to be aware of the distinction between Items, Projectiles, and Tiles. Starting out, you may inadvertently confuse concepts if you aren't clear how they are different. For example, some people might get confused and try to add a workbench item to a recipe when really they wanted to add the workbench tile. It is also important to realize that many something like a boomerang weapon consists of both the item and the projectile. While a simple concept, try to remember.

# Making an Item
To add an item to Terraria, we must first create a "class" that "inherits" from ModItem. To do so, make a .cs file in your mod's source directory (My Games\Terraria\ModLoader\Mod Sources\MyModName) and then open that file in your text editor. Paste the following into that file, replacing `NameHere` with the internal name of your item and `ModNamespaceHere` with your mod's foldername/namespace. (A common mistake is to use apostrophes or spaces in internal names, don't do this, the computer won't understand.)
```cs
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
```
Now that you have a .cs file, bring in your texture file (a .png image file that you have made) and put it in the folder with this .cs file. Make sure read [Autoload](https://github.com/blushiemagic/tModLoader/wiki/Basic-Autoload) so you know how to satisfy what the computer expects for its filename and folder structure.

# SetDefaults
The most important part of an Item is the SetDefaults. SetDefaults is where you set values for the item, things like what ammo does the item use, how big is the item, and which tile the item places. See [Item Class Documentation](https://github.com/blushiemagic/tModLoader/wiki/Item-Class-Documentation) to see what values commonly set in SetDefaults mean. You can also view vanilla item values by visiting [Vanilla Item Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Item-Field-Values). 

