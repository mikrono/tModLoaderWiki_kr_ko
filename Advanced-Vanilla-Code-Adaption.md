# Vanilla Code Adaption
This guide teaches how to navigate the jungle that is Terraria source code to find or adapt code. Why? Maybe you wish to customize an aiStyle, maybe you want to know how Magma Stone works, or maybe you want to mimic a vanilla effect.

## Obtaining Vanilla Source Code
It is against the law to publish the decompiled source code, but anyone with a competent computer can decompile their personal copy of Terraria.exe no problem. Note that you may wish to end up with a decompiled tModLoader rather than a decompiled vanilla Terraria so that you know where tModLoader hooks are. See [Advanced Prerequisites](https://github.com/blushiemagic/tModLoader/wiki/Advanced-Prerequisites) for more info.

## Example: Projectile or NPC AI code
This guide teaches how to adapt vanilla AI code for new uses. In Beginner level coding, we sometimes use vanilla aiStyles for our ModProjectiles or ModNPC. In Intermediate level coding, we take that a step further by setting aiType to mimic the specifics of an aiStyle or even use PostAI or PreAI to add additional code to vanilla aiStyles for our particular thing. This is not always enough, so in this tutorial, we will learn how to investigate vanilla source code to find the information or code snippets we need to completely customize the things in our Mod. 

### Custom Flail
Making a flail, you might have noticed that the range of the flail is hard to customize. (TODO)