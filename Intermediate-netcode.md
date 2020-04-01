# Introductory
Before you continue this guide, you should read our [basic netcode](basic-netcode) guide first.

# Practical advice
In general, you'll want to make your packets small so they can be sent across clients quickly and do not cause any problems in the network. Big packets or many packets sent will cause the network to clog up, and can cause lag or problems while playing the game. If a mod performs bad during multiplayer, this can be why. If a mod is actually buggy during multiplayer, it is often because they do not send packets at all and clients aren't synchronized.

To help aid making packets small and minimizing the amount you need to send, you should try to make code deterministic whenever you can. Determinism means that code on clients align, and do not need further interaction between them to be in the same state. This means no networking between them needs to be done to ensure everyone gets the same result on their client.

Ensuring determinism isn't always easy, but can easily be checked against when answering the following question:
> "Will multiple clients be in the same state (or have the same result) when this code is ran for them individually, without any interaction between them?"

If the answer is yes, that means your code is deterministic enough. If all clients can reach this point at the same time, no more networking is needed to ensure the same result.

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

# Modelling Send and Receive for ModPacket
In general, it is easy to model how you should write and read data from a packet. **You should read the data, in the same order it was sent**. Consider the following scenario:
```cs
float someVal;
int someInt;

public void Send(int toWho, int fromWho)
{
	ModPacket packet = mod.GetPacket();
	if (Main.netMode == NetmodeID.Server) {
		packet.Write(fromWho)
	}
	packet.Write(someVal);
	packet.Write(someInt);
	packet.Send(toWho, fromWho);
}

public void Receive(BinaryReader reader, int fromWho)
{
	if (Main.netMode == NetmodeID.MultiplayerClient) {
		fromWho = reader.ReadInt32();
	}
	someVal = reader.ReadSingle();
	someInt = reader.ReadInt32();
	if (Main.netMode == NetmodeID.Server) {
		Send(-1, fromWho);
	} else {
		MyModPlayer myModPlayer = Main.player[fromWho].GetModPlayer<MyModPlayer>();
		myModPlayer.someVal = someVal;
		myModPlayer.someInt = someInt;
	}
}
```

In the above example we're showcasing how you can model/design your send and receive methods to handle all involved parties: both connected clients and the server. 

In the Send() method, we're sending a packet that contains a float and int value, to some specific target and ignoring the passed fromWho client. In the Receive() method, we're reading the packet data, and then doing something based on if we're a server or client. You'll see that we read the fromWho data dependent on the Main.netMode. This is because this part of the packet only exists when the packet was sent by the server, so we can only read it if we're a client. You can do this to access the client that originally sent the packet, instead of 256 as fromWho which signifies the server. 

If you compare Send() and Receive() closely, you'll see that the order of reading and writing aligns. At the end of the Receive() method, we call Send() again and pass -1 as toWho which signifies every connected client, except the fromWho client. This makes the server forward the packet to other clients. Conditionally if we're not a server (so we're a connected client), we can set the newly read values in some place where we actually need them.

# Good practice managing many packets
If you work on a big mod, you might find yourself sending many packets which can become a hassle to manage without a proper setup. Recommended is to use a PacketHandler class that can make it easier to handle different packets. Feel free to use this class:
```cs
	internal abstract class PacketHandler
	{
		internal byte HandlerType { get; set; }
		
		public abstract void HandlePacket(BinaryReader reader, int fromWho);

		protected PacketHandler(byte handlerType)
		{
			HandlerType = handlerType;
		}

		protected ModPacket GetPacket(byte packetType, int fromWho)
		{
			var p = MyMod.Instance.GetPacket();
			p.Write(HandlerType);
			p.Write(packetType);
			if (Main.netMode == NetmodeID.Server)
			{
				p.Write((byte)fromWho);
			}
			return p;
		}
	}
```
This class takes care of some stuff for you, including writing the handler type and packet type. This will be useful when receiving packets, knowing where to forward to.

You can inherit this base class, for example when making a handler for "MyBossNPC":
```cs
	internal class MyBossNPCPacketHandler : PacketHandler
	{
		public MyBossNPCPacketHandler(byte handlerType) : base(handlerType)
		{
		}

		public override void HandlePacket(BinaryReader reader, int fromWho)
		{
			throw new NotImplementedException();
		}
	}
```
If we go by the example earlier, you might want to sync the target of this boss. You can modify the class to reflect this. You'll need to make HandlePacket switch by the packet type, and call the appropriate handling method for that packet:
```cs
	internal class MyBossNPCPacketHandler : PacketHandler
	{
		public const byte SyncTarget = 1;

		public MyBossNPCPacketHandler(byte handlerType) : base(handlerType)
		{
		}

		public override void HandlePacket(BinaryReader reader, int fromWho)
		{
			switch (reader.ReadByte())
			{
				case (SyncTarget):
					ReceiveTarget(reader, fromWho);
					break;
			}
		}

		public void SendTarget(int toWho, int fromWho, int npc, int target)
		{
			ModPacket packet = GetPacket(SyncTarget, fromWho);
			packet.Write(npc);
			packet.Write(target);
			packet.Send(toWho, fromWho);
		}

		public void ReceiveTarget(BinaryReader reader, int fromWho)
		{
			int npc = reader.ReadInt32();
			int target = reader.ReadInt32();
			if (Main.netMode == NetmodeID.Server) {
				SendTarget(-1, fromWho, npc, target);
			}
			else {
				NPC theNpc = Main.npc[npc];
				theNpc.oldTarget = theNpc.target;
				theNpc.target = target;
			}
		}
	}
```

The last thing you'll need to do, is actually call the appropriate PacketHandler class when your mod receives the packet. You can make a global class that handles this for you. In your mod class:
```cs
public override void HandlePacket(BinaryReader reader, int whoAmI) 
{
	ModNetHandler.HandlePacket(reader, whoAmI);
}
```

In your new ModNetHandler class:
```cs
internal class ModNetHandler 
{
	// When a lot of handlers are added, it might be wise to automate
	// creation of them
	public const byte MyBossNpcType = 1;
	internal static MyBossNPCPacketHandler myBossNpc = new MyBossNPCPacketHandler (MyBossNpcType);
	public static void HandlePacket(BinaryReader r, int fromWho)
	{
		switch (r.ReadByte())
		{
			case MyBossNpcType:
				myBossNpc.HandlePacket(r, fromWho);
				break;
		}
	}
}
```

With this setup, you can keep creating new PacketHandler classes to handle certain subjects in a more global way.


