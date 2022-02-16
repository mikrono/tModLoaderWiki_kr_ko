# NetMessage Class Documentation
This page lists methods pertaining to the NetMessage class.  You can consult this page to get a better understanding of how NetMessage works.  If you want to create custom netcode for your mod, this page is not relevant, see [Basic netcode](https://github.com/tModLoader/tModLoader/wiki/Basic-Netcode) and [Intermediate netcode](https://github.com/tModLoader/tModLoader/wiki/Intermediate-netcode) for that.

Index|
-----|
[Fields](#fields)|
[Methods](#methods)|
[NetMessage.SendData()](#netmessagesenddata)|

# Fields
`NetMessage` doesn't use very many fields due to most of the information being stored in its `MessageBuffer` instances.

| Field   | Type | Description |
|---------|------|-------------|
| [buffer](#buffer)<a name="buffer"></a> | MessageBuffer[257] | Where all outgoing/incoming netcode data is stored. Index 256 corresponds to this client's NetMessage uses, the rest of the indices are directly tied to each player in `Main.player[]` and client in `Netplay.Clients[]`. |
| [_currentPlayerDeathReason](#_currentPlayerDeathReason)<a name = "_currentPlayerDeathReason"></a> | PlayerDeathReason | Used when sending player hurt/kill netcode data. |

# Methods
All netcode data is sent via the `SendData()` method.  However, `NetMessage` does have several other methods for more specific purposes.

## public static void SendChatMessageToClient(NetworkText text, Color color, int playerId)
Sends a text packet to the client player with the given `playerId`.

## public static void BroadcastChatMessage(NetworkText text, Color color, int excludedPlayer = -1)
Like `SendChatMessageToClient()`, but the packet is sent to all players and, optionally, ignores a certain player.

## public static void SendChatMessageFromClient(ChatMessage text)
Sends a text packet from this client's player to the server.

## public static void SendData(int msgType, int remoteClient = -1, int ignoreClient = -1, NetworkText text = null, int number = 0, float number2 = 0f, float number3 = 0f, float number4 = 0f, int number5 = 0, int number6 = 0, int number7 = 0)
Sends netcode data from this client to the server or vice versa.  Covered in more detail in the [next section](#netmessagesenddata).

## public static void SendObjectPlacment(int whoAmi, int x, int y, int type, int style, int alternative, int random, int direction)
Shorthand for `NetMessage.SendData(MessageID.PlaceObject, remoteClient, ignoreClient, null, x, y, type, style, alternative, random, direction);`.  
`remoteClient` is -1 and `ignoreClient` is `whoAmi` if this message is sent from the server and vice versa if this message is sent from a client.

## public static void SendTemporaryAnimation(int whoAmi, int animationType, int tileType, int xCoord, int yCoord)
Shorthand for `NetMessage.SendData(MessageID.TemporaryAnimation, whoAmi, -1, null, animationType, tileType, xCoord, yCoord, 0, 0, 0);`.

## public static void SendTileRange(int whoAmi, int tileX, int tileY, int xSize, int ySize, TileChangeType changeType = TileChangeType.None)
Use this method to update a certain area of tiles.  Has more control over the specified area than `SendTileSquare()`.  
Shorthand for `NetMessage.SendData(MessageID.TileSquare, whoAmi, -1, null, number, tileX, tileY, 0f, (int)changeType, 0, 0);`.  
(tileX, tileY) is the **top-left corner** of the tile square.  
`number` is set to the max of `xSize` and `ySize`.

## public static void SendTileSquare(int whoAmi, int tileX, int tileY, int size, TileChangeType changeType = TileChangeType.None)
Use this method to update a certain area of tiles.  
Shorthand for `NetMessage.SendData(MessageID.TileSquare, whoAmi, -1, null, size, tileX - num, tileY - num, 0f, (int)changeType, 0, 0);`.  
(tileX, tileY) is the **center** for the tile square.  
`num` is set to `(size - 1) / 2`.  
The "center" is biased towards the top-left of the tile square.  For example, passing in `10, 10, 4` for `tileX, tileY, size` will have the top-left tile in the square be at (9, 9).  However, passing in `10, 10, 5` for `tileX, tileY, size` will have the top-left tile in the square be at (8, 8), which makes sense since the square has odd-numbered edges.

## public static void SendTravelShop(int remoteClient)
Informs a certain client of the Travelling Merchant's current shop.
If this game is a server instance, this method is a shorthand for `NetMessage.SendData(MessageID.TravelMerchantItems, remoteClient, -1, null, 0, 0f, 0f, 0f, 0, 0, 0);`.  Otherwise, it does nothing.

## public static void SendAnglerQuest(int remoteClient)
Informs all clients or a specific client of the current Angler quest.  If this game isn't a server instance, this method does nothing.

## public static void SendSection(int whoAmi, int sectionX, int sectionY, bool skipSent = false)
Useful for loading a specific section of the map for multiplayer purposes.  Sections are 200 by 150 tile "chunks" of the world map.  
If this game isn't a server instance, this method does nothing.  
Otherwise, if the given section of the map isn't loaded for the client `Netplay.Clients[whoAmi]`, the section is loaded and all NPCs inside the section send their info to that client.

## public static void sendWater(int x, int y)
For each client, if the tile at `Main.tile[x, y]` is in one of the client's loaded sections of the map, said client is informed that that tile's liquid properties were changed.

# NetMessage.SendData()
### public static void SendData(int msgType, int remoteClient = -1, int ignoreClient = -1, NetworkText text = null, int number = 0, float number2 = 0f, float number3 = 0f, float number4 = 0f, int number5 = 0, int number6 = 0, int number7 = 0)
`NetMessage.SendData()` is the God method for NetMessage, meaning it does anything and everything for sending netcode data.  Below is the documentation for each valid message type, their names, and what data is sent:

## MessageID.NeverCalled (0)
Unused.  Plain and simple.

## MessageID.ClientHello (1)
The first step of client connection to a server.  
*Client state: 0 ->1*  
Vanilla sends the version string `$"Terraria{Main.curRelease}"`.  tModLoader sends the version string `$"{ModLoader.versionedName}"` instead.  
If the client is banned or the client's version string does not match the server's version string, the client is immediately kicked with the appropriate message.  
If the server has a password, the client sends a [MessageID.RequestPassword](#messageidrequestpassword-37) message.  If the server does not have a password, the client sends a [MessageID.LoadPlayer](#messageidloadplayer-3) message instead.  
If the server accepts vanilla clients and the client sent a vanilla version string, tModLoader flags the client as a vanilla client and communicates using an unmodified protocol.  
If the client is not flagged as a vanilla client, the client sends a [MessageID.SyncMods](#messageidsyncmods-251) message instead of a [MessageID.LoadPlayer](#messageidloadplayer-3) message.

| Type(s) | Size | Description |
|------|------|-------------|
| byte | 1 | MessageID.ClientHello |
| string | ? | ModLoader.versionedName |
| byte, string | ? | NetMessage.SendData, message type [MessageID.Kick](#messageidkick-2); only sent if the client is banned, the client's version number doesn't match the server's version number or if the client is using a vanilla client and `ModNet.AllowVanillaClients` is false |
| byte, short, ? | ? | NetMessage.SendData, message type [MessageID.LoadPlayer](#messageidloadplayer-3); only sent if the server doesn't use a password |
| ? | ? | NetMessage.SendData, message type [MessageID.RequestPassword](#messageidrequestpassword-37); only sent if the server uses a password |

## MessageID.Kick (2)
Kicks the client player whose `player.whoAmI` equals `remoteClient` with the given reason `text`.

| Type(s) | Size | Description |
|------|------|-------------|
| byte | 1 | MessageID.Kick|
| byte, string | ? | Information about the `BinaryWriter` used for preparing data to send as well as the `text` reason provided for the kick |

## MessageID.LoadPlayer (3)
The next two steps of client connection to a server.  
*Client state: 1 ->2*  
The client sends [MessageID.SyncPlayer](#messageidsyncplayer-4), [MessageID.ClientUUID](#messageidclientuuid-68), [MessageID.PlayerHealth](#messageidplayerhealth-16), [MessageID.PlayerMana](#messageidplayermana-42), [MessageID.PlayerBuffs](#messageidplayerbuffs-50) and various [MessageID.SyncEquipment](#messageidsyncequipment-5) messages.  
The final [MessageID.SyncEquipment](#messageidsyncequipment-5) messages are sent for the player's main inventory, armor/accessories, equipped dyes, miscellaneous accessories, miscellaneous dyes, Piggy Bank inventory, Safe inventory, trash item slot and Defender's Forge inventory.    
tModLoader then calls `PlayerHooks.SyncPlayer()`.  
The client then sends a [MessageID.RequestWorldInfo](#messageidrequestworldinfo-6) message.  
*Client state: 2 ->3*

| Type(s) | Size | Description |
|------|------|-------------|
| `byte` | 1 | MessageID.LoadPlayer |
| `byte` | 1 | remoteClient; the client's `player.whoAmI` |
| ? | ? | NetMessage.SendData, message type [MessageID.SyncPlayer](#messageidsyncplayer-4) |
| ? | ? | NetMessage.SendData, message type [MessageID.ClientUUID](#messageidclientuuid-68) |
| ? | ? | NetMessage.SendData, message type [MessageID.PlayerHealth](#messageidplayerhealth-16) |
| ? | ? | NetMessage.SendData, message type [MessageID.PlayerMana](#messageidplayermana-42) |
| ? | ? | NetMessage.SendData, message type [MessageID.PlayerBuffs](#messageidplayerbuffs-50) |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's main inventory |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's armor and accessory slots |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's equipped dye slots |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's miscellaneous accessory slots (pet, light pet, hook, minecart, mount) |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's miscellaneous dye slots (for the miscellaneous accessories) |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's Piggy Bank inventory |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's Safe inventory |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for the client player's trash item slot |
| `byte`, `short`, ? | 7 + modded data | NetMessage.SendData, message type [MessageID.SyncEquipment](#messageidsyncequipment-5); called for each item in the client player's Defender's Forge inventory |
| none | 0 | NetMessage.SendData, message type [MessageID.RequestWorldInfo](#messageidrequestworldinfo-6) |

## MessageID.SyncPlayer (4)
// TODO

## MessageID.SyncEquipment (5)
// TODO

## MessageID.RequestWorldInfo (6)
// TODO

## MessageID.WorldData (7)
Sends information about the world to the server and/or other clients.  
Use this `MessageID` whenever anything about the world has changed, including, but not limited to, the time of day and weather.

| Type(s)  | Size | Description
|----------|------|------------
| `int`    | 4    | `Main.time`
| `byte`   | 1    | A `BitsByte` set to the values of `Main.dayTime`, `Main.bloodMoon` and `Main.eclipse`
| `byte`   | 1    | `Main.moonPhase`
| `short`  | 2    | `Main.maxTilesX`
| `short`  | 2    | `Main.maxTilesY`
| `short`  | 2    | `Main.spawnTileX`
| `short`  | 2    | `Main.spawnTileY`
| `short`  | 2    | `Main.worldSurface`
| `short`  | 2    | `Main.rockLayer`
| `int`    | 4    | `Main.worldID`
| `string` | ?    | `Main.worldName`
| `byte[]` | ?    | The return value of `Main.ActiveWorldFileData.UniqueId.ToByteArray()`
| `ulong`  | 8    | `Main.ActiveFileData.WorldGeneratorVersion`
| `byte`   | 1    | `Main.moonType`
| `byte`   | 1    | `WorldGen.treeBG`
| `byte`   | 1    | `WorldGen.corruptBG`
| `byte`   | 1    | `WorldGen.snowBG`
| `byte`   | 1    | `WorldGen.hallowBG`
| `byte`   | 1    | `WorldGen.crimsonBG`
| `byte`   | 1    | `WorldGen.desertBG`
| `byte`   | 1    | `WorldGen.oceanBG`
| `byte`   | 1    | `Main.iceBackStyle`
| `byte`   | 1    | `Main.jungleBackStyle`
| `byte`   | 1    | `Main.hellBackStyle`
| `float`  | 4    | `Main.windSpeedSet`
| `byte`   | 1    | `Main.numClouds`
| `int`    | 12   | The contents of `Main.treeX[]`
| `byte`   | 4    | The contents of `Main.treeStyle[]`, each cast to `byte`
| `int`    | 12   | The contents of `Main.caveBackX[]`
| `byte`   | 4    | The contents of `Main.caveBackStyle[]`, each cast to `byte`
| `float`  | 4    | `Main.maxRaining`
| `byte`   | 1    | A `BitsByte` set to the values of `WorldGen.shadowOrbSmashed`, `NPC.downedBoss1`, `NPC.downedBoss2`, `NPC.downedBoss3`, `Main.hardMode`, `NPC.downedClown` and `NPC.downedPlantBoss`
| `byte`   | 1    | A `BitsByte` set to the values of `NPC.downedMechBoss1`, `NPC.downedMechBoss2`, `NPC.downedMechBoss3`, `NPC.downedMechBossAny`, `Main.cloudBGActive >= 1f`, `WorldGen.crimson`, `Main.pumpkinMoon` and `Main.snowMoon`
| `byte`   | 1    | A `BitsByte` set to the values of `Main.expertMode`, `Main.fastForwardTime`, `Main.slimeRain`, `NPC.downedSlimeKing`, `NPC.downedQueenBee`, `NPC.downedFishron`, `NPC.downedMartian` and `NPC.downedAncientCultist`
| `byte`   | 1    | A `BitsByte` set to the values of `NPC.downedMoonlord`, `NPC.downedHalloweenKing`, `NPC.downedHalloweenTree`, `NPC.downedChristmasIceQueen`, `NPC.downedChristmasSantank`, `NPC.downedChristmasTree`, `NPC.downedGolemBoss` and `BirthdayParty.PartyIsUp`
| `byte`   | 1    | A `BitsByte` set to the values of `NPC.downedPirates`, `NPC.downedFrost`, `NPC.downedGoblins`, `Sandstorm.Happening`, `DD2Event.Ongoing`, `DD2Event.DownedInvasionT1`, `DD2Event.DownedInvasionT2` and `DD2Event.DownedInvasionT3`
| `sbyte`  | 1    | `Main.invasionType`
| ?        | ?    | The result of calling `WorldIO.SendModData(BinaryWriter)`.  `ModWorld.NetSend(BinaryWriter)` is called here.
| `ulong`  | 8    | If `SocialAPI.Network` isn't `null`, then the result of `SocialAPI.Network.GetLobbyId()` is written.  Otherwise, a `0` is written instead.
| `float`  | 4    | `Sandstorm.IntendedSeverity`

Sending this net message has the following side effect:
- `Main.maxRaining` is set to `0f` before it is written to the `BinaryWriter` if `Main.raining` is `false`.

## MessageID.RequestTileData (8)
// TODO

## MessageID.StatusText (9)
// TODO

## MessageID.TileSection (10)
// TODO

## MessageID.FrameSection (11)
// TODO

## MessageID.SpawnPlayer (12)
// TODO

## MessageID.PlayerControls (13)
// TODO

## MessageID.PlayerActive (14)
// TODO

## MessageID.Unused15 (15)
Unused.  Plain and simple.

## MessageID.PlayerHealth (16)
// TODO

## MessageID.TileChange (17)
// TODO

## MessageID.MenuSunMoon (18)
Unused.  Plain and simple.  
Not sure why main menu information would need to be sent through a server anyway.

## MessageID.ChangeDoor (19)
// TODO

## MessageID.TileSquare (20)
// TODO

## MessageID.SyncItem (21)
Sends the information of the dropped item `Main.item[number]`.  If the target item is not `active`, then this MessageID can be used to inform other clients that this item has despawned.  
The `number2` parameter is used when sending the message from the server in `Item.NewItem()` for if `noGrabDelay` is true.  Otherwise, it can just be ignored.

Example:
```cs
int item = Item.NewItem(/* args */);

// Code that directly modifies Main.item[item] here...

if(Main.netMode != NetmodeID.Singleplayer)
    NetMessage.SendData(MessageID.SyncItem, number: item);
```

## MessageID.ItemOwner (22)
// TODO

## MessageID.SyncNPC (23)
// TODO

## MessageID.UnusedStrikeNPC (24)
Unused.  Plain and simple.  
Most likely unused due to it using an item's damage and knockback directly without any of the usual variance.

## MessageID.ChatText (25)
Obsolete.  Either use the [chat message methods](#public-static-void-sendchatmessagetoclientnetworktext-text-color-color-int-playerid) in `NetMessage` or use the `NetTextModule` class.

## MessageID.HurtPlayer (26)
Obsolete.  Use [MessageID.PlayerHurtV2](#messageidplayerhurtv2-117) instead.

## MessageID.SpawnProjectile (27)
// TODO

## MessageID.StrikeNPC (28)
// TODO

## MessageID.KillProjectile (29)
// TODO

## MessageID.PlayerPvP (30)
// TODO

## MessageID.RequestChestOpen (31)
// TODO

## MessageID.SyncChestItem (32)
Syncs the item at `Main.chest[number].item[number2]`.  This message is usually sent within `MessageID.ChestUpdates`, but it still functions normally by itself.

Example:
```cs
Point16 position = /* some tile position */;
//FindByGuessing searches the nearby tiles within a 3x3 area with the target position at the center
//The call will return the index of whatever chest had this target position inside of its 2x2 tile bounds
int chest = Chest.FindByGuessing(position.X, position.Y);

if(chest > -1){
    Item item = Main.chest[chest].item[20] = new Item();
    item.SetDefaults(ItemID.DirtBlock);
    item.stack = 20;

    if(Main.netMode != NetmodeID.Singleplayer)
        NetMessage.SendData(MessageID.SyncChestItem, number: chest, number2: 20, number3: 0);
}
```

## MessageID.SyncPlayerChest (33)
// TODO

## MessageID.ChestUpdates (34)
// TODO

## MessageID.HealEffect (35)
// TODO

## MessageID.PlayerZone (36)
// TODO

## MessageID.RequestPassword (37)
// TODO

## MessageID.SendPassword (38)
// TODO

## MessageID.ResetItemOwner (39)
// TODO

## MessageID.PlayerTalkingNPC (40)
// TODO

## MessageID.ItemAnimation (41)
// TODO

## MessageID.PlayerMana (42)
// TODO

## MessageID.ManaEffect (43)
// TODO

## MessageID.KillPlayer (44)
Obsolete.  Use [MessageID.PlayerDeathV2](#messageidplayerdeathv2-118) instead.

## MessageID.PlayerTeam (45)
// TODO

## MessageID.RequestReadSign (46)
// TODO

## MessageID.ReadSign (47)
// TODO

## MessageID.LiquidUpdate (48)
Obsolete.  Use [MessageID.NetModules](#messageidnetmodules-82) instead.

## MessageID.StartPlaying (49)
// TODO

## MessageID.PlayerBuffs (50)
// TODO

## MessageID.Assorted1 (51)
// TODO

## MessageID.Unlock (52)
// TODO

## MessageID.AddNPCBuff (53)
// TODO

## MessageID.SendNPCBuffs (54)
// TODO

## MessageID.AddPlayerBuff (55)
// TODO

## MessageID.SyncNPCName (56)
// TODO

## MessageID.TileCounts (57)
// TODO

## MessageID.PlayNote (58)
// TODO

## MessageID.HitSwitch (59)
// TODO

## MessageID.NPCHome (60)
// TODO

## MessageID.SpawnBoss (61)
// TODO

## MessageID.Dodge (62)
// TODO

## MessageID.PaintTile (63)
// TODO

## MessageID.PaintWall (64)
// TODO

## MessageID.Teleport (65)
// TODO

## MessageID.SpiritHeal (66)
// TODO

## MessageID.Unused67 (67)
Unused.  Plain and simple.

## MessageID.ClientUUID (68)
// TODO

## MessageID.ChestName (69)
// TODO

## MessageID.BugCatching (70)
// TODO

## MessageID.BugReleasing (71)
// TODO

## MessageID.TravelMerchantItems (72)
// TODO

## MessageID.TelportationPotion (73)
// TODO

## MessageID.AnglerQuest (74)
// TODO

## MessageID.AnglerQuestFinished (75)
// TODO

## MessageID.AnglerQuestCountSync (76)
// TODO

## MessageID.TemporaryAnimatino (77)
// TODO

## MessageID.InvasionProgressReport (78)
// TODO

## MessageID.PlaceObject (79)
// TODO

## MessageID.SyncPlayerChestIndex (80)
// TODO

## MessageID.CombatTextInt (81)
// TODO

## MessageID.NetModules (82)
// TODO

## MessageID.NPCKillCountDeathTally (83)
// TODO

## MessageID.PlayerSealth (84)
// TODO

## MessageID.QuickStackChests (85)
// TODO

## MessageID.TileEntitySharing (86)
Attempts to send a tile entity's data, if the ID provided exists.  The data is sent via `TileEntity.Write(BinaryWriter, TileEntity, bool)`, which calls `ModTileEntity.NetSend(BinaryWriter, bool)`.

Example:
```cs
Point16 tilePosition = new Point16(i, j);
ModTileEntity entity = ModContent.GetInstance<SomeModTileEntity>();

//If the given position does not have an entity on it, place one and send a net message
if(entity.Find(tilePosition.X, tilePosition.Y) < 0){
    int id = entity.Place(tilePosition.X, tilePosition.Y);

    if(Main.netMode == NetmodeID.MultiplayerClient)
        NetMessage.SendData(MessageID.TileEntitySharing, remoteClient: -1, ignoreClient: Main.myPlayer, number: id);
}
```

If the ID no longer exists in the client that sends the message (via `TileEntity.ByID.Remove(int)`), but it does exist on the other clients, the tile entity will be removed on those clients.

Example:
```cs
Point16 tilePosition = new Point16(i, j);
if(TileEntity.ByPosition.TryGetValue(tilePosition, out TileEntity entity)){
    //Assumes that the TileEntity is actually a ModTileEntity, which should be the case if you're using this code
    ModTileEntity existing = entity as ModTileEntity;
    //Kill an entity if it exists at (i, j)
    existing.Kill(i, j);
    
    //Send a net message
    if(Main.netMode == NetmodeID.MultiplayerClient)
        NetMessage.SendData(MessageID.TileEntitySharing, remoteClient: -1, ignoreClient: Main.myPlayer, existing.ID);
}
```

## MessageID.TileEntityPlacement (87)
// TODO

## MessageID.ItemTweaker (88)
// TODO

## MessageID.ItemFrameTryPlacing (89)
// TODO

## MessageID.InstancedItem (90)
// TODO

## MessageID.SyncEmoteBubble (91)
// TODO

## MessageID.SyncExtraValue (92)
Syncs `Main.npc[number].extraValue` by setting it to `number2` and, on clients, plays the "ping" sound at the world position `(number3, number4)`.

Example:
```cs
//"npc" is an NPC instance and "position" is a Vector2 where the "pickup" visuals should be displayed
int givenCoins = 50;  //50 copper coins
npc.extraValue += givenCoins;

if(Main.netMode == NetmodeID.Singleplayer)
    npc.moneyPing(position);
else
    NetMessage.SendData(MessageID.SendExtraValue, number: npc.whoAmI, number2: givenCoins, number3: position.X, number4: position.Y);
```

## MessageID.SocialHandshake (93)
// TODO

## MessageID.Unused94 (94)
Unused.  Plain and simple.

## MessageID.MurderSomeoneElsesProjectile (95)
// TODO

## MessageID.TeleportPlayerThroughPortal (96)
// TODO

## MessageID.AchievementMessageNPCKilled (97)
// TODO

## MessageID.AchievementMessageEventHappened (98)
// TODO

## MessageID.MinionRestTargetUpdate (99)
// TODO

## MessageID.TeleportNPCThroughPortal (100)
// TODO

## MessageID.UpdateTowerShieldStrengths (101)
// TODO

## MessageID.NebulaLevelupRequest (102)
// TODO

## MessageID.MoonlordCountdown (103)
// TODO

## MessageID.ShopOverride (104)
// TODO

## MessageID.GemLockToggle (105)
// TODO

## MessageID.PoofOfSmoke (106)
// TODO

## MessageID.SmartTextMessage (107)
// TODO

## MessageID.WiredCannonShot (108)
// TODO

## MessageID.MassWireOperation (109)
// TODO

## MessageID.MassWireOperationPay (110)
// TODO

## MessageID.ToggleParty (111)
// TODO

## MessageID.SpecialFX (112)
// TODO

## MessageID.CrystalInvasionStart (113)
// TODO

## MessageID.CrystalInvasionWipeAllTheThingsss (114)
No, that's not a typo.  The constant is actually called `CrystalInvasionWipeAllTheThingsss`.  
// TODO

## MessageID.MinionAttackTargetUpdate (115)
// TODO

## MessageID.CrystalInvasionSendWaitTime (116)
// TODO

## MessageID.PlayerHurtV2 (117)
// TODO

## MessageID.PlayerDeathV2 (118)
// TODO

## MessageID.CombatTextString (119)
// TODO

## MessageID.Count (120)
// TODO

## MessageID.InGameChangeConfig (249)
// TODO

## MessageID.ModPacket (250)
// TODO

## MessageID.SyncMods (251)
// TODO

## MessageID.ModFile (252)
// TODO