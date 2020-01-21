# What is a Projectile?

Before you start modding a projectile, you should be aware of the difference between items and projectiles. Items are the objects that can be stored in your inventory, whereas projectiles are the objects that are shot from weapons or enemies, for example.

# What uses Projectiles?

Many items in Terraria are functional due to projectiles, including guns and bows (the bullets and arrows respectively), lasers, bombs and other thrown items, and most magic weapons. Some other items you might not think to be projectiles include; grappling hooks, flails, spears, pets, summons, drills, and yoyos.

# Making a Projectile

To create a projectile in Terraria, you must first create a class that "inherits" from ModProjectile. To do so, make a .cs file in your mod's source directory (My Games\Terraria\ModLoader\Mod Sources\MyModName) and then open that file in your text editor. Paste the following into that file, replacing `NameHere` with the internal name of your item and `ModNamespaceHere` with your mod's foldername/namespace. (A common mistake is to use apostrophes or spaces in internal names, don't do this, the computer won't understand.)


```c#
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

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
Now that you have a .cs file, bring in your texture file (a .png image file that you have made) and put it in the folder with this .cs file. Make sure read [Autoload](https://github.com/tModLoader/tModLoader/wiki/Basic-Autoload) so you know how to satisfy what the computer expects for its filename and folder structure.

# I can't find my Projectile
Remember that Items and Projectiles are different. A common mistake is modders will make a projectile and not understand that they need to make something use that projectile. For example, for a throwing knife weapon, you need to make both an Item and a Projectile. Ammo items need a unique projectile associated with it as well. You don't always need both and item and a projectile, such as if the projectile is spawned by an npc. The easiest way to test a projectile is to make an item and set item.shoot to the projectile. For example, `item.shoot = mod.ProjectileType("MyProjectile")`. See ExampleMod for many examples of Projectiles spawned by Items, they are in separate folders, but they are easy to find. 

# SetDefaults
The most important part of a Projectile is the SetDefaults. SetDefaults is where you set values for the projectile, things like the hitbox width and height, if the projectile is friendly or hostile, and which AI the projectile will use. See [Projectile Class Documentation](https://github.com/tModLoader/tModLoader/wiki/Projectile-Class-Documentation) to see what values commonly set in SetDefaults mean. You can also view vanilla projectile values by visiting [Vanilla Projectile Field Values](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Projectile-Field-Values). Many examples of different projectiles can be found in [ExampleMod.Projectiles](https://github.com/tModLoader/tModLoader/tree/master/ExampleMod/Projectiles)

## projectile.damage
A commons mistake is setting projectile.damage in `SetDefaults`, this does not work, as the damage value a projectile has is always overwritten by the value passed into `Projectile.NewProjectile` when the projectile is spawned. Usually the item or the npc spawning the item will influence the damage.

## drawOffsetX, drawOriginOffsetY, drawOffsetX
These are ModProjectile fields related to properly centering a hitbox to a sprite. Read [Drawing and Collision](#drawing-and-collision) for more info.

# Other Hooks/Methods
The [ModProjectile documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_projectile.html) lists many other hooks/methods you will want to use to make your projectile unique. For example, if you'd like to apply a debuff when the projectile hits, you would use `OnHitNPC`. To do something when the projectile hits a tile, use `OnTileCollide`. See the documentation and usages in ExampleMod to see how to properly use them.

# What is AI
The AI of a projectile is the most important aspect of a projectile, it controls how the projectile moves and acts after it is spawned. It is easiest for new modders to first rely on AI code already used in other vanilla projectiles by assigning `projectile.aiStyle = #;` and `aiType = ProjectileID.NameHere;`. This is called mimicking a vanilla projectile. As you desire more advanced movement, you'll realize that mimicking vanilla projectile AI is very limited. We will discuss mimicking and custom AI below.

# Using Vanilla AI
We can use vanilla AI to prototype our projectiles. Let's make a boomerang. Using the same aiStyle as the vanilla projectiles that move like a boomerang, we can make a boomerang. You can look up boomerang projectiles in [Vanilla Projectile Field Values](https://github.com/tModLoader/tModLoader/wiki/Vanilla-Projectile-Field-Values) and you will discover that boomerangs all use aiStyle of 3:    
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
That dust is cool, but if you want to change the color of that dust or any other small thing, you can't rely on `aiStyle` and `aiType`. To change things, you'll need to consult the [Vanilla Code Adaption](https://github.com/tModLoader/tModLoader/wiki/Advanced-Vanilla-Code-Adaption) guide to tweak existing code or read on to learn how to do AI code from scratch. Remember, using `projectile.aiStyle` and `aiType` is a prototyping tool, anything remotely interesting in a mod would likely need to write their own AI code or adapt vanilla code.

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

### spriteDirection
If your sprite is upside-down when shot to the left, you'll want to set this: `projectile.spriteDirection = projectile.direction;` See [Drawing and Collision](#drawing-and-collision) for an explanation and example.

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
### Dust Trail
A dust trail can be accomplished by spawning 1 dust every AI update.

## Lighting
Modders have many different definitions of lighting. If you want to add particles, see the Dust section. If you want the projectile texture to be un-affected by lighting, see `ModProjectile.GetAlpha`. If you want the projectile to give off white light, you can set `projectile.light = 1f;` (or any number between 0 and 1) in `SetDefaults`. Finally, if you want to give off color light NOT from spawned dust, light that lights up nearby tiles, use Lighting.AddLight inside your `AI` method:
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
Crystal Bullet and the Scourge of the Corruptor projectile (EatersBite) both spawn new projectiles when they die. We typically see spawning projectiles in `Kill` or `OnTileCollide`, but we can do it in `AI` as well. When spawning projectiles, we need to be aware of [Multiplayer Compatibility](https://github.com/tModLoader/tModLoader/wiki/Multiplayer-Compatibility#projectile--modprojectile) and be sure to only spawn projectiles when `Main.myPlayer == projectile.owner` is true to prevent issues. Scaling down projectile.damage is typical. See [Projectile.NewProjectile](https://github.com/tModLoader/tModLoader/wiki/Projectile-Class-Documentation#public-static-int-newprojectilefloat-x-float-y-float-speedx-float-speedy-int-type-int-damage-float-knockback-int-owner--255-float-ai0--0f-float-ai1--0f) to see the parameters.
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
In short:
You can loop Main.npc, and select a valid target. Then for the target your projectile has, you adjust the velocity of the projectile so it moves towards the target.

## Follow Mouse
// TODO
If you want the projectile to be exactly on the cursor, just set projectile.position to Main.MouseWorld in the AI:
`projectile.position = Main.MouseWorld` 

## Held Projectile
// TODO

## Fade In/Out
Many bullets fade in so that when they spawn they don't overlap the gun muzzle they appear from. You can set the projectile to spawn transparent with `projectile.alpha = 255;` in `SetDefaults`.
```cs
if (projectile.alpha > 0)
{
	projectile.alpha -= 15; // Decrease alpha, increasing visibility.
}
```
[`ExampleAnimatedPierce`](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Projectiles/ExampleAnimatedPierce.cs) shows using both fading in and out.

## Animation/Multiple Frames
Projectile animation, switching which frame of the sprite to draw, happens in AI. Make sure to set `Main.projFrames[projectile.type] = #;` in `SetStaticDefaults` first. You can set `projectile.frame` to whatever frame you want to be drawn. 

### Looping/Cycling
You can use `projectile.frameCounter` and `Main.projFrames[projectile.type]` to implement a looping animation. Example: [ExampleAnimatedPierce](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Projectiles/ExampleAnimatedPierce.cs)
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

# Drawing and Collision
You may find yourself noticing that your projectile is hitting walls when it shouldn't or otherwise having a weird hitbox. First off, it is worth reiterating that `projectile.width` and `projectile.height` correspond to the hitbox of the projectile, NOT the sprite used. You almost never want `width` or `height` to be different, it should be square. You also never want to use `projectile.scale` since the vanilla drawing code doesn't really take it into account correctly. The drawing of the sprite attempts to overlay the hitbox with the sprite, the drawing of this sprite is influenced by various bits of math done in the `Main.DrawProj` method.

## Vertical Sprite Example
Lets work through this example as we explore collision and drawing issues and work to solve them. Here is the sprite, it is 48x70 pixels:    
![](https://i.imgur.com/y4OcJAv.png)    
The important parts of this ModProjectile are as follows:    
```cs
// SetDefatults
projectile.width = 8;
projectile.height = 8;
// AI
projectile.rotation = projectile.velocity.ToRotation() + MathHelper.ToRadians(90f);
```
Our goal is to have the yellow part of this projectile be the hitbox. The yellow area is 8 by 8 pixels, so we set `width` and `height` to 8 already. The `projectile.rotation` code there sets the rotation to the velocity while adding 90 degrees of rotation, since the sprite we happen to be using faces up instead of to the right as is expected by the game. In this guide, we'll be using the [Modders Toolkit](https://forums.terraria.org/index.php?threads/modders-toolkit-a-mod-for-modders-doing-modding.55738/) mod to visualize hitboxes. This is very useful.

Here we see the hitbox, the yellow square, doesn't match up with the tip of our sprite: [High Quality Video](https://gfycat.com/SimpleMinorImperialeagle)    
![](https://thumbs.gfycat.com/SimpleMinorImperialeagle-small.gif)    
The math for what vanilla code is doing is a little confusing, but basically we need to set `drawOffsetX` and `drawOriginOffsetY` to values that offset the drawing of our sprite in an attempt to properly place the sprite over the hitbox. If you are attempting this, either use [Modders Toolkit](https://forums.terraria.org/index.php?threads/modders-toolkit-a-mod-for-modders-doing-modding.55738/) to change the offset values in-game or use [Edit and Continue](https://github.com/tModLoader/tModLoader/wiki/Why-Use-an-IDE#edit-and-continue) to adjust the values in-game. Another approach is to just measure it out on the sprite itself in your graphics program:    
![](https://i.imgur.com/m5DxkBm.png)   
Here we see testing various values with Modders Toolkit. Make sure to replicate these values in your `SetDefaults` code: [High Quality Video](https://gfycat.com/MintyCharmingCopperhead)    
![](https://thumbs.gfycat.com/MintyCharmingCopperhead-small.gif)     
After some experimentation or measuring, we know that adding `drawOffsetX = -20;` to this `ModProjectile.SetDefaults` will fix the positioning of the drawing relative to the hitbox.

Lets now try to position the hitbox over the blue portion of our sprite. This time, lets use [Edit and Continue](https://github.com/tModLoader/tModLoader/wiki/Why-Use-an-IDE#edit-and-continue) to accomplish this. In the clip below, you can see how quickly we can test out new values: [High Quality Video](https://gfycat.com/WebbedUntimelyHarborseal)      
![](https://thumbs.gfycat.com/WebbedUntimelyHarborseal-small.gif)    
As you saw, we added `drawOriginOffsetY = -16;` to position the hitbox lower on the sprite.

### Fixing upside-down sprite problem
You might've noticed that the sprite is upside down when fired to the left. Remember that in our `AI`, we have this line of code: `projectile.rotation = projectile.velocity.ToRotation() + MathHelper.ToRadians(90f);`. If we rotate our sprite to the left, then it is upside-down. We can fix this with `spriteDirection`. `spriteDirection` will flip the drawing of the sprite horizontally. To implement this, simply add `projectile.spriteDirection = projectile.direction;` to the `AI` code after the `projectile.rotation = ` line. 

No fix:     
![](https://i.imgur.com/sKUq94z.png)    
Fixed:     
![](https://i.imgur.com/w3ALhDX.png)    

## Horizontal Sprite Example
Things change a little if your sprite is oriented horizontally. Here is our new horizontal sprite, which is now 70x48 and oriented horizontally, pointing to the right instead of up as before:    
![](https://i.imgur.com/etzbzs0.png)

Once again, we can see that the hitbox doesn't line up:    
![](https://thumbs.gfycat.com/ConfusedSardonicCowbird-small.gif)     

Unlike the horizontal example, this time we set `projectile.rotation = projectile.velocity.ToRotation();` directly instead of adding additional 90 degrees. After some experimentation, we arrive at the following for a hitbox on the tip:
```cs
drawOffsetX = -62;
drawOriginOffsetY = -20; 
drawOriginOffsetX = 31;
```
These values are a bit odd because of some math Terraria is doing, so here is the algorithm for calculating them:
```cs
drawOffsetX = Negative X pixel position of the top left corner of the intended hitbox
drawOriginOffsetY = Negative Y pixel position of the top left corner of the intended hitbox
drawOriginOffsetX = X pixel position of center of hitbox minus Texture Width divided by 2 
```
Here is a diagram:
![](https://i.imgur.com/zQfxXM3.png)    
If you don't like fighting against the vanilla projectile rendering code, you can always draw the projectile yourself as seen in [ExampleAnimatedPierce Projectile](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Projectiles/ExampleAnimatedPierce.cs#L114)

### Fixing upside-down sprite problem again
With the vertical sprite, using `projectile.spriteDirection` works because it controls a horizontal flip of the projectile sprite. Using a horizontal sprite, a horizontal flip makes the sprite move facing backwards:    
![](https://i.imgur.com/vfKrRzZ.png)    
To fix this, we need to adjust the offsets dynamically and conditionally add 180 degrees or Pi to the rotation. Here is the code:
```cs
// Set both direction and spriteDirection to 1 or -1 (right and left respectively)
// projectile.direction is automatically set correctly in Projectile.Update, but we need to set it here or the textures will draw incorrectly on the 1st frame.
projectile.spriteDirection = projectile.direction = (projectile.velocity.X > 0).ToDirectionInt();
// Adding Pi to rotation if facing left corrects the drawing
projectile.rotation = projectile.velocity.ToRotation() + (projectile.spriteDirection == 1 ? 0f : MathHelper.Pi);
if (projectile.spriteDirection == 1) // facing right
{
	drawOffsetX = -62; // These values match the values in SetDefaults
	drawOriginOffsetY = -20;
	drawOriginOffsetX = 31;
}
else
{
	// Facing left.
	// You can figure these values out if you flip the sprite in your drawing program.
	drawOffsetX = 0; // 0 since now the top left corner of the hitbox is on the far left pixel.
	drawOriginOffsetY = -20; // doesn't change
	drawOriginOffsetX = -31; // Math works out that this is negative of the other value.
}
```
![](https://i.imgur.com/FKfhtQ0.png)    

Hopefully these answers can help you solve your projectile hitbox and drawing issues.
