### These are the things you can use in the ModMount's SetDefaults

| MountData | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| :--  | :--                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| mountData.spawnDust (int) | The ID of the dust spawned when mounted or dismounted.|
| mountData.buff (int) | The ID number of the buff assigned to the mount.|
| mountData.abilityChargeMax (int) | Used in the Scutlix mount, represents the maximum amount of time in frames a mount will charge its ability until it stops charging. In the case of the Scutlix mount, it affects the rate of change of the lighting on the mount's eyes. The higher the value is for the Scutlix mount, the slower the blue value of the color value of the lighting on the Scutlix's eyes will raise. |
| mountData.abilityCooldown (int) | The amount of time in frames before the mount can call Mount.UseAbility or the ModMount can callq MountLoader.UseAbility.  |
| mountData.abilityDuration (int) | How long, in frames, the crosshair on the Scutlix mount will exist on screen after the ability has been used. |
| mountData.swimSpeed (float) | This parameter is used in vanilla for the turtle mount, the drill mount, and the cute fishron mount. It represents the horizontal velocity, how many pixels on the x axis it can move per frame. |
| mountData.heightBoost (int) | How many pixels the player is boosted in the vertical axis when mounted. |
| mountData.fallDamage (float) | A number which is the fall damage multiplier. |
| mountData.runSpeed (float) | The amount of pixels the mount can move left or right, possibly modified by other variables before applied to the player's position update next frame. |
| mountData.dashSpeed (float) | The speed the mount moves |
| mountData.flightTimeMax (int) | |
| mountData.jumpHeight (int) | | 
| mountData.acceleration (float) | The rate at which the mount falls faster down the y axis due to gravity when in the air. To find the speed in pixels per frame that you will be falling while mounted at any given time will be your existing Y velocity - Acceleration + Gravity + 0.001. Gravity will be a value between 0.25 and 1 depending on your X position in relation to the world's surface position divided by 6. |
| mountData.jumpSpeed (float) | |
| mountData.blockExtraJumps (boolean) | |
| mountData.totalFrames (int) | This is the number of frames in the Mount's texture. Certain keyframes are used to represent a state of the mount at the given time. 0 is standing, 1 is running, 2 is freefalling, 3 is flying, 4 is swimming, and 5 is dashing|
| mountData.constantJump (boolean) | |
