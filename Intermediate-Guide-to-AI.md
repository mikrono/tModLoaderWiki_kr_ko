The following guide aims to teach some of the common ways of creating AI for both projectiles and npcs.  Most things can be used interchangeably between the 2, just remember to replace npc with projectile and vice versa.  
To start, let's make a basic npc. This guide won't cover the things in set defaults, you can look at example mod and the vanilla field values (https://github.com/tModLoader/tModLoader/wiki/Vanilla-NPC-Field-Values) to get an idea of what to put.  Most of the AI we make will go into the AI hook, so override that.  
```cs
//other stuff
public override void AI(){
     //AI is called every frame.  Anything you put here will happen 60 times a second.
}
```

We now have a place to put our ai, but it doesn't do anything yet - let's start by changing the npcs velocity.  The velocity is a vector2(which is composed of 2 floats, x and y) corresponding to the x and y change of the npc every frame(60 times a second) in pixels.  A velocity of -5,5 means left 5 and down 5 pixels a frame(Keep in mind in Terraria, adding to y goes down). Changing velocity is the most common way to make your npc move and it's kept in between frames - if I set it to 5,5 it will stay that way till something else changes it. 
Let's make the npc float upwards by changing the velocity. 
```cs
public override void AI(){
      npc.velocity = new Vector2(0, -5);//subtract from Y to go up
      //AI is called every frame.  Anything you put here will happen 60 times a second
}
```
If you test this code you should see your npc fly upwards.  You probably want your npc to do something more complex though, and to do that we need to keep track of time, and find a player to target. 
Timers are covered in this guide https://github.com/tModLoader/tModLoader/wiki/Time-and-Timers, which you should read if you haven't already. Lets add a timer like this guide shows 
```cs
int Time = 0;
public override void AI(){
    Time++;
    //npc.velocity is the change in pixels of the npc each frame(60 times a second)
    npc.velocity = new Vector2(0, -5);//subtract from Y to go up
    //AI is called every frame.  Anything you put here will happen 60 times a second
}
```
Now that we have a variable to keep track of time we can make things happen on a timer with %. % computes the remainder after dividing its left operand by its right operand, and is useful for making things happen every x ticks like this
```cs
int Time;
public override void AI(){
            Time++;
            npc.velocity = new Vector2(0, -5);//subtract from Y to go up
            if (Time % 120 == 0)
            {
                //things here happen once every 120 ticks, or 2 seconds
            }
            //AI is called every frame.  Anything you put here will happen 60 times a second
        }
```
Now, we need to do something within our if statement.   One of the most common things people want bosses to do is shoot a projectile, which is simple - call the NewProjectile method Like this `Projectile.NewProjectile(Parameters);`
Parameters will be replaced with the info about the spawned projectile, in this order 
```cs
NewProjectile(float X, float Y, float SpeedX, float SpeedY, int Type, int Damage, float KnockBack, int Owner = 255, float ai0 = 0f, float ai1 = 0f);
```
Note that there is a different overload with Vector2s instead of floats for x/y and Speedx/SpeedY.  For consistency I will not be using it, but it is effectively the same.
Float X is a float of the X coordinate to spawn at, Y is a float of the Y coordinate to spawn at, SpeedX and SpeedY are both floats of the x and y velocity respectively to spawn with.  Type is the type of projectile - is it a wooden arrow? Your modded projectile? This will either be a vanilla projectile ID, like ProjectileID.WoodenArrowHostile or a modded one, like ModContent.ProjectileType<MyClass>(), Damage is self explanatory, remember that setting projectile.Damage in the projectile doesn't do anything, it has to be set here; Knockback is self explanatory, but notice it's a float, so it can take decimals.
Owner, ai0, and ai1 are all optional parameters(see how they have = something after, which shows their default value). 
Seems like a lot right, but thankfully it's much simpler to write
This code fires a wooden arrow projectile straight up from the npcs center 
```cs
Projectile.NewProjectile(npc.Center.X, npc.Center.Y, 0, -50, ProjectileID.WoodenArrowHostile, 50, 0f);
```
Keep in mind that the wooden arrow AI slows and eventually reverses velocity 
As you can see each parameter is filled out with the information about how to create the projectile.  If we add this into our AI we can get are first "Attack"(I also removed the velocity change so that you can better see how the attack will loop endlessly)
```cs     
   int Time;
   public override void AI()
        {
            Time++;
            if (Time % 120 == 0)
            {
                //things here happen once every 120 ticks, or 2 seconds
                Projectile.NewProjectile(npc.Center.X, npc.Center.Y, 0, -5, ProjectileID.WoodenArrowHostile, 50, 0f);
            }
            //AI is called every frame.  Anything you put here will happen 60 times a second
        }
```
This code will work fine in single player, but in multiplayer this will cause extra projectiles to spawn.  This is because AI is run on every client, but Projectile.NewProjectile syncs the spawned projectile - every client and the server will spawn it and then sync it to the other clients, causing extra to spawn.  Solving this is simple, we just need to only spawn the projectile on the server, and let NewProjectile sync it on all clients by adding if(Main.netMode != NetmodeID.MultiplayerClient) before spawning a projectile(or a npc).  We can add in an extra condition to our if with && and only have to use one if
```cs
 if (Time % 120 == 0 && Main.netMode != NetmodeID.MultiplayerClient)// if we arent a multiplayer client and it's been 120 ticks
```
### Finding a Target
Now that we know how to shoot projectiles, let's make this attack harder to dodge by firing it at the player.  To do that we need to find the players we want to target.  For finding the closest near a npc, this is very easy first call npc.TargetClosest(true/false),with true/false being if you want it the npc to change the sprite direction to face the target.  We can then get the player with Main.Player[npc.Target] and store it in a player variable. 
```cs
npc.TargetClosest(false);
Player target = Main.player[npc.target];
//use target
```
 If we didn't want the closest but instead wanted some other condition checked, we could loop the Main.Player array to find a valid target. 
```cs
npc.TargetClosest(false);
Player p = Main.player[npc.target];//first get the closest just in case all of our checks fail
for (int i = 0; i < Main.maxPlayers; i++)
{
      if(Main.player[i].active)//add in other checks with &
           p = Main.player[i];
}
```
With our player variable we can do tons of new things.  One common thing people do is to fire a projectile or make the boss move towards the player.  To do this first we get the direction to the player 
```cs
public override void AI()
{
npc.TargetClosest(false);//first we setnpc.target to the closest person
Player target = Main.player[npc.target]; //then we get the player from the array and store it in a variable.
Vector2 ToPlayer = npc.DirectionTo(target.Center);//then we make a vector2 of the direction to the player's center.  
//use ToPlayer
//If you want whatever you use ToPlayer for to go faster, multiply it by something like npc.Velocity = ToPlayer * 5f
        }
```
Shooting a projectile would look like this  
```cs
int Time;
public override void AI()
{
Time++;
     if (Time % 120 == 0 && Main.netMode != NetmodeID.MultiplayerClient)//if its been 120 ticks and we arent a client
            {
                npc.TargetClosest(false);//set npc.traget to the nearest player
                Player target = Main.player[npc.target];//get the player we just targeted 
                Vector2 ToPlayer = npc.DirectionTo(target.Center) * 5;//get the direction to the player. 5 is the speed 
                Projectile.NewProjectile(npc.Center.X, npc.Center.Y, ToPlayer.X, ToPlayer.Y, ProjectileID.WoodenArrowHostile, 50, 0f);//launch a projectile using the direction to the player we just got
            }
            //AI is called every frame.  Anything you put here will happen 60 times a second
        }
    }
```
Hopefully you understand what everything there does - the Time variable keeps track of the number of ticks the boss has been alive.  if(Time %120 ==0) checks if that variable is a multiple of 120, which will be every 120 ticks or 2 seconds.  npc.TargetClosest(false) and Player target = Main.Player[npc.target]; get the closest players and store it in a Player variable called target.  Vector2 ToPlayer = npc.DirectionTo(target.Center); then stores the velocity we want to fire our projectile with in a Vector 2.  Finally we spawn a projectile. If we wanted to instead fly to a location we would just set npc.velocity to a direction Vector2 like so:
```cs
npc.TargetClosest(false);
Player target = Main.player[npc.target];
Vector2 ToPlayer = npc.DirectionTo(target.Center) * 3;
npc.velocity = ToPlayer; 
```
### Making Patterns
Most of the time we don't want our npc to do just one thing, instead we want to have more then "attack" it can be doing.  To do this you need to have a variable holding what attack is happening.  In order to make this code cleaner, I separated each attack into its own method and used constants to represent the "ID" of each phase.
```cs
//Constant variables used to make code readable
const int ProjectileState = 0;//Instead of checking if state is 0, we can check it is ProjectileSate.  In application they mean the same thing, but the former is much nicer to look at.
const int ChaseState = 1;
const int CircleProjectileState = 2;
int Timer;
int state;
public override void AI()
        {
            //since all states Increase Timer and use npc.Target, we can put the code all of them would have the same here
            Timer++;
            npc.TargetClosest(false);
            if (state == ProjectileState)
            {
                ProjectileAttack();//run the attack method
                if (Timer == 180)
                {//if it has been 3 seconds,
                    Timer = 0;//reset timer
                    state = ChaseState;//go to a another state
                    /*What if I want random attacks, not a set order? 
                    use Main.rand.Next(bottom, top +1) to get a random number
                    state = Main.rand.Next(0,3);// notice how we put the second value as 3 even through the highest attack state is 2 - the second  number in Next can't be chosen, it's one above the highest that can. 
                       Also keep in mind that if I choose a random state, I will have to sync it to other clients.  See the basic netcode guide or keep reading.
                     */
                }
            }
            else if (state == ChaseState)
            {
                Chase();
                if (Timer == 240)
                {//if it has been 4 seconds
                    Timer = 0;//reset timer
                    npc.velocity = Vector2.Zero;//since we change the velocity in this state, we need to reset it before moving to a new stage.
                    state = CircleProjectileState;
                    //state = Main.rand.Next(0,3); - random state
                }
            }
            else if (state == CircleProjectileState)
            {
                CircleProjectile();
                if (Timer == 180)
                {//if it has been 3 seconds
                    Timer = 0;//reset timer
                    state = ChaseState;//go to a another state
                                       //state = Main.rand.Next(0,3); - random state
                }
            }
        }
        private void ProjectileAttack()
        {
            if (Timer % 20 == 0 && Main.netMode != NetmodeID.MultiplayerClient)
            {
                Player target = Main.player[npc.target];
                Vector2 ToPlayer = npc.DirectionTo(target.Center) * 3;//change 3 to change speed
                Projectile.NewProjectile(npc.Center.X, npc.Center.Y, ToPlayer.X, ToPlayer.Y, ProjectileID.WoodenArrowHostile, 50, 0f);//What if I want a modded projectile? Replace the type parameter(ProjectileID.WoodenArrow) with ModContent.ProjectileType<MyClass>() and make sure to be using the  namespace of it
            }

        }
        private void Chase()
        {
            Player target = Main.player[npc.target];
            Vector2 ToPlayer = npc.DirectionTo(target.Center) * 3;//change 3 to change speed;
            npc.velocity = ToPlayer;
        }
        private void CircleProjectile()
        {
            if (Timer % 60 == 0 && Main.netMode != NetmodeID.MultiplayerClient)
            {
                for (int i = 0; i < 360; i += 12)
                {//loop till i = 360, for one full rotation. You can change the interval of rotation by changing the 12 
                    Player target = Main.player[npc.target];
                    Vector2 ToPlayer = npc.DirectionTo(target.Center).RotatedBy(MathHelper.ToRadians(i));//By default, rotation uses radians.  I prefer to use degrees, and therefore I convert the radians. 
                    if (Main.netMode != NetmodeID.MultiplayerClient)
                        Projectile.NewProjectile(npc.Center.X, npc.Center.Y, ToPlayer.X * 5, ToPlayer.Y * 5, ProjectileID.WoodenArrowHostile, 50, 0f);//fire the projectile
                }
            }

        }
```
Theres one last issue with this, if you used random attacks.  If we are in multiplayer, then each client may end up with a different attack, causing the npcs to desync.  
To avoid this we need to ensure the state variable is the same on every client.  There's 2 ways to do this -  one is to hold them in one of the npc arrays, which is automatically synced, the other is to use Send and Receive extra ai hooks to sync them.  See https://github.com/tModLoader/tModLoader/wiki/Basic-Netcode

### Using WhoAmI and the Main.  Arrays
Sometimes we want to spawn a npc and have it do something that requires finding the target, position or some other value from the npc that spawned it, or requires finding out if the spawned npc is alive/what it's hp is or some other property of a npc that isnt us.  To do this, you first need to know what a "WhoAmI" is.  Every npc, projectile, and player that is active(and some that aren't)are held in their arrays(Main.npc, Main.projectile, and Main.player respectively).    The place in the array of something is it's "WhoAmI" or index.  Why does this matter? Because if you have the WhoAmI, you can then get the thing itself with Something x = Main.something[whoami]; If this looks familiar, that's because we have already used this to get our target in the example above.  One other important thing is that NewNPC and NewProjectile both return the place in the array of the spawned projectile, allowing you to access it easily like this. 
```cs
int Index = Projectile.NewProjectile(parameters);//you will obviously need to fill in the parameters
Projectile p= Main.projectile[Index];//this can be applied to npcs too - use NPC n = Main.npc[Index]; after calling NewNPC
// you can now use it's fields/methods
//for example p.Center or p.Kill()
```
But what if I want my spawned projectile(or npc) to have the WhoAmI of the "parent" that spawned it? This is when we use those optional parameters of the spawning methods.   If you look at either NewNPC/Projectiles parameters, you should see floats like ai0, ai1 etc.  These are used to pass info to the spawned thing.  Let's say I set ai0 to 15 -  then in the projectile or npc, npc/projectile.ai[0] will be 15.  And, more importantly if I set it to the WhoAmI of the parent, I can 
then get the parent with npc/projectile.ai[0].
```cs
//In the Parent
Projectile.NewProjectile(npc.Center.X, npc.Center.Y, ToPlayer.X * 5, ToPlayer.Y * 5, ProjectileID.WoodenArrow, 50,0f,255, npc.WhoAmI);

// In the spawned Projectile
Something s = Main.Something([(int)projectile.ai[0]];//not how I put (int) here - since the ai parameters are floats and can be decimals, I have to cast them to a whole number to access the array(Note this does not round, it just removes everything past the decimal point.)
```
One final thing to note is that you can pass npc.Target as an ai parameter and get the target player with Main.player.
### Making Phases
You can make your NPC choose different attacks based on certain factors, and make your code overall more organized by making your phase choosing code in it's own method, then call instead of setting state in the attack.
Let's look one phase from before, but this time make the state be set to a new phase we added if hp is below 50 by checking npc.Life and LifeMax before setting it
```cs

if (npc.life < (npc.lifeMax / 2){//if below 50, set it to something else, like an stage 2 attack
     State = Below50Attack;//Random? Main.rand.Next(LowestStageOneAttack , Highest); 
}
else
{
     state = ChaseState;//go to a another state
     //state = Main.rand.Next(0,3); - random state
}
```
Thing is, if we want it to transition after every attack, we would have to put this code in every single state in the first phase, which is a lot of unnecessary code and causes other issues.
Instead we can make a method to change the phase
```cs
  public int ChoosePhase()
{
     if (npc.Life < (npc.LifeMax / 2))
       {
            return Main.rand.Next(4, 7);//if we are below half our hp, return 4-6.  Then in ai we will check what state is set to 
       }
       /* else if(somecondtion){//add more like this
            return somethingelse;
       }*/
            return Main.rand.Next(0, 3);//since we will have already returned if below 50, we don't need to do this in an else.  
       }
       //then in our each of our states 
       if (timer == phaselength)
        {
            //replace phaselength with the amount of ticks to attack for before swapping
            State = ChoosePhase();
        }
```
This way, we avoid having to put the same code over and over, and if we want to edit this code later(like adding another state or removing one) we only need to change it in one spot. 
### Making a NPC Despawn
To despawn a npc, you just need to set npc.active to false.  Of course, you only want your npc to despawn if nobody is around to fight.  To do this we need to check if a valid target exists. 
First we check if the current target is dead or if something else would cause us to want to despawn
```cs
if (!player.active || player.dead)
{
     //if the player is dead or not "active"(meaning the player is no longer playing)
     npc.TargetClosest(false);//try to find a new target
     player = Main.player[npc.target];
          if (!player.active || player.dead) // if the new one is also dead, then we know there is no active player, and can despawn 
          {
          npc.active = false;//set ourselves to inactive, this makes it so we don't drop loot
          }
}
```
Projectile despawning happens automatically after TimeLeft runs out(which you should set in setdefaults).  If you want to manually get rid of it call projectile.Kill(); If you don't want the projectile to despawn after a certain amount of time, set projectile.TimeLeft to be greater then 1 in ai.  Since ai is called every tick, it will always never drop to 0 and the projectile will never despawn.
### Common behaviors
**Spawning a NPC**
Spawning another npc in a boss is a lot like a projectile but instead you use NPC.NewNPC
It's parameters are NewNPC(int X, int Y, int Type, int Start = 0, float ai0 = 0f, float ai1 = 0f, float ai2 = 0f, float ai3 = 0f, int Target = 255)
Remember all the ones with a = after are optional, and you only need to put a value for if you know what you're doing. X and Y are self explanatory, and Type is just like in NewProjectile - vanilla npcs would be NPCID.Name and modded is Modcontent.NPCType<Class>().
**Fire a burst of something**
You can make code execute more then once in ai with a for loop(read about them here https://www.tutorialspoint.com/csharp/csharp_for_loop.htm). If you want to make your projectiles inaccurate or not all fired towards one spot read the "Changing Projectiles Direction" section below, making use of the loop variable. 
**Changing Projectiles Direction**
To change where the projectile is fired you need to affect the Vector2 ToPlayer(assuming you're still using the code from above). There's a number of handy methods that you can use on a Vector2 to change it's contents.  One of the most useful is RotatedBy(double Radians) or RoatatedByRandom. This rotates the Vector 2s contents, making it usefully for shooting ahead of a target or making a spread/shotgun pattern.  Keep in mind it takes Radians not degrees so make sure to use MathHelper.ToRadians if you have a degree measurement. .
Example usage - shooting a circle of thing
 ```cs
                for (int i = 0; i < 360; i += 12)
                {//loop till i = 360, for one full rotation. You can change the interval of rotation by changing the 12 
                    Player target = Main.player[npc.target];
                    Vector2 ToPlayer = npc.DirectionTo(target.Center).RotatedBy(MathHelper.ToRadians(i));//By default, rotation uses radians.  I prefer to use degrees, and therefore I convert the radians. 
                    if (Main.netMode != NetmodeID.MultiplayerClient)
                        Projectile.NewProjectile(npc.Center.X, npc.Center.Y, ToPlayer.X * 5, ToPlayer.Y * 5, ProjectileID.WoodenArrowHostile, 50, 0f);//fire the projectile
                }
```
**How Do I Spawn My NPC Like Vanilla Bosses**
Spawning your boss is simple, just call NPC.SpawnOnPlayer(player.WhoAmI, ModContent.NPCType<Class>()); 
**How Do I Make my NPC/Projectile Give a Debuff**
To do this we need to override OnHitPlayer(Player player, int damage, bool crit) for npcs or ModifyHitPlayer(Player target, ref int damage, ref bool crit) for projectiles and call player(npcs) or target(projectiles).AddBuff(int type, int duration) 
Keep in mind that duration is in ticks. 60 = one second
```cs
        public override void OnHitPlayer(Player player, int damage, bool crit)
        {//note this is a differently named hook in projectiles
         	player.AddBuff(BuffID.Cursed, 240);//240 = 4 seconds. 
         //you can change the buff by using a buff ID.  Modded buffs should use ModContent.BuffType<Class>()
        }
```
**How do I Make Something Talk**
To print a message in chat, put Main.NewText("Text", New Color(r,g,b)).  R, G, and B are  numbers 0-255 of the message's color. 

**How do I Give my npc Drops**
Read https://github.com/tModLoader/tModLoader/wiki/Basic-NPC-Drops-and-Loot  
**How Do I Make a Worm Boss**
Worm bosses take significantly more code then this guide covers.  Example mod shows an example of what worm code would look like here: 
1.4:https://github.com/tModLoader/tModLoader/blob/dd44e70738e29e5ded0cb979355b671b41f56efe/ExampleMod/Content/NPCs/Worm.cs
1.3: https://github.com/tModLoader/tModLoader/blob/1.3/ExampleMod/NPCs/Worm.cs
### Common Errors
Remember google is your friend when trying to find how to ix an error. 
**The Type or Namespace something Cannot be Found**
Keep in mind c# is case sensitive - Projectile is not the same as projectile. 
If "Something" is not from your mod
This probably means you forgot an using.  If you're on vs, it should automatically recommend them.  If you're not, try looking around if it's in example mod and adding the usings it has.  Adding
```cs
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;
using System;
```
Should cover most of them.  If it doesn't and you can't find it make sure you haven't misspelled anything. 
If "Something" is from your mod, make sure it's spelled correctly.  If it is, go to it's file and make sure you're using it's namespace. 

**Cannot implicitly convert type 'float' to 'int'. An explicit conversion exists (are you missing a cast?)**
This means you tried to use a float(variable that can hold decimals) for something that uses an int(whole number).  You can solve this easily by "casting it" by adding (int) before, but keep in mind this will cause everything right of the decimal point to be removed.  You can get a rounded number with
(int)Math.Round(MyFloat);

If you cant find your issue here, first google it, if you still are stuck ask for help in tmods discord server.

