# Basic Dust

Dust is the name we give those tiny particles that add visual elements to weapons and other things. Dust are completely visual and should never be used as a gameplay element. This is because the amount of dust spawned by Terraria is dependent on the capabilities of the computer. 

Each dust has an image and an associated behavior. For example, some dust are affected by gravity, some aren't. Some dust randomly move as if caught in wind flurries, while others move straight. These behaviors are all programmed into the dust.

## Using Dust

To use dust, simply call one of the Dust spawning methods. These are listed below. 

```c#
public static int NewDust(Vector2 Position, int Width, int Height, int Type, float SpeedX = 0f, float SpeedY = 0f, int Alpha = 0, Color newColor = default(Color), float Scale = 1f)
public static Dust NewDustDirect(Vector2 Position, int Width, int Height, int Type, float SpeedX = 0f, float SpeedY = 0f, int Alpha = 0, Color newColor = default(Color), float Scale = 1f)
public static Dust NewDustPerfect(Vector2 Position, int Type, Vector2? Velocity = null, int Alpha = 0, Color newColor = default(Color), float Scale = 1f)
```
NewDust is most commonly used, but NewDustPerfect foregoes the randomization of spawn position and can be useful. Also note that NewDust returns the index of the dust in the Main.dust array while the other 2 return the Dust instance directly.

Here are some examples:
```c#
// Spawning a modded dust from an npc method
Dust.NewDust(npc.position, npc.width, npc.height, mod.DustType("Sparkle"));

// Spawning a random vanilla confetti dust
int dustType = Main.rand.Next(139, 143);
int dustIndex = Dust.NewDust(npc.position, npc.width, npc.height, dustType);

// Spawn dust 73 in ModProjectile.AI, but only 1/6th of the time (so it is less frequent). Also, scaling down velocity.
if (Main.rand.Next(6) == 0)
{
	int dustnumber = Dust.NewDust(projectile.position, projectile.width, projectile.height, 73, 0f, 0f, 200, default(Color), 0.8f);
	Main.dust[dustnumber].velocity *= 0.3f;
}

// Use a for loop to spawn a lot of dust at once. Here is a Magic Mirror style dust spawning. 
for (int d = 0; d < 70; d++)
{
	Dust.NewDust(player.position, player.width, player.height, 15, 0f, 0f, 150, default(Color), 1.5f);
}
```

Some notes: Position, Width, and Height define a rectangle from which the dust will randomly spawn. The only difference between spawning vanilla dust and modded dust is replacing 4th parameter, which is usually just a number, with `mod.DustType("DustName")`. You can omit optional parameters if you want.

## Finding Vanilla Dust

To find vanilla dust, please consult the image below. Note that it is read left to right, starting from 0. Each row is 100 dust long and each row is actually 3 frames of the same dust, meaning there are 3 rows total.

![](https://i.imgur.com/A2YlbEl.png)

This diagram should help explain how to read the image.

![](https://i.imgur.com/Z8tpu1p.png)

The easiest way to try out Dust, however, is to download [Modders Toolkit](https://forums.terraria.org/index.php?threads/modders-toolkit-a-mod-for-modders-doing-modding.55738/) and use the Dust spawning tool to find the dust you need. [Video of Dust Tool](https://gfycat.com/VengefulDearBluetonguelizard)

## ModDust

While vanilla dust are all 10x30 pixel images comprising 3 frames of animation, ModDust can be any size and any amount of frames. See ExampleMod for many different dusts.