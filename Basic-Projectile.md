# What is a Projectile?

Before you start modding a projectile, you should be aware of the difference between items and projectiles. Items are the objects that can be stored in your inventory, whereas projectiles are the objects that are shot from weapons or enemies, for example.

# What uses Projectiles?

Many items in Terraria are functional due to projectiles, including guns and bows (the bullets and arrows respectively), lasers, bombs and other thrown items, and most magic weapons. Some other items you might not think to be projectiles include; grappling hooks, flails, spears, pets, summons, drills, and yoyos.

# Making a Projectile

To create a projectile in Terraria, you must first create a class that "inherits" from ModProjectile. To do so, make a .cs file in your mod's source directory (My Games\Terraria\ModLoader\Mod Sources\MyModName) and then open that file in your text editor. Paste the following into that file, replacing `NameHere` with the internal name of your item and `ModNamespaceHere` with your mod's foldername/namespace. (A common mistake is to use apostrophes or spaces in internal names, don't do this, the computer won't understand.)

using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

```c#
namespace ModNamespaceHere
{
    public class NameHere : ModProjectile
    {
	public override void SetStaticDefaults()
	{
		DisplayName.SetDefault("English Display Name Here");
	}

        public override void SetDefaults()
        {
		projectile.arrow = true;
		projectile.width = 10;
		projectile.height = 10;
		projectile.aiStyle = 1;
		projectile.friendly = true;
		projectile.ranged = true;
		aiType = ProjectileID.WoodenArrowFriendly;
        }

	// Additional hooks/methods here.
    }
}
```
Now that you have a .cs file, bring in your texture file (a .png image file that you have made) and put it in the folder with this .cs file. Make sure read [Autoload](https://github.com/blushiemagic/tModLoader/wiki/Basic-Autoload) so you know how to satisfy what the computer expects for its filename and folder structure.

# SetDefaults
The most important part of a Projectile is the SetDefaults. SetDefaults is where you set values for the projectile, things like the hitbox width and height, if the projectile is friendly or hostile, and which AI the projectile will use. See [Projectile Class Documentation](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation) to see what values commonly set in SetDefaults mean. You can also view vanilla projectile values by visiting [Vanilla Projectile Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Projectile-Field-Values). Many examples of different projectiles can be found in [ExampleMod.Projectiles](https://github.com/blushiemagic/tModLoader/tree/master/ExampleMod/Projectiles)

## projectile.damage
A commons mistake is setting projectile.damage in `SetDefaults`, this does not work, as the damage value a projectile has is always overwritten by the value passed into `Projectile.NewProjectile` when the projectile is spawned. Usually the item or the npc spawning the item will influence the damage.

# AI
The AI of a projectile is the most important aspect of a projectile, it controls how the projectile moves and acts after it is spawned. It is easiest for new modders to first rely on AI code already used in other vanilla projectiles by assigning `projectile.aiStyle = #;` and `aiType = ProjectileID.NameHere;`

## Vanilla AI


# Other Hooks/Methods
The [ModProjectile documentation](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_projectile.html) lists many other hooks/methods you will want to use to make your projectile unique. For example, if you'd like to apply a debuff when the projectile hits, you would use `OnHitNPC`. To do something when the projectile hits a tile, use `OnTileCollide`. See the documentation and usages in ExampleMod to see how to properly use them.
