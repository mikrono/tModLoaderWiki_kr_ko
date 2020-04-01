# Introductory
Terraria is frequently played with many players on a server. Ensuring that your mods work well with friends is something that should not be an afterthought. Test early and often in multiplayer as you develop your mod. In this guide, we will go over how to make your mods multiplayer compatible, and hopefully show you it is not as difficult as it may seem.

# Main Concepts
The following are main concepts key to understanding how Terraria works in multiplayer.
1. When you play Terraria, your application is considered and called a "Client"
1. During multiplayer, your client and other players' clients connect to a "Server"
1. While connected to this server, the connected clients need to communicate well together through the server, so that things happening in the game are synchronized between these clients.

In essence: network packets are sent across clients to make sure the game is in sync for all players.
This happens all the time in multiplayer play, even in vanilla.

# Packet flows
There are a couple of flows that can happen for network packets:

**Flow 1**: Client -> Server -> Client <br>
This flow is very common. A client sends a packet to the server, after which the server forwards this packet to all other connected clients (or specified ones).

**Flow 2**: Client -> Server <br>
This flow is also very common and is used when a client needs to instruct the server of a change.
An example could be running a (chat) command that alters server behavior, so something that the server controls.

**Flow 3**: Server -> Client <br>
This flow is less common than the other 2 flows and is used to have the server instruct the client about something.
It is common for this flow to occur in games when a client is out of sync and the server sends it information to come back in sync.

# ModPacket
### Making and sending a packet
If you want to send packets in your mod, you will use the ModPacket class. This class is designed to send data in any of the designated flows described above. A very simple packet could look like this:
```cs
ModPacket myPacket = myMod.GetPacket();
myPacket.write("Hello world!");
```
Now you have written a packet with the string data "Hello World!". In order to send the packet to the server, call the `Send` function on ModPacket:
```cs
myPacket.Send();
```

You may already have realized we are now in flow 2: we are sending a packet to the server. It is now up to the server to do something with this packet.

### Receiving packets
For receiving and reading packets, you'll have to override the `HandlePackets` method in your Mod class:
```cs
public override void HandlePacket(BinaryReader reader, int whoAmI) {
    reader.readString(); // "Hello world!"
}
```
`HandlePacket` is called whenever a packet is received from a client (if this is a server) or the server (if this is a client). whoAmI is the ID of whomever sent the packet (equivalent to the Main.myPlayer of the sender), and reader is used to read the binary data of the packet. **So in order to handle packets sent by a client on the server you must make sure your mod is also running on the server.**

You'll obviously want to write this method more elegantly so you can handle different kinds of packets. It's very common to write an identifier for your packet so you can recognize what the packet is about. You can then read this type and execute the corresponding reading logic:
```cs
ModPacket myPacket = myMod.GetPacket();
myPacket.Write((byte)0); // id
myPacket.Write("Hello world!"); // message
myPacket.Send();
```
```cs
		public override void HandlePacket(BinaryReader reader, int whoAmI) {
			byte msgType = reader.ReadByte();
			switch (msgType) {
				case 0:
					string msg = reader.ReadString(); // "Hello world!"
					break;
				case 1:
					... // another message type goes here
					break;
				default:
					Logger.WarnFormat("MyMod: Unknown Message type: {0}", msgType);
					break;
			}
		}
	}
```

### Reading a packet
Reading is just as writing, you'll have to call any of the appropriate read functions from the BinaryReader object. Very common are: `ReadString()`, `ReadInt32()`, `ReadByte()` and `ReadSingle()` (reading a float)

### Writing and reading in the correct order
It is very important that you remember that packets are **FIFO**: First in = First out.
**You must read the data in the order it was sent in**.
For example, let's consider a packet where you send a byte, and integer and then a float. We'll be using some arbitrary numbers alongside an id for the packet. You'll write and send the packet as follows:
```cs
ModPacket packet = mod.GetPacket();
packet.Write((byte)0); // id
packet.Write(100); // progress
packet.Write(1.0f); // difficulty
packet.Send();
```
You must read the packet in the same order on the receiving end, so:
```cs
byte id = reader.ReadByte();
int progress = reader.ReadInt32();
float difficulty = reader.ReadSingle();
```

# Networking optimization
Bad networking code can cause your mod to perform poorly in multiplayer play. Let's go over some concepts to ensure that doesn't happen.

**Under-reading or over-writing** <br>
In general, you must remember to **only send data you will actually use**. If you send more data than you are reading, this is considered under-reading or over-writing. Essentially you are bloating the network with redundant data, data that you aren't using. **Always make sure you are actually using all the data you are sending**.

**Common data you usually don't have to send are calculated values, or instead parameters for a calculation**.
Consider that you have calculated a number on one client, and this number needs to go another client. You could choose to send the non-deterministic parameters which you used to calculate the number, or you could choose to send the calculated number. For example, if we take simple multiplication for integers the latter choice would be best because we're sending just one integer vs two integers. In some cases however, you may end up with multiple results from these two parameters, in this case it may be better to send the parameters instead and calculate the values again on the other client.

**Sending packets at the right moment** <br>
To not overflow the network with ridiculous amounts of packets you need to think about when it is the right time to send a packet. For example consider a packet notifying the death of a boss. You could choose to send a packet every frame with the health of the boss and check this value on the receiving end. However, the game updates at an interval that will cause you to send dozens of packets every second! You can imagine that packets holding a lot of data this way will bloat the networking and cause the game to perform poorly.

**Make sure you send a packet where it is supposed to be sent, in this case on death for the boss.** ModNPC has a hook for this.

**In a lot of cases, you do not need to send packets at all; simply use one of the TML hooks.** In the given example we could also just check if the running instance is a server in the death hook for the boss, and then run whatever logic we need. Usually there's a hook already available you can use for your logic. Our [intermediate netcode guide](intermediate-netcode) goes more in-depth on this matter.
 
But now you might wonder, **when to send a packet then**? Usually a good time to send a packet is with non-deterministic behavior. Consider the boss again in a multiplayer game. You may have programmed the AI to semi-randomly switch phases of the boss. In a multiplayer scenario this random switch should occur on the server, and at that point the server should send a packet to all clients informing them of the phase change. This way all clients will stay in sync because they are informed by the server. If the clients were to decide for themselves when to switch phases each client would turn out of sync with each other because they would switch at different moments.

# Improving
Many examples can be found in ExampleMod for packets. For example you can use the Notepad++ "Find in files" feature and look for `GetPacket()` in the ExampleMod folder. Make sure 'Match case' and 'In all sub-folders' are checked. You'll find all locations in which a packet is created and likely sent.

If you feel you want to delve even deeper and optimize your networking more, read our [intermediate netcode guide](intermediate-netcode).