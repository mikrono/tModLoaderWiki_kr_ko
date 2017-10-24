Read the [Advanced-Prerequisites](Advanced-Prerequisites) page before continuing.
WIP

# Angular velocity
First, let's disregard what we'll be about on this guide before going over the things you need to understand to follow this guide:
* **Angles**. What is an angle? How is an angle comprised? Where and when is it used?
* **Vectors**. What is a Vector and how and when is it used? You understand Vectors used in a 2D space.
* **Radians**. You understand radians; what are radians, and how do they relate to angles?
* **Spheres/circles**. What is a circle? You know and understand a circle's radius and diameter.
* **Scalars/Psuedo-scalar**. You know what a scalar is.

It _may_ be useful to understand what a **psuedo vector** is, since the angular velocity (or rather angular speed) is one. I will explain it here. A pseudo vector or axial vector is a mathematical object that, like a "normal" vector, has a direction and a magnitude but which, in contrast to an ordinary vector, is invariant under the axis of the underlying axis while an ordinary vector would be opposed to direction to become.

We will start with angular velocity, then move to linear velocity.

## What is angular velocity?
In physics, the angular velocity of an object is the rate of change of its angular displacement with respect to time. I suppose these words are too fancy for some, so let's break it down. You just need to understand the words `angular velocity`. Image a spherical object like a ball, when this object moves it is also spinning or orbiting around its center. The angular velocity basically is a measurement between the relation of the spherical object's velocity and spinning around its center. These rotations around the center are often called a `revolution`, so one revolution being a full spin around the center. We can look at the SI unit of angular velocity to make some more sense out of this, which is `radians per second`. We know a full circle is 2pi, equaling 360 degrees. Because we know a revolution is nothing more than a full spin around the center in a second, so we can visualize this as: `x revs/sec * 2pi radians/rev`, where x is the amount of revolutions we are having. This can be rewritten as the revolutions cancel each other out: `x * 2pi radians/sec`, which I hope makes sense to you intuitively. To give an example before we move on, imagine doing 5 revolutions a second. Look back the definition of a revolution: a full spin around the center. We know a full spin is 2pi, so 1 revolution is 2pi. Meaning for every revolution, we are spinning 2pi a second, so with 5 revolutions per second you're spinning 10pi radians per second. This measurement of _how fast we are spinning around the center point_ is what we call _angular velocity_, or sometimes rather _angular speed_ due to it being a psuedo vector.

## What can we use angular velocity for?
We can use angular velocity to accurately spin an object by its velocity, when implemented well it will work perfectly. You can also in fact spin non spherical objects by angular velocity if you wanted to, but you'd have to use an imaginable sphere as the base.

## Setting up
Before we start spinning, we should setup a helper property for our angular velocity. Remember that our angular velocity is our revolutions per second, meaning the total amount of spins around the center per second. Again, a full spin is 2pi. To relate this to our velocity, we need to find a way to make use of our velocity vector. We can do this by taking the magnitude (length) of our velocity. The magnitude is basically the 'size' of our vector, which is a scalar. If we multiply our magnitude with the total angular displacement of a full spin, we end up with: `2pi * magnitude*`. This defines our angular speed. Let's implement:
```js
get angularVelocity()
{
  Math.PI * 2 * this.velocity.length();
}
```
Because Terraria has useful helpers, you can replace 2pi with `MathHelper.TwoPi` for a constant value, which is very fast compared to continuously calculating:
```js
get angularVelocity()
{
  MathHelper.TwoPi * this.velocity.length();
}
```

## Applying angular velocity
You can apply the angular velocity in many different ways, as you can apply rotation in many different ways in Terraria. One way you can do it, is by adding the angular velocity to the rotation of the entity in the AI. This is useful for things like npcs or projectiles: `this.rotation += this.angularVelocity;`. It is important that you add the angular velocity instead of setting it directly to it, because remember that it defines the angular **displacement**.

If you work with something else, it is likely you should store your own angle variable. Then in any sort of update call, you can apply the same behavior as above.

With custom draw code, you should pass the displaced angle to draw with. The following example assumes you have already applied above behavior somewhere:
```cs
spriteBatch.Draw
(
  texture, position, rectangle, color, rotation + angularVelocity, size, scale, SpriteEffects.None, 0f
);
```
Notice how the angular displacement is added to the given rotation. Understand that this will cause glitching issues if you are nowhere updating the actual rotation.


## Drafting the linear velocity formula with math
Angular velocity = angular speed, denoted by lowercase omega (w) => `x revolutions / sec`

The formula of linear velocity: `v = s/t`, next confine s: `s = arc length = 2pi * r * (theta/360)`
Where r is the radius and theta the angle. Now we can rewrite this with radians:`2pi * r * (theta / 2pi)`, the 2pi cancel each other out, leaving: `r * theta`

So now we have: `v = linear velocity = r * theta / t`. Going back to angular velocity: `w = theta / t`. This formula should make sense, as we said earlier the angular velocity is the angular displacement with respect to time. This is exactly what is displayed here.

So we end up with: `v = r * w`