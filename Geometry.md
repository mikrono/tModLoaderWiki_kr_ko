# Why Geometry
Many visually interesting effects and interesting behaviors that a modder would want to implement in their mod require a small bit of geometry knowledge. This guide will explain the most basic usages of geometry in regard to tModLoader modding.

For example, spawning dust or projectiles in an arc, having an enemy shoot towards the player, and writing homing behaviors all make use of geometry. After teaching the basics, the latter part of this guide will have examples of these concepts.

# Prerequisite Knowledge
## Geometry Education
As a programmer, especially one with interest in video games, geometry is an essential skill. As Terraria is a 2D game, the geometry knowledge needed is not extensive, but familiarity with the basics of Vectors is essential.

## Coordinates
Terraria uses different sets of coordinates, and the directions of X and Y might surprise you if you haven't worked in graphics before. See [Coordinates](https://github.com/tModLoader/tModLoader/wiki/Coordinates) and familiarize yourself with world coordinates and the direction of positive X and Y.

## Rotation
Rotation is expressed in radians, not degrees. If you would like to use degrees, simply call the `MathHelper.ToRadians` method. Typically you only want to use degrees for the initial assignment of some behavior, such as declaring that the weapon will shoot in a 30 degree arc. Also note that a rotation of 0 faces to the right. Rotating by 90 degrees will point straight down, as Y points down. 

# Vector2
`Vector2` is a struct that represents a 2 dimensional vector as taught in geometry. A `Vector2` contains 2 fields, `X` and `Y`, representing the magnitude of the `X` and `Y` components of the 2 dimensional vector.

// pictures here teaching basics of vectors

// DrawLine example?

// Vector from A to B example.

## Vector2.Normalize

### Explain multiplying a vector by a scaler

## Vector2.Distance



// Others?

# Random Vectors
Utilizing random vectors is an easy way to add variety to visual and behavior effects. There are many ways to generate a random vector. The diagrams accompanying the following approaches show the distribution an an in-game example of spawning several dust using the approach to illustrate their behavior. 

## Random Vector Within Circle
This approach is the most typical result when a modder wants to make a random vector. The results are well distributed.
```cs
// Normal approach
Vector2 speed = Main.rand.NextVector2Circular(1f, 1f);
// Generate vectors within an arc only. 
Vector2 speed = Main.rand.NextVector2Unit((float)Math.PI / 4, (float)Math.PI / 2) * Main.rand.NextFloat();
// NextVector2Circular allows supplying the width and height radii, for an oval distribution
Vector2 speed = Main.rand.NextVector2Circular(0.5f, 1f);
```    
![](https://i.imgur.com/RJZuLoA.png)     ![](https://i.imgur.com/N6XF5CZ.png)    ![](https://i.imgur.com/wGW3hFn.png)    
![](https://thumbs.gfycat.com/MeaslyRecklessFantail-size_restricted.gif)    ![](https://thumbs.gfycat.com/FoolhardyQuerulousBluewhale-size_restricted.gif)    ![](https://thumbs.gfycat.com/FlakyEvilBrontosaurus-size_restricted.gif)

## Random Vector Along Circle Edge
By generating a random vector that reaches the edges, a modder can generate a random vectors with a consistent length or magnitude.    
```cs
// Normal approach
Vector2 speed = Main.rand.NextVector2Unit();
// Optional parameters allow for specifying a range of rotations. In this example, the start rotation is  Math.PI / 4 and it can be up to Math.PI / 2 more than that.
Vector2 speed = Main.rand.NextVector2Unit((float)Math.PI / 4, (float)Math.PI / 2);
```    
![](https://thumbs.gfycat.com/BlandDismalGnu-size_restricted.gif)   ![](https://thumbs.gfycat.com/PlainImpressionableCricket-size_restricted.gif)   

## Random Vector Within Square
A lot of old Terraria code uses a strange approach to randomizing vectors. Each component, X and Y, are randomly generated in the following manner:   
```cs
float xSpeed = Main.rand.NextFloat(-1f, 1f);
float ySpeed = Main.rand.NextFloat(-1f, 1f);
// Another approach
Vector2 speed = Utils.RandomVector2(Main.rand, -1f, 1f);
```
On first glance, this seems like it should work fine, but this approach actually has a strange distribution that may be unwanted, it actually can generate vectors longer than intended extending out towards the corners of an imaginary square. In the image and gif below, note the odd shape that forms.   
![](https://i.imgur.com/rmPZlwk.png)    
![](https://thumbs.gfycat.com/LightCircularBluefish-size_restricted.gif)    

## Random Vector Square Edge
Not very useful.

# Examples
Now that we have a basic knowledge of geometry and have seen various Vector2 methods that facilitate that knowledge, we can finally use geometry to program interesting behaviors into our mod.

In these examples, you may see Dust or Projectiles being used to illustrate the technique, but they are interchangeable. Just remember to consult the method signature of the method you are using to know the purpose of each parameter.

## Spawn a circle of something
This is the code used for the random vector section above. In this example, we use a for loop to spawn 50 dust, each with a random vector along the edge of the circle. Note that we multiply the vector by 5 to scale it and make it larger, causing the dust to move a good distance.
```cs
for (int i = 0; i < 50; i++) {
	Vector2 speed = Main.rand.NextVector2CircularEdge(1f, 1f);
	Dust d = Dust.NewDustPerfect(Main.LocalPlayer.Top, DustID.BlueCrystalShard, speed * 5, Scale: 1.5f);
	d.noGravity = true;
}
```
We can use some simple geometry to change the spawn location away from the same spot. By adding `speed * 32` to `Main.LocalPlayer.Top`, the dust start in a small circle and expand outward from there instead of all starting in the same spot. 
```cs
Dust d = Dust.NewDustPerfect(Main.LocalPlayer.Top + speed * 32, DustID.BlueCrystalShard, speed * 2, Scale: 1.5f);
```
The speed has been reduced to more easily visualize the effect.    
![](https://thumbs.gfycat.com/ImaginativeMenacingIbex-size_restricted.gif)    

## Shoot at a Target

## Spawn something in an Arc



