This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here. For migration information from past versions, see [Update Migration Guide Previous Versions](Update-Migration-Guide-Previous-Versions)

# v2023.X (1.4.4)
The tModLoader team took advantage of the release of Terraria 1.4.4.9 to implement a wide variety of breaking changes. These changes are large features that have been highly requested from the community. The extent of the changes, as well as changes in Terraria, are so large that all mods will need to update to continue working on 1.4.4. tModLoader for 1.4.3 will continue to be available to users. If a modder intends to continue updating their mod for 1.4.3 users, they should use backups or [Git](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Git-&-mod-management) to maintain copies of their source code for 1.4.3 and 1.4.4. 

This migration guide assumes the mod has already been migrated to 1.4.3. If that is not the case, do that first.

The first step in migrating to 1.4.4 is following the [Localization Changes](#localization-changes) section below. Once that is done, switch to the `1.4.4-preview` branch on steam by following the [instructions](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-players#to-access-13-legacy-tmodloader-and-other-beta-options). Launch tModLoader, make sure it says `Terraria v1.4.4.9` and `tModLoader v2023.4.x.y Preview` in the corner. Next, visit the Workshop->Develop Mods menu in game and click on the "Run tModPorter" button. After that completed, you are ready to open Visual Studio and begin working on updating parts of the code that tModPorter either couldn't port or left a comment with instructions (to find all comments, first, go to Tools -> Options -> Environment -> Task List, and add `tModPorter` as a custom token. Then close it, and opening View -> Task List will list all comments, where you can also use the search bar to filter specific comments). As 1.4.4 continues to update during this beta period, you might need to "Run tModPorter" again if an update breaks something.

## Localization Changes
The largest change to tModLoader is that localization is now done fully in the `.hjson` localization files. You **MUST** follow the [Migrating from 1.4.3 to 1.4.4](https://github.com/tModLoader/tModLoader/wiki/Localization#migrating-from-143-to-144) instructions. Failure to do this step will make porting a mod extremely tedious.

## New Vanilla Features
Terraria 1.4.4 has many new features and changes. The [1.4.4 changelog](https://terraria.wiki.gg/wiki/1.4.4) details these changes. Modders should be aware of the changes in case they would impact the balance or functionality of their mods. For example, `NPC` can now have 20 buffs and `Player` can now have 44 buffs by default.

Of specific note is the Shimmer, modders should consider how content in their mod would interact with the Shimmer. [ExampleMod](https://github.com/tModLoader/tModLoader/search?utf8=%E2%9C%93&q=shimmer+path:ExampleMod&type=Code) contains examples of various shimmer features, such as transforming NPC, decrafting examples, and transforming items.

## Renamed or Moved Members

### Namespaces / Classes
* `GameContent.UI.ResourceSets.HorizontalBarsPlayerReosurcesDisplaySet` -> `GameContent.UI.ResourceSets.HorizontalBarsPlayerResourcesDisplaySet`

### Static Methods
* `NetMessage.SendObjectPlacment` -> `NetMessage.SendObjectPlacement`

### Static Fields / Constants / Properties
* `ID.TileID.Sets.TouchDamageSands` -> `ID.TileID.Sets.Suffocate`
* `ID.TileID.Sets.TouchDamageOther` -> `ID.TileID.Sets.TouchDamageImmediate` and possibly `ID.TileID.Sets.TouchDamageBleeding`
* `ID.TileID.Sets.TouchDamageVines` -> `ID.TileID.Sets.TouchDamageImmediate` and `ID.TileID.Sets.TouchDamageDestroyTile`
* `DustID.Fire` -> `DustID.Torch`
* `MessageID.SendNPCBuffs` -> `MessageID.NPCBuffs`
* `MessageID.Unlock` -> `MessageID.LockAndUnlock`
* `MessageID.StartPlaying` -> `MessageID.InitialSpawn`
* `MessageID.SpawnBoss` -> `MessageID.SpawnBossUseLicenseStartEvent`
* `MessageID.Teleport` -> `MessageID.TeleportEntity`
* `MessageID.ClientHello` -> `MessageID.Hello
* `MessageID.LoadPlayer` -> `MessageID.PlayerInfo`
* `MessageID.RequestWorldInfo` -> `MessageID.RequestWorldData`
* `MessageID.RequestTileData` -> `MessageID.SpawnTileData`
* `MessageID.StatusText` -> `MessageID.StatusTextSize`
* `MessageID.FrameSection` -> `MessageID.TileFrameSection`
* `MessageID.SpawnPlayer` -> `MessageID.PlayerSpawn`
* `MessageID.PlayerHealth` -> `MessageID.PlayerLifeMana`
* `MessageID.TileChange` -> `MessageID.TileManipulation`
* `MessageID.MenuSunMoon` -> `MessageID.SetTime`
* `MessageID.ChangeDoor` -> `MessageID.ToggleDoorState`
* `MessageID.UnusedStrikeNPC` -> `MessageID.UnusedMeleeStrike`
* `MessageID.StrikeNPC` -> `MessageID.DamageNPC`
* `MessageID.PlayerPVP` -> `MessageID.TogglePVP`
* `MessageID.HealEffect` -> `MessageID.PlayerHeal`
* `MessageID.PlayerZone` -> `MessageID.SyncPlayerZone`
* `MessageID.ResetItemOwner` -> `MessageID.ReleaseItemOwnership`
* `MessageID.PlayerTalkingNPC` -> `MessageID.SyncTalkNPC`
* `MessageID.ItemAnimation` -> `MessageID.ShotAnimationAndSound`
* `MessageID.MurderSomeoneElsesProjectile` -> `MessageID.MurderSomeoneElsesPortal`
* `Main.fastForwardTime` -> Removed, use `IsFastForwardingTime()`, `fastForwardTimeToDawn` or `fastForwardTimeToDusk`
* The following all change from `Terraria.WorldGen` to `Terraria.WorldBuilding.GenVars`: `configuration`, `structures`, `copper`, `iron`, `silver`, `gold`, `copperBar`, `ironBar`, `silverBar`, `goldBar`, `mossTile`, `mossWall`, `lavaLine`, `waterLine`, `worldSurfaceLow`, `worldSurface`, `worldSurfaceHigh`, `rockLayerLow`, `rockLayer`, `rockLayerHigh`, `snowTop`, `snowBottom`, `snowOriginLeft`, `snowOriginRight`, `snowMinX`, `snowMaxX`, `leftBeachEnd`, `rightBeachStart`, `beachBordersWidth`, `beachSandRandomCenter`, `beachSandRandomWidthRange`, `beachSandDungeonExtraWidth`, `beachSandJungleExtraWidth`, `shellStartXLeft`, `shellStartYLeft`, `shellStartXRight`, `shellStartYRight`, `oceanWaterStartRandomMin`, `oceanWaterStartRandomMax`, `oceanWaterForcedJungleLength`, `evilBiomeBeachAvoidance`, `evilBiomeAvoidanceMidFixer`, `lakesBeachAvoidance`, `smallHolesBeachAvoidance`, `surfaceCavesBeachAvoidance2`, `maxOceanCaveTreasure`, `numOceanCaveTreasure`, `oceanCaveTreasure`, `skipDesertTileCheck`, `UndergroundDesertLocation`, `UndergroundDesertHiveLocation`, `desertHiveHigh`, `desertHiveLow`, `desertHiveLeft`, `desertHiveRight`, `numLarva`, `larvaY`, `larvaX`, `numPyr`, `PyrX`, `PyrY`, `jungleOriginX`, `jungleMinX`, `jungleMaxX`, `JungleX`, `jungleHut`, `mudWall`, `JungleItemCount`, `JChestX`, `JChestY`, `numJChests`, `tLeft`, `tRight`, `tTop`, `tBottom`, `tRooms`, `lAltarX`, `lAltarY`, `dungeonSide`, `dungeonLocation`, `dungeonLake`, `crackedType`, `dungeonX`, `dungeonY`, `lastDungeonHall`, `maxDRooms`, `numDRooms`, `dRoomX`, `dRoomY`, `dRoomSize`, `dRoomTreasure`, `dRoomL`, `dRoomR`, `dRoomT`, `dRoomB`, `numDDoors`, `DDoorX`, `DDoorY`, `DDoorPos`, `numDungeonPlatforms`, `dungeonPlatformX`, `dungeonPlatformY`, `dEnteranceX`, `dSurface`, `dxStrength1`, `dyStrength1`, `dxStrength2`, `dyStrength2`, `dMinX`, `dMaxX`, `dMinY`, `dMaxY`, `skyLakes`, `generatedShadowKey`, `numIslandHouses`, `skyLake`, `floatingIslandHouseX`, `floatingIslandHouseY`, `floatingIslandStyle`, `numMCaves`, `mCaveX`, `mCaveY`, `maxTunnels`, `numTunnels`, `tunnelX`, `maxOrePatch`, `numOrePatch`, `orePatchX`, `maxMushroomBiomes`, `numMushroomBiomes`, `mushroomBiomesPosition`, `logX`, `logY`, `maxLakes`, `numLakes`, `LakeX`, `maxOasis`, `numOasis`, `oasisPosition`, `oasisWidth`, `oasisHeight`, `hellChest`, `hellChestItem`, `statueList`, `StatuesWithTraps`
* `WorldGen.houseCount` -> `WorldBuilding.GenVars.skyIslandHouseCount`

### Non-Static Methods
* `UI.UIElement.MouseDown` -> `UI.UIElement.LeftMouseDown`
* `UI.UIElement.MouseUp` -> `UI.UIElement.LeftMouseUp`
* `UI.UIElement.Click` -> `UI.UIElement.LeftClick`
* `UI.UIElement.DoubleClick` -> `UI.UIElement.LeftDoubleClick`
* `Player.VanillaUpdateEquip` -> Removed, use either `GrantPrefixBenefits` (if `Item.accessory`) or `GrantArmorBenefits` (for armor slots)
* `Player.IsAValidEquipmentSlotForIteration` -> `Player.IsItemSlotUnlockedAndUsable`
* `Item.DefaultToPlacableWall` -> `Item.DefaultToPlaceableWall`

### Non-Static Fields / Constants / Properties
* `UI.UIElement.OnMouseDown` -> `UI.UIElement.OnLeftMouseDown`
* `UI.UIElement.OnMouseUp` -> `UI.UIElement.OnLeftMouseUp`
* `UI.UIElement.OnClick` -> `UI.UIElement.OnLeftClick`
* `UI.UIElement.OnDoubleClick` -> `UI.UIElement.OnLeftDoubleClick`
* `Player.discount` -> `Player.discountAvailable`
* `Item.canBePlacedInVanityRegardlessOfConditions` -> `Item.hasVanityEffects`

### Misc changes

### tModLoader changes
The following contains smaller scale changes to tModLoader members. More elaborate changes are handled in separate categories below.

* `ModTile.OpenDoorID` removed, use `TileID.Sets.OpenDoorID` instead
* `ModTile.CloseDoorID` removed, use `TileID.Sets.CloseDoorID` instead
* `NPCSpawnInfo.PlanteraDefeated` removed, use `(NPC.downedPlantBoss && Main.hardMode)` instead
* `ModNPC.ScaleExpertStats` -> `ModNPC.ApplyDifficultyAndPlayerScaling`, parameters changed. `bossLifeScale` -> `balance` (`bossAdjustment` is different, see the docs for details) 
* `GlobalNPC.ScaleExpertStats` -> `GlobalNPC.ApplyDifficultyAndPlayerScaling`, parameters changed. `bossLifeScale` -> `balance` (`bossAdjustment` is different, see the docs for details) 
* `ModNPC.CanTownNPCSpawn` parameters changed, copy the implementation of `NPC.SpawnAllowed_Merchant` in vanilla if you to count money, and be sure to set a flag when unlocked, so you don't count every tick
* `ModItem.SacrificeTotal` -> `Item.ResearchUnlockCount`
* `ModItem.OnCreate` -> `ModItem.OnCreated`
* `GlobalItem.OnCreate` -> `GlobalItem.OnCreated`
* `ModItem.CanBurnInLava` -> Use `ItemID.Sets.IsLavaImmuneRegardlessOfRarity` or add a method hook to `On_Item.CheckLavaDeath`
* `GlobalItem.CanBurnInLava` -> Use `ItemID.Sets.IsLavaImmuneRegardlessOfRarity` or add a method hook to `On_Item.CheckLavaDeath`
* `ModItem.ExtractinatorUse` parameters changed
* `GlobalItem.ExtractinatorUse` parameters changed
* `Player.QuickSpawnClonedItem` -> `Player.QuickSpawnItem`
* `ModPlayer.clientClone` -> Renamed to `CopyClientState` and replace `Item.Clone` usages with `Item.CopyNetStateTo`
* `ModPlayer.PlayerConnect` parameters changed
* `ModPlayer.PlayerDisconnect` parameters changed
* `ModPlayer.OnEnterWorld` parameters changed
* `ModPlayer.OnRespawn` parameters changed
* `ModTree.GrowthFXGore` -> `ModTree.TreeLeaf`
* `Terraria.DataStructures.BossBarDrawParams.LifePercentToShow` removed, use `Life / LifeMax` instead
* `Terraria.DataStructures.BossBarDrawParams.ShieldPercentToShow` removed, use `Shield / ShieldMax` instead
* `ModBossBar.ModifyInfo` life and shield current and max values are now separate to allow for hp/shield number text draw
* `ModItem/GlobalItem.ModifyHitNPC/OnHitNPC/ModifyHitPvp/OnHitPvp` -> See Player/NPC damage hooks rework section below
* `ModNPC/GlobalNPC.HitEffect/ModifyHitPlayer/OnHitPlayer/ModifyHitNPC/OnHitNPC/ModifyHitByItem/OnHitByItem/ModifyHitByProjectile/OnHitByProjectile/ModifyIncomingHit/ModifyCollisionData/StrikeNPC` -> See Player/NPC damage hooks rework section below
* `ModProjectile/GlobalProjectile.ModifyHitNPC/OnHitNPC/ModifyHitPlayer/OnHitPlayer/ModifyDamageScaling` -> See Player/NPC damage hooks rework section below
* `ModPlayer.ModifyHurt/OnHurt/PostHurt/ModifyHitNPCWithItem/OnHitNPCWithItem/ModifyHitNPCWithProj/OnHitNPCWithProj/ModifyHitByNPC/OnHitByNPC/ModifyHitByProjectile/OnHitByProjectile/CanHitNPC/ModifyHitNPC/OnHitNPC/PreHurt/Hurt` -> See Player/NPC damage hooks rework section below
* `ModProjectile.SingleGrappleHook` -> in `SetStaticDefaults`, use `ProjectileID.Sets.SingleGrappleHook[Type] = true` if you previously had this method return true
* `GlobalProjectile.SingleGrappleHook` -> in `SetStaticDefaults`, use `ProjectileID.Sets.SingleGrappleHook[Type] = true` if you previously had this method return true
* `ModPrefix.AutoStaticDefaults` -> Nothing to override anymore. Use hjson files and/or override DisplayName to adjust localization
* `ModPrefix.ValidateItem` -> `ModPrefix.AllStatChangesHaveEffectOn`
* `ModSystem.SetLanguage` -> Use `OnLocalizationsLoaded`. New hook is called at slightly different times, so read the documentation
* `ModSystem.ModifyWorldGenTasks` parameters changed
* `ModLoader.ItemCreationContext` -> `DataStructures.ItemCreationContext`
* `ModLoader.RecipeCreationContext` -> `DataStructures.RecipeItemCreationContext`
* `ModLoader.InitializationContext` -> `ModLoader.InitializationItemCreationContext`
* `Terraria.Recipe.Condition` -> `Terraria.Condition`
* `Condition.InGraveyardBiome` -> `Condition.InGraveyard`
* `Item.IsCandidateForReforge` removed, use `maxStack == 1 || Item.AllowReforgeForStackableItem` or `Item.Prefix(-3)` to check whether an item is reforgeable
* `Item.CloneWithModdedDataFrom` removed, use `Clone`, `ResetPrefix` or `Refresh`
* `ModLoader.Mod.CreateTranslation` removed, use `Localization.Language.GetOrRegister`. See [Localization Changes Details](#localization-changes-details) below.
* `ModLoader.Mod.AddTranslation` removed, use `Localization.Language.GetOrRegister`. See [Localization Changes Details](#localization-changes-details) below.
* `ModLoader.ModTranslation` removed, use `Localization.LocalizedText` instead. See [Localization Changes Details](#localization-changes-details) below.
* `InfoDisplay.InfoName` -> `InfoDisplay.DisplayName`
* `InfoDisplay.DisplayValue` -> suggestion: Set displayColor to InactiveInfoTextColor if your display value is "zero" or shows no valuable information
* `DamageClass.ClassName` -> `DamageClass.DisplayName`
* `GlobalTile.Drop/CanDrop` and `ModTile.Drop/CanDrop` -> These methods changed drastically. See [Tile Drop Changes](#tile-drop-changes) for more information.
* `ModTile.ChestDrop` -> Now use `ItemDrop`, if needed. See [Tile Drop Changes](#tile-drop-changes) for more information.
* `ModTile.DresserDrop` -> Now use `ItemDrop`, if needed. See [Tile Drop Changes](#tile-drop-changes) for more information.
* `ModTile.ContainerName` -> Removed, override `DefaultContainerName` instead
* `TileLoader.ContainerName` -> `TileLoader.DefaultContainerName`, parameters changed
* `ModBuff.ModifyBuffTip` -> `ModBuff.ModifyBuffText`, parameters changed
* `GlobalBuff.ModifyBuffTip` -> `GlobalBuff.ModifyBuffText`, parameters changed
* `ModNPC/GlobalNPC.DrawTownAttackSwing` -> Parameters changed
* `ModNPC/GlobalNPC.DrawTownAttackGun` -> Parameters changed. `closeness` is now `horizontalHoldoutOffset`, use `horizontalHoldoutOffset = Main.DrawPlayerItemPos(1f, itemtype) - originalClosenessValue` to adjust to the change. See docs for how to use hook with an item type.
* `ModNPC/GlobalNPC.SetupShop` -> `ModifyActiveShop`. Shops have drastically changed, see [Shop Changes](#shop-changes) for more information.
* `ModNPC.OnChatButtonClicked` ->  Parameters changed
* `ModNPC.ModifyActiveShop` ->  Parameters changed. Shops have drastically changed, see [Shop Changes](#shop-changes) for more information.
* `GlobalNPC.ModifyActiveShop` ->  Parameters changed. Shops have drastically changed, see [Shop Changes](#shop-changes) for more information.
* `ModPylon.GetNPCShopEntry` ->  Parameters changed. Shops have drastically changed, see [Shop Changes](#shop-changes) for more information.
* `ModPylon.IsPylonForSale` ->  `ModPylon.GetNPCShopEntry`, see ExamplePylonTile for an example. To register to specific NPC shops, use the new shop system directly in ModNPC.AddShop, GlobalNPC.ModifyShop or ModSystem.PostAddRecipes

## Big change concepts

### Localization Changes Details
WIP

### Player/NPC damage hooks rework. Hit/HurtModifiers and Hit/HurtInfo
[PR 3212](https://github.com/tModLoader/tModLoader/pull/3212), [PR 3355](https://github.com/tModLoader/tModLoader/pull/3355), and [PR 3359](https://github.com/tModLoader/tModLoader/pull/3359) drastically changes how player and npc hit hooks are structured.   

**Short Summary:**    
Player and NPC hit hooks (`OnHit`, `ModifyHit`, `OnHitBy`, etc) have been simplified with common architecture, and had many new features added.

The goal of this change is to improve mod compatibility by making the damage calculations an explicit equation, where mods contribute modifiers, and tModLoader calculates the result. 

Many examples of how to make hit/hurt modifying accessories and weapon effects have been added, demonstrating the new system.
* [ExampleDodgeBuff](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Buffs/ExampleDodgeBuff.cs)/ExampleDamageModificationPlayer: `ConsumableDodge` example, similar to 
* [ExampleDefenseDebuff](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Buffs/ExampleDefenseDebuff.cs): Multiplicative defense debuff, similar to Broken Armor
* [AbsorbTeamDamageAccessory](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Accessories/AbsorbTeamDamageAccessory.cs)/AbsorbTeamDamageBuff/ExampleDamageModificationPlayer: Damage absorption example, similar to Paladins Shield item.
* [HitModifiersShowcase](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Weapons/HitModifiersShowcase.cs): Extensive examples of new `ModifyHitNPC` and `OnHitNPC` hooks. Disable damage variation, modify knockback, modify crit bonus damage, flat armor penetration, percentage armor penetration, etc examples.
* ExampleWhipDebuff and ExampleWhipAdvancedDebuff: Show proper tag damage, both flat and scaling varieties.
* PartyZombie: Shows an NPC that is "resistant" to a specific damage class, taking less damage
* ExampleAdvancedFlailProjectile: Shows ModifyDamageScaling replacement, as well as proper usage of various `HitModifiers` fields and properties to scale damage and knockback dynamically.
* ExamplePaperAirplaneProjectile: Shows a projectile dealing bonus damage to an npc that is weak to it.

tModPorter will be able to convert the hook signatures, but you will need to read the Porting Notes (in particular the FAQ), to figure out how to convert old damage modification code into the new system.

**Porting Notes:**    
Too many to write here, please read the PR descriptions if you have questions after running tModPorter.

### Shop Changes (aka Declarative Shops)
[PR 3219](https://github.com/tModLoader/tModLoader/pull/3219) has changed how NPC shops are done.

**Short Summary:**    
• NPC shops are now declarative, meaning they get registered, and items can be added to them with conditions, similarly to NPC drops (loot) and recipes    
• Adding items to shops, or hiding items from shops is now as easy as adding or disabling recipes    
• Info mods can traverse the NPCShopDatabase to show how to obtain an item. All the conditions for items appearing in shops have been localized.    
• Registering multiple shops per NPC is perfectly fine, and selecting the shop to be opened when chatting can now be done via the ref string shop parameter in OnChatButtonClicked

**Porting Notes:**    
• `ModPylon.IsPylonForSale` has been replaced by `ModPylon.GetNPCShopEntry`. Please see the documentation and [ExamplePylonTile](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Tiles/ExamplePylonTile.cs)    
• `Mod/GlobalNPC.SetupShop` is now `ModifyActiveShop`. This hook still exists to allow for custom shop modification, but you should consider moving to the new `ModNPC.AddShops` and `GlobalNPC.ModifyShop` hooks    
• The `Item[]` in `ModifyActiveShop` will now contain null entries. This distinguishes slots which have not been filled, from slots which are intentionally left empty (as in the case of DD2 Bartender)    
• Please take a look at the changes to Example Mod in the PR to more easily understand the new system.    

### Tile Drop Changes
WIP

## Smaller Changes
### [PR 3063](https://github.com/tModLoader/tModLoader/pull/3063): Fix Quick Heal and Quick Mana consuming non-consumables
**Short Summary:** Adds `item.consumable` as a check in Quick Heal and Quick Mana.     
**Porting Notes:** For Quick heal, Quick Mana non-consummables, it is recommended to use `Item.consumable` field instead of overriding `ConsumeItem()`

### [PR 3360](https://github.com/tModLoader/tModLoader/pull/3360): Add ModSystem.ClearWorld
**Short Summary:** Adds ModSystem.ClearWorld which runs on world clear. The new best place to clear/initialize world related data structures.    
**Porting Notes:** Consider replacing overrides of `OnWorldLoad` with `ClearWorld` to reset world related data. Generally no need to override `OnWorldUnload` anymore. Use Unload if you want to fully clean up all memory, but empty lists are nothing to worry about.

### [PR 3341](https://github.com/tModLoader/tModLoader/pull/3341): Unify Localized Conditions
**Short Summary:** Terraria.Recipe.Condition has been moved to Terraria.Condition and can now be applied to more things. Recipes, item variants, drops and soon shops. Added SimpleItemDropRuleCondition class to help make drop conditions more easily.    
**Porting Notes:** `Terraria.Recipe.Condition` -> `Terraria.Condition` (tModPorter). `Recipe` parameter removed from condition delegate, since it was almost always unused. Custom conditions will have to change from `_ => calculation` or `recipe => calculation` to `() => calculation`
