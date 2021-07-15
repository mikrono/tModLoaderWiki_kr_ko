# Why Geometry
Many visually interesting effects and interesting behaviors that a modder would want to implement in their mod require a small bit of geometry knowledge. This guide will explain the most basic usages of geometry in regard to tModLoader modding.

For example, spawning dust or projectiles in an arc, having an enemy shoot towards the player, and writing homing behaviors all make use of geometry. After teaching the basics, the latter part of this guide will have examples of these concepts.

# Prerequisite Knowledge
## Geometry Education
As a programmer, especially one with interest in video games, geometry is an essential skill. As Terraria is a 2D game, the geometry knowledge needed is not extensive, but familiarity with the basics of Vectors is essential.

## Coordinates
Terraria uses different sets of coordinates, and the directions of X and Y might surprise you if you haven't worked in graphics before. See [Coordinates](https://github.com/tModLoader/tModLoader/wiki/Coordinates) and familiarize yourself with world coordinates and the direction of positive X and Y.

# Vector2
`Vector2` is a struct that represents a 2 dimensional vector as taught in geometry. A `Vector2` contains 2 fields, `X` and `Y`, representing the magnitude of the `X` and `Y` components of the 2 dimensional vector.

// pictures here teaching basics of vectors

// DrawLine example?

// Vector from A to B example.

## Vector2.Normalize

## Vector2.Distance

// Others?

# Random Vectors
Utilizing random vectors is an easy way to add variety to visual and behavior effects. There are many ways to generate a random vector.

## Random Vector Along Circle Edge

## Random Vector Within Circle

## Random Vector Square Edge

## Random Vector Within Square

# Examples
Now that we have a basic knowledge of geometry and have seen various Vector2 methods that facilitate that knowledge, we can finally use geometry to program interesting behaviors into our mod.

## Shoot at a Target

## Spawn something in an Arc



