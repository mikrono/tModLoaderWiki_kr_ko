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

# I can't find my Projectile
Remember that Items and Projectiles are different. A common mistake is modders will make a projectile and not understand that they need to make something use that projectile. For example, for a throwing knife weapon, you need to make both an Item and a Projectile. Ammo items need a unique projectile associated with it as well. You don't always need both and item and a projectile, such as if the projectile is spawned by an npc. The easiest way to test a projectile is to make an item and set item.shoot to the projectile. For example, `item.shoot = mod.ProjectileType("MyProjectile")`. See ExampleMod for many examples of Projectiles spawned by Items, they are in separate folders, but they are easy to find. 

# SetDefaults
The most important part of a Projectile is the SetDefaults. SetDefaults is where you set values for the projectile, things like the hitbox width and height, if the projectile is friendly or hostile, and which AI the projectile will use. See [Projectile Class Documentation](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation) to see what values commonly set in SetDefaults mean. You can also view vanilla projectile values by visiting [Vanilla Projectile Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Projectile-Field-Values). Many examples of different projectiles can be found in [ExampleMod.Projectiles](https://github.com/blushiemagic/tModLoader/tree/master/ExampleMod/Projectiles)

## projectile.damage
A commons mistake is setting projectile.damage in `SetDefaults`, this does not work, as the damage value a projectile has is always overwritten by the value passed into `Projectile.NewProjectile` when the projectile is spawned. Usually the item or the npc spawning the item will influence the damage.

# Other Hooks/Methods
The [ModProjectile documentation](http://blushiemagic.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_projectile.html) lists many other hooks/methods you will want to use to make your projectile unique. For example, if you'd like to apply a debuff when the projectile hits, you would use `OnHitNPC`. To do something when the projectile hits a tile, use `OnTileCollide`. See the documentation and usages in ExampleMod to see how to properly use them.

# What is AI
The AI of a projectile is the most important aspect of a projectile, it controls how the projectile moves and acts after it is spawned. It is easiest for new modders to first rely on AI code already used in other vanilla projectiles by assigning `projectile.aiStyle = #;` and `aiType = ProjectileID.NameHere;`. This is called mimicking a vanilla projectile. As you desire more advanced movement, you'll realize that mimicking vanilla projectile AI is very limited. We will discuss mimicking and custom AI below.

# Using Vanilla AI
Let's make a boomerang. Using the same aiStyle as the vanilla projectiles that move like a boomerang, we can make a boomerang. You can look up boomerang projectiles in [Vanilla Projectile Field Values](https://github.com/blushiemagic/tModLoader/wiki/Vanilla-Projectile-Field-Values) and you will discover that boomerangs all use aiStyle of 3:    
![](https://i.imgur.com/RSaxV6T.png)    

We can now use `projectile.aiStyle = 3;` in our code. To make this boomerang even easier, we can use `projectile.CloneDefaults(ProjectileID.EnchantedBoomerang)`, which will copy all the other defaults as well. Doing this, you will get a projectile that almost behaves the same way as the vanilla projectile:    
![](https://i.imgur.com/CL2MwaF.png)     

You'll notice that the dust aren't being spawned. We can fix this by using `aiType`. `aiType` is used to further narrow down `projectile.aiStyle`. Each `aiStyle` is shared between many different projectiles. If we want to use a specific behavior of a particular type of projectile, we need to set `aiType`. Here is how our copy of EnchantedBoomerang looks after assigning `aiType` as well:    
![](https://i.imgur.com/39KqXhc.png)    

Here is the resulting code.
```cs
public override void SetDefaults()
{
	projectile.CloneDefaults(ProjectileID.EnchantedBoomerang);
	// projectile.aiStyle = 3; This line is not needed since CloneDefaults sets it.
	aiType = ProjectileID.EnchantedBoomerang;
}
```
That dust is cool, but if you want to change the color of that dust or any other small thing, you can't rely on `aiStyle` and `aiType`. To change things, you'll need to consult the [Vanilla Code Adaption](https://github.com/blushiemagic/tModLoader/wiki/Advanced-Vanilla-Code-Adaption) guide to tweak existing code or read on to learn how to do AI code from scratch.

# Custom AI
This section will discuss elements you can incorporate into your AI. Remember to set `projectile.aiStyle` back to 0 if you are using `projectile.CloneDefaults` to copy other projectile defaults. All code for custom AI goes into the `ModProjectile.AI` method.


