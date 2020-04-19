### These are the things you can use in the ModMount's SetDefaults

| MountData | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| :--  | :--                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| mountData.spawnDust (int) | The ID of the dust spawned when mounted or dismounted.|
| mountData.buff (int) | The ID number of the buff assigned to the mount.|
| mountData.abilityChargeMax (int) | |
| mountData.abilityCooldown (int) | |
| mountData.abilityDuration (int) | |
| mountData.swimSpeed (float) | This parameter is used in vanilla for the turtle mount, the drill mount, and the cute fishron mount. It represents the horizontal velocity, how many pixels on the x axis it can move per frame. |
| mountData.heightBoost (int) | How many pixels the player is boosted in the vertical axis when mounted. |
| mountData.fallDamage (float) | A number which is the fall damage multiplier. |
| mountData.runSpeed (float) | |
| mountData.dashSpeed (float) | |
| mountData.flightTimeMax (int) | |
| mountData.jumpHeight (int) | | 
| mountData.acceleration (float) | The rate at which the mount falls faster down the y axis due to gravity when in the air. To find the speed in pixels per frame that you will be falling while mounted at any given time will be your existing Y velocity - Acceleration + Gravity + 0.001. Gravity will be a value between 0.25 and 1 depending on your X position in relation to the world's surface position divided by 6. |
| mountData.jumpSpeed (float) | |
| mountData.blockExtraJumps (boolean) | |
| mountData.totalFrames (int) | This is the number of frames in the Mount's texture. Certain keyframes are used to represent a state of the mount at the given time. 0 is standing, 1 is running, 2 is freefalling, 3 is flying, 4 is swimming, and 5 is dashing|
| mountData.constantJump (boolean) | |
