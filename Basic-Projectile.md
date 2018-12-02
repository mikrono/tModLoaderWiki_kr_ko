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

## Timers
Many projectiles use timers to delay actions. Typically we use `projectile.ai[0]` or `projectile.ai[1]` as those values are synced automatically, but we can also use class fields as well. Here we count to 30, or in other words, half a second.

```cs
projectile.ai[0] += 1f;
if (projectile.ai[0] >= 30f)
{
	// Half a second has passed. Reset timer, etc.
	projectile.ai[0] = 0f;
	projectile.netUpdate = true;
	// Do something here, maybe change to a new state.
}
```

## Gravity
Gravity doesn't actually exist for projectiles, every projectile that moves with gravity actually just has code in their AI. To implement gravity, simply add a small value to `projectile.velocity.Y`:
```cs
projectile.velocity.Y = projectile.velocity.Y + 0.1f; // 0.1f for arrow gravity, 0.4f for knife gravity
if (projectile.velocity.Y > 16f) // This check implements "terminal velocity". We don't want the projectile to keep getting faster and faster. Past 16f this projectile will travel through blocks, so this check is useful.
{
	projectile.velocity.Y = 16f;
}
```

### Delayed Gravity
Arrows and Throwing Knife projectiles all wait several frames before being affected by gravity:   
```cs
projectile.ai[0] += 1f; // Use a timer to wait 15 ticks before applying gravity.
if (projectile.ai[0] >= 15f)
{
	projectile.ai[0] = 15f;
	projectile.velocity.Y = projectile.velocity.Y + 0.1f;
}
if (projectile.velocity.Y > 16f)
{
	projectile.velocity.Y = 16f;
}
```

## Wind Resistance
By reducing projectile.velocity.X multiplicity, we can easily implement wind resistance. Combine with a timer to have this effect conditionally.
```cs
projectile.velocity.X = projectile.velocity.X * 0.97f; // 0.99f for rolling grenade speed reduction. Try values between 0.9f and 0.99f
```

## Rotation
### Constant Rotation
We can increase projectile.rotation in AI to rotate like a boomerang.
```cs
projectile.rotation += 0.4f * (float)projectile.direction;
```

### Face Forward
Rotating in the direction of travel is often used in projectiles like arrows. If your projectile faces right, you don't need to add `MathHelper.PiOver2` (found in Microsoft.Xna.Framework). If your projectile points up, you'll need to.
```cs
projectile.rotation = projectile.velocity.ToRotation() + MathHelper.PiOver2; // projectile sprite faces up
// or
projectile.rotation = projectile.velocity.ToRotation(); // projectile faces sprite right
```

## Dust
Spawn dust in AI for a visual effect. Randomizing placement, dustid, and frequency is visually pleasing. Here is the Enchanted boomerang dust spawn (aiStyle 3, aiType ProjectileID.EnchantedBoomerang):    
```cs
if (Main.rand.Next(5) == 0) // only spawn 20% of the time
{
	int choice = Main.rand.Next(3); // choose a random number: 0, 1, or 2
	if (choice == 0) // use that number to select dustID: 15, 57, or 58
	{
		choice = 15;
	}
	else if (choice == 1)
	{
		choice = 57;
	}
	else
	{
		choice = 58;
	}
	// Spawn the dust
	Dust.NewDust(projectile.position, projectile.width, projectile.height, choice, projectile.velocity.X * 0.25f, projectile.velocity.Y * 0.25f, 150, default(Color), 0.7f);
}
```

## Lighting
Modders have many different definitions of lighting. If you want to add particles, see the Dust section. If you want the projectile texture to be un-affected by lighting, see `ModProjectile.GetAlpha`. If you want the projectile to give off white light, you can set `projectile.light = 1f;` (or any number between 0 and 1) in `SetDefaults`. Finally, if you want to give off color light NOT from spawned dust, light that lights up nearby tiles, use Lighting.AddLight:
```cs
Lighting.AddLight(projectile.Center, 0.9f, 0.1f, 0.3f); // R G B values from 0 to 1f. This is the red from the Crimson Heart pet
```

## Sound
### Repeating Sound
The field `soundDelay` will automatically decrease each frame. Checking that it is 0 and then setting it to a value and playing a sound will result in a repeating sound. This example is from the boomerang aiStyle (3).
```cs
if (projectile.soundDelay == 0)
{
	projectile.soundDelay = 8;
	Main.PlaySound(SoundID.Item7, projectile.position);
}
```

## Splitting/Spawning Projectiles
Crystal Bullet and the Scourge of the Corruptor projectile (EatersBite) both spawn new projectiles when they die. We typically see spawning projectiles in `Kill` or `OnTileCollide`, but we can do it in `AI` as well. When spawning projectiles, we need to be aware of [Multiplayer Compatibility](https://github.com/blushiemagic/tModLoader/wiki/Multiplayer-Compatibility#projectile--modprojectile) and be sure to only spawn projectiles when `Main.myPlayer == projectile.owner` is true to prevent issues. Scaling down projectile.damage is typical. See [Projectile.NewProjectile](https://github.com/blushiemagic/tModLoader/wiki/Projectile-Class-Documentation#public-static-int-newprojectilefloat-x-float-y-float-speedx-float-speedy-int-type-int-damage-float-knockback-int-owner--255-float-ai0--0f-float-ai1--0f) to see the parameters.
```cs
// This code spawns 3 projectiles in the opposite direction of the projectile, with random variance in velocity.
if (OptionallySomeCondition && projectile.owner == Main.myPlayer)
{
	for (int i = 0; i < 3; i++)
	{
		// Calculate new speeds for other projectiles.
		// Rebound at 40% to 70% speed, plus a random amount between -8 and 8
		float speedX = -projectile.velocity.X * Main.rand.NextFloat(.4f, .7f) + Main.rand.NextFloat(-8f, 8f);
		float speedY = -projectile.velocity.Y * Main.rand.Next(40, 70) * 0.01f + Main.rand.Next(-20, 21) * 0.4f; // This is Vanilla code, a little more obscure.
		// Spawn the Projectile.
		Projectile.NewProjectile(projectile.position.X + speedX, projectile.position.Y + speedY, speedX, speedY, 90, (int)(projectile.damage * 0.5), 0f, projectile.owner, 0f, 0f);
	}
}
```

## Homing
// TODO

## Follow Mouse
// TODO

## Fade In/Out
Many bullets fade in so that when they spawn they don't overlap the gun muzzle they appear from. You can set the projectile to spawn transparent with `projectile.alpha = 255;` in `SetDefaults`.
```cs
if (projectile.alpha > 0)
{
	projectile.alpha -= 15; // Decrease alpha, increasing visibility.
}
```
[`ExampleAnimatedPierce`](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Projectiles/ExampleAnimatedPierce.cs) shows using both fading in and out.

## Animation/Multiple Frames
Projectile animation, switching which frame of the sprite to draw, happens in AI. Make sure to set `Main.projFrames[projectile.type] = #;` in `SetStaticDefaults` first. You can set `projectile.frame` to whatever frame you want to be drawn. 

### Looping/Cycling
You can use `projectile.frameCounter` and `Main.projFrames[projectile.type]` to implement a looping animation. Example: [ExampleAnimatedPierce](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Projectiles/ExampleAnimatedPierce.cs)
```cs
// Loop through the 4 animation frames, spending 5 ticks on each.
if (++projectile.frameCounter >= 5)
{
	projectile.frameCounter = 0;
	if (++projectile.frame >= Main.projFrames[projectile.type])
	{
		projectile.frame = 0;
	}
}
// Or, more compactly:
if (++projectile.frameCounter >= 5)
{
	projectile.frameCounter = 0;
	projectile.frame = ++projectile.frame % Main.projFrames[projectile.type];
}
```