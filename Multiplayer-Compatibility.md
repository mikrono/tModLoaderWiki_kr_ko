# Multiplayer Compatibility
Terraria is frequently played in with multiple players. Ensuring that your code is multiplayer compatible is something that should not be an afterthought. Test early and often in multiplayer as you develop your mod.

# Main Concepts
The following are main concepts key to understanding how Terraria works in multiplayer:
* Clients ("Players") all connect to a single Server via the internet/network code, even with Host and Play.
* Communication from a Client always goes to the Server. The Server can then send the data to the other Clients. Messages can never go Client to Client.
* Code such as AI code executes on all Clients and the Server independently. We must design our code to work deterministically or sync changes from the owner of that entity to the other Clients/Server.


