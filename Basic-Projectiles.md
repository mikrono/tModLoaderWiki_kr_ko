# What is a Projectile?

Before you start modding a projectile, you should be aware of the difference between items and projectiles. Items are the objects that can be stored in your inventory, whereas projectiles are the objects that are shot from weapons or enemies, for example.

# What uses Projectiles?

Many items in Terraria are functional due to projectiles, including guns and bows (the bullets and arrows respectively), lasers, bombs and other thrown items, and most magic weapons. Some other items you might not think to be projectiles include; grappling hooks, flails, spears, pets, summons, drills, and yoyos.

# Making a Projectile

To create a projectile in Terraria, you must first create a class that "inherits" from ModProjectile. To do so, make a .cs file in your mod's source directory (My Games\Terraria\ModLoader\Mod Sources\MyModName) and then open that file in your text editor. Paste the following into that file, replacing `NameHere` with the internal name of your item and `ModNamespaceHere` with your mod's foldername/namespace. (A common mistake is to use apostrophes or spaces in internal names, don't do this, the computer won't understand.)

using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

```
namespace ModNamespaceHere
{
    public class NameHere : ModProjectile
    {
        public override void SetDefaults()
        {
            DisplayName.SetDefault("Modded Projectile");
            projectile.width = 16; // The width of the projectile
            projectile.height = 16; // the hight of the projectile
            projectile.timeLeft = 180; // The lifetime
            projectile.penetrate = 2; 
            projectile.tileCollide = false;
            projectile.ignoreWater = false; // Will the projectile slow down in water?
            projectile.friendly = true; // Is this a player or enemy projectile?
            projectile.hostile = false; // """"
            projectile.ranged = true; // The damage type. Aka *.melee, *.magic etc
            projectile.aiStyle = 3; //18 is the demon scythe style
            projectile.light = 1f; // The light value emitted by the projectile
            projectile.stepSpeed = 3; // The speed of the projectile
            // Set other projectile.X values here
        }

    }
}
```