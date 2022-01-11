___
Looking to learn how to write and send your own ModPackets? Read our [intermediate netcode guide](intermediate-netcode).
___

# Introductory
Terraria is frequently played with many players on a server. Ensuring that your mods work well with friends is something that should not be an afterthought. Test early and often in multiplayer as you develop your mod. In this guide, we will go over how to make your mods multiplayer compatible, and hopefully show you it is not as difficult as it may seem.

# Main Concepts
The following are main concepts key to understanding how Terraria works in multiplayer.
1. When you play Terraria, your application is considered and called a "Client"
1. During multiplayer, your client and other players' clients connect to a "Server"
1. While connected to this server, the connected clients need to communicate well together through the server, so that things happening in the game are synchronized between these clients.

In essence: network packets are sent across clients to make sure the game is in sync for all players.
This happens all the time in multiplayer play, even in vanilla. **Clients CANNOT send or receive things DIRECTLY from other clients - the server has to act as a middleman.**

# Automatic Syncing (non-ModPacket)
Terraria handles many network syncing things for us, we just have to use them right.

## NPC / ModNPC
With NPC, any non-deterministic decision must be synced to the clients. The Server is the owner of all NPC. As seen in [FlutterSlime.cs](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/NPCs/FlutterSlime.cs#L146), we use npc.netUpdate to trigger the NPC syncing code. Terraria will sync position, life, and other data from the server to the clients whenever `npc.netUpdate` has been set to true. We can use [ModNPC.SendExtraAI](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_n_p_c.html#ad091edf8203519c61a7f7c903880dd60) and [ModNPC.ReceiveExtraAI](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_n_p_c.html#a3ff34ad5598cdf5e664eaa32d687d1c0) to sync extra data needed by all clients rather than `ModPacket`. This extra data will always be synced whenever the npc itself is synced.

## Projectile / ModProjectile
Projectiles operate the same way as NPC, with the exception that the owner of a projectile is not always the Server. Projectiles spawned by players are owned by that player/client, while all other projectiles such as NPC spawned or World spawned projectiles are owned by the server. In the case of client changes, the `projectile.netUpdate` flag will sync the client's data to the server and the server will relay that to the other clients. An example is [Magic Missile](https://terraria.gamepedia.com/Magic_Missile). The AI will check `if (Main.myPlayer == projectile.owner)` and then set `projectile.netUpdate = true;` if the position has changed due to the client players mouse position.

## World / ModWorld
World data should only ever be changed on the server. Whenever something important happens in the world, such as the Noon command on the server console, a Boss being defeated, or a random invasion triggering, World data is synced from the server to the clients. Whenever world data is synced, each ModWorld is also synced via [NetSend](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_world.html#a58fe0616b0c4135ec038025e67268561) and [NetReceive](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_world.html#aaf20d147cc364d10b4b137999b5c26b5). We can trigger a network sync of world data by calling `NetMessage.SendData(MessageID.WorldData);` on the server, as seen in [Abomination](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/NPCs/Abomination/CaptiveElement2.cs#L367) when we set `ExampleWorld.downedAbomination = true;`. Try to only do this when necessary.

## Item / ModItem / GlobalItem
Items are also synced. Use [`NetSend`](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_item.html#a5f7a03b9388a7610935e6f628b257dfe) and [`NetRecieve`](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_item.html#a4e4112cadf7f23f51b913cda238c8708) to sync data. Follow decompiled source code if you need to do anything unorthodox, most mods won't need to do anything such as triggering manual syncing. When Items are synced, only a very select subset of the fields are synced, this is why changing default values such as `item.damage` in something like `UpdateInventory` is not recommended. Use [`GetWeaponsDamage`](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_item.html#af2170bc3ec0925114fa9488767bf2ceb) for that.

## ModTileEntity
Owned by the server. Only sent to client when the client first visits the "chunk"/"section" of the world where it resides. Changes are applied on the server and synced from the server to client. See [TEScoreBoard](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Tiles/TEScoreBoard.cs) for an example.

## Player / ModPlayer
Player data is very large, so special data syncing is used to minimize how often and which Player data must be synced. See `CustomBiomesMatch`, `CopyCustomBiomesTo`, `SendCustomBiomes`, `ReceiveCustomBiomes`, `clientClone`, `SyncPlayer`, and `SendClientChanges` in the [ModPlayer Documentation](http://tmodloader.github.io/tModLoader/html/class_terraria_1_1_mod_loader_1_1_mod_player.html) and [ExamplePlayer](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/ExamplePlayer.cs) to learn their purpose and see them in use. Vanilla `Player` data such as inventory slots, health, position, and selected item are all synced automatically, so changing those in code will automatically sync to the server and be relayed to the other clients. Failure to sync Biomes will cripple NPC spawning behavoir in Multiplayer. Failure to use SyncPlayer and SendClientChanges will lead to desync and many other problems, they are very important to get right. 

# Improving & beyond pre-defined hooks
Many examples can be found in ExampleMod for packets. For example you can use the Notepad++ "Find in files" feature and look for `GetPacket()` in the ExampleMod folder. Make sure 'Match case' and 'In all sub-folders' are checked. You'll find all locations in which a packet is created and likely sent.

If you feel you want to delve even deeper to learn how to send ModPakcets and optimize your networking more, read our [intermediate netcode guide](intermediate-netcode).