Read the [Advanced-Prerequisites](Advanced-Prerequisites) page before continuing.

# Angular velocity
First, let's disregard what we'll be about on this guide before going over the things you need to understand to follow this guide:
* **Angles**. What is an angle? How is an angle comprised? Where and when is it used?
* **Vectors**. What is a Vector and how and when is it used? You understand Vectors used in a 2D space.
* **Radians**. You understand radians; what are radians, and how do they relate to angles?
* **Spheres/circles**. What is a circle? You know and understand a circle's radius and diameter.
* **Scalars/Psuedo-scalar**. You know what a scalar is.

It **may** be useful to understand what a **psuedo vector** is, since the angular velocity (or rather angular speed) is one. I will explain it here. A pseudor vector or axial vector is a mathematical object that, like a "normal" vector, has a direction and a magnitude but which, in contrast to an ordinary vector, is invariant under the axis of the underlying axis while an ordinary vector would be opposed to direction to become.

## What is angular velocity?
In physics, the angular velocity of an object is the rate of change of its angular displacement with respect to time. I suppose these words are too fancy for some, so let's break it down. You just need to understand the words `angular velocity`. Image a spherical object like a ball, when this object moves it is also spinning or orbiting around its center. The angular velocity basically is a measurement between the relation of the spherical object's velocity and spinning around its center. These rotations around the center are often called a `revolution`, so one revolution being a full spin around the center. We can look at the SI unit of angular velocity to make some more sense out of this, which is `radians per second`. We know a full circle is 2pi, equaling 360 degrees. Because we know a revolution is nothing more than a full spin around the center in a second, so we can visualize this as: `x revs/sec * 2pi radians/rev`, where x is the amount of revolutions we are having. This can be rewritten as the revolutions cancel each other out: `x * 2pi radians/sec`, which I hope makes sense to you intuitively. To give an example before we move on, imagine doing 5 revolutions a second. Look back the definition of a revolution: a full spin around the center. We know a full spin is 2pi, so 1 revolution is 2pi. Meaning for every revolution, we are spinning 2pi a second, so with 5 revolutions per second you're spinning 10pi radians per second. This measurement of _how fast we are spinning around the center point_ is what we call _angular velocity_, or sometimes rather _angular speed_ due to it being a psuedo vector.

## Drafting the formula with math
Angular velocity = angular speed, denoted by lowercase omega (w) => `x revolutions / sec`

Linear velocity: `v = s/t`

`s = arc length = 2pi * r * (theta/360)`


Rewrite arc length with radians:

`2pi * r * (theta / 2pi)` => `r * theta`

so: `v = r * theta / t`

Angular velocity: `w = theta / t`

so: `v = r * w`