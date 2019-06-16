# Time and Timers
When dealing with various aspects of Terraria modding, you'll find yourself wanting to delay actions of make things happen less frequently. In short, you'll want to utilize time and timers to accomplish your modding goals. This guide will go over many time related concepts so that you don't unknowingly make a mistake in your code.

## Wall Time vs Game Time vs World Time
First off, it is important to know the difference between various types of time. 

Wall Time is actual time in the real world. We almost never want to use this in modding because usually our effects run at the same speed as the game or at the speed of the game world. Running things in wall time would mean that while the game is paused or is running at a slower frames per second, the events we are coding would still happen. This would lead to many bugs.

World Time is the reported time of the world. An issue that could arise with improper usage of world time would be events happening extremely quickly while an enchanted sundial is active.

Game time is the time we want to use for gameplay effects most often. Terraria tries to run at 60 frames per second, but weak computers might be running the game in slow motion if they can't maintain 60 FPS. By using game time in our logic, we can account for gameplay effects running at the same relative speed. For example, if an enemy should attack every second, using Wall time would mean that the users running Terraria at 30 FPS would have twice as many attacks to dodge. 

In summary, use Game Time for pretty much everything. Never use Wall Time. Use World Time for things tied to the day/night cycle of the world itself. 

## Game Time

## World Time


## Wall Time
When you are a beginner modder, you may find yourself googling "c# timer" in an attempt to code up something for your mod. If you did, you found examples of using `System.Timers`. Those concepts do not apply to video games or tModLoader modding at all, as they are examples of Wall Time. You'll need to consult [World Time](#world-time) and [Game Time](#game-time) above to determine the correct approach. Again, any usage of `System.Timers` is almost certainly the wrong approach. It'll be buggy and incorrect when the user pauses the game or uses an enchanted sundial.

Main.time
timer you control: npc.ai, projectile.ai

Main.GameUpdateCount