This page contains guides for migrating your code to new methods and functionality of newer tModLoader versions. When a tModLoader update requires rewriting code, we will present the information here. For migration information from past versions, see [Update Migration Guide Previous Versions](Update-Migration-Guide-Previous-Versions)

<!-- Generated with https://luciopaiva.com/markdown-toc/ -->
# Table of contents

- [v2023.08](#v202308)
- [v2023.X (1.4.4)](#v2023x-144)
  - [Localization Changes](#localization-changes)
  - [New Vanilla Features](#new-vanilla-features)
  - [Renamed or Moved Members](#renamed-or-moved-members)
  - [Big change concepts](#big-change-concepts)
    - [Localization Changes Details](#localization-changes-details)
    - [Player/NPC damage hooks rework. Hit/HurtModifiers and Hit/HurtInfo](#playernpc-damage-hooks-rework-hithurtmodifiers-and-hithurtinfo)
    - [Shop Changes (aka Declarative Shops)](#shop-changes-aka-declarative-shops)
    - [Tile Drop Changes](#tile-drop-changes)
    - [Improve Player.clientClone performance](#improve-playerclientclone-performance)
    - [Max Health and Mana Manipulation API](#max-health-and-mana-manipulation-api)
    - [Extra Jump API](#extra-jump-api)
  - [Smaller Changes](#smaller-changes)

# v2023.08
> Currently in Preview, coming to Stable on October 1st

### [PR 3770](https://github.com/tModLoader/tModLoader/pull/3770): Rename `(Mod|Global)Projectile.Kill` hook to `OnKill`
**Short Summary:** `(Mod|Global)Projectile.Kill` renamed to `OnKill` to better match the behavior and other similar hooks.     
**Porting Notes:** Run tModPorter or rename usages of `(Mod|Global)Projectile.Kill` to `OnKill`

### [PR 3759](https://github.com/tModLoader/tModLoader/pull/3759): `ModPlayer.OnPickup` hook
**Short Summary:** 
- Adds `ModPlayer.OnPickup` which functions the same as the `GlobalItem` hook.     
- The hook has been added for convenience, to reduce the need to make a separate `GlobalItem` class when many of the effects modders want to make are stored on a `ModPlayer` instance

### [PR 3746](https://github.com/tModLoader/tModLoader/pull/3746): Add modded world header data
**Short Summary:** 
- Modded world data can be now be saved into a 'header' in the .twld file. The header can be read without deserializing the entire .twld file, and the modded data is accessible in the world select menu and during vanilla world loading.
- The list of mods the world was last played with is now shown in the world select menu, just like for players
- The list of mods (and version of those mods) the world was generated with is now stored in the header. Only applies to worlds generated in the future of course.    

See [PR 3746](https://github.com/tModLoader/tModLoader/pull/3746) for more details and usage examples    

### [PR 2918](https://github.com/tModLoader/tModLoader/pull/2918): Modded Emote Bubble
**Short Summary:**  
- Modders can now make custom emotes
- Modders can adjust how NPC pick emotes
- ExampleMod shows off several custom emotes and custom emote spawning    

### [PR 3731](https://github.com/tModLoader/tModLoader/pull/3731): Modded Builder Toggles
**Short Summary:** Modders can now make builder toggles, which are those small icons top left of the inventory that are used for block swap, wire visibility, etc.     
**Porting Notes:** If you previously made builders toggles using your own approach, use the tModLoader approach.

### [PR 3710](https://github.com/tModLoader/tModLoader/pull/3710): Better Changelogs
**Short Summary:** Expand ChangeLog functionality     
**Porting Notes:** 
-  tModLoader no longer adds it's own text when a changelog.txt is provided. We recommend adding to your own changelog.txt files based on [ExampleMod's changelog.txt](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/changelog.txt)
- All 4 fields in the example, such as `{ModVersion}`, are replaced with the info from tModLoader during publishing for convenience.

### [PR 3453](https://github.com/tModLoader/tModLoader/pull/3453): Rework `NPCID.Sets.DebuffImmunitySets`
**Short Summary:**      
- Replace `NPCID.Sets.DebuffImmunitySets` with `NPCID.Sets.SpecificDebuffImmunity`, `NPCID.Sets.ImmuneToAllBuffs`, and `NPCID.Sets.ImmuneToRegularBuffs` to simplify modder code.
- Added buff immunity inheritance through the `BuffID.Sets.GrantImmunityWith` set and corresponding methods.

**Porting Notes:** 
- If your mod has any NPCs or does anything with buff immunity, you'll need to update their buff immunity code. Read the Porting Notes section of [PR 3453](https://github.com/tModLoader/tModLoader/pull/3453).
- Mods should consider using the new buff immunity inheritance system for buff inheritance compatibility.

### [PR 3568](https://github.com/tModLoader/tModLoader/pull/3568): Rubblemaker support
**Short Summary:** Modders can now add tiles to the [Rubblemaker](https://terraria.wiki.gg/wiki/Rubblemaker)   

### Other v2023.08 Changes
* [`ItemID.Sets.IsSpaceGun` added](https://github.com/tModLoader/tModLoader/commit/b6a6fcc40d2d39aed6df98e862f29e8deaf7a77e)
* [`GlobalInfoDisplay.ModifyDisplayParameters` replaces `ModifyDisplayValue`/`ModifyDisplayName`/`ModifyDisplayColor`](https://github.com/tModLoader/tModLoader/commit/3ca9bf16a3722c35ea86377b7a28f4456b072743)
* [`GenPass.Disable` and `GenPass.Enabled`](https://github.com/tModLoader/tModLoader/commit/8460afd82e71a88b7fa3856713e33baa4f0fda57)
* [`TooltipLine.Hide` and `TooltipLine.Visible`](https://github.com/tModLoader/tModLoader/commit/42daf2a8789bebab092902a4dad59bdfdfd88a17)
* [`ModTree.Shake`'s `createLeaves` parameter now defaults to `true`](https://github.com/tModLoader/tModLoader/commit/1352e4b5c8109de9f86cc20735bceb91ebdb5198)
* [Automatic TranslationsNeeded.txt feature](https://github.com/tModLoader/tModLoader/commit/aee3e5ece65982d28dd0e4c5930e4fbc501e3882)
* [`GlobalInfoDisplay.ModifyDisplayColor` has new `displayShadowColor` parameter](https://github.com/tModLoader/tModLoader/commit/8e0274a1fb317b856600a804acc93cd2635d31fe)
* [`DamageClassLoader.GetDamageClass` added](https://github.com/tModLoader/tModLoader/commit/e9572b649b8e8af69c2d493b33043bb61e3e9c9a)
* [`TileRestingInfo` constructor changed](https://github.com/tModLoader/tModLoader/commit/e5ac187611e201685650bca482cbecfa457b7649)
* [`ModTile.IsTileBiomeSightable` hook](https://github.com/tModLoader/tModLoader/commit/57d8c0de992e688465257c184f2eaf1c7307ea3e)
* [`SceneMetrics.GetLiquidCount` added](https://github.com/tModLoader/tModLoader/commit/d03a022416e01f7858800454e20ce28a1d5c954b)
* [`NPCID.Sets.BelongsToInvasionGoblinArmy`/`BelongsToInvasionFrostLegion`/`BelongsToInvasionPirate`/`BelongsToInvasionMartianMadness`/`NoInvasionMusic`/`InvasionSlotCount` added](https://github.com/tModLoader/tModLoader/commit/4f378265980bc513105618113a22b76f531358b3)
* [`ArmorIDs.Head.Sets.IsTallHat` added](https://github.com/tModLoader/tModLoader/commit/a2f8cfa204f403fe6d99aee67a6722a22329cf0d)
* [`GoreID.Sets.PaintedFallingLeaf` added](https://github.com/tModLoader/tModLoader/commit/e171965d2547488d7eca5d45c4eac6d0cadf252c)
* [All `LocalizationCategory` implementations now virtual](https://github.com/tModLoader/tModLoader/commit/ffea871bc30f5eea3ba0f4d2162ce26c38d8eb2d)

# v2023.X (1.4.4)
The tModLoader team took advantage of the release of Terraria 1.4.4.9 to implement a wide variety of breaking changes. These changes are large features that have been highly requested from the community. The extent of the changes, as well as changes in Terraria, are so large that all mods will need to update to continue working on 1.4.4. tModLoader for 1.4.3 will continue to be available to users as the `1.4.3-legacy` steam beta option. If a modder intends to continue updating their mod for 1.4.3 users, they should use backups or [Git](https://github.com/tModLoader/tModLoader/wiki/Intermediate-Git-&-mod-management) to maintain copies of their source code for 1.4.3 and 1.4.4. 

This migration guide assumes the mod has already been migrated to 1.4.3. If that is not the case, do that first.

The first step in migrating to 1.4.4 is following the [Localization Changes](#localization-changes) section below. Once that is done, switch to the `None` branch on steam by following the [instructions](https://github.com/tModLoader/tModLoader/wiki/tModLoader-guide-for-players#to-access-13-legacy-tmodloader-and-other-beta-options). Launch tModLoader, make sure it says `Terraria v1.4.4.9` and `tModLoader v2023.6.x.y` in the corner. Next, visit the Workshop->Develop Mods menu in game and click on the "Run tModPorter" button. After that completed, you are ready to open Visual Studio and begin working on updating parts of the code that tModPorter either couldn't port or left a comment with instructions (to find all comments, first, go to Tools -> Options -> Environment -> Task List, and add `tModPorter` as a custom token. Then close it, and opening View -> Task List will list all comments, where you can also use the search bar to filter specific comments). As 1.4.4 continues to update, you might need to "Run tModPorter" again if an update breaks something.

## Localization Changes
The largest change to tModLoader is that localization is now done fully in the `.hjson` localization files. You **MUST** follow the [Migrating from 1.4.3 to 1.4.4](https://github.com/tModLoader/tModLoader/wiki/Localization#migrating-from-143-to-144) instructions. Failure to do this step will make porting a mod extremely tedious. More details can be found in [Localization Changes details](#localization-changes-details).

## New Vanilla Features
Terraria 1.4.4 has many new features and changes. The [1.4.4 changelog](https://terraria.wiki.gg/wiki/1.4.4) details these changes. Modders should be aware of the changes in case they would impact the balance or functionality of their mods. For example, `NPC` can now have 20 buffs and `Player` can now have 44 buffs by default.

Of specific note is the Shimmer, modders should consider how content in their mod would interact with the Shimmer. [ExampleMod](https://github.com/tModLoader/tModLoader/search?utf8=%E2%9C%93&q=shimmer+path:ExampleMod&type=Code) contains examples of various shimmer features, such as transforming NPC, decrafting examples, and transforming items.

### Other Vanilla Changes
* `ModWaterStyle` now requires an additional texture, `_Slope`. See `ExampleWaterStyle` for details.
* `GrantPrefixBenefits` is only called if `Item.accessory` is `true`. This applies in mod accessory slots too now.
* Reforging is now implemented via `Item.ResetPrefix`. This sets `prefix` to 0 and then refreshes the item. Make sure any custom fields set by custom prefixes are not serialized independently.

## Renamed or Moved Members
All of the changes in this section will be handled by tModPorter and are listed here for completeness. Modders can skip this section and go directly to [Big change concepts](#big-change-concepts) to see the changes that require the modder to make big changes to their mod.

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
* `MessageID.ClientHello` -> `MessageID.Hello`
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
* `ModNPC/GlobalNPC.SetupShop` -> `ModifyActiveShop`. Shops have drastically changed, see [Shop Changes](#shop-changes-aka-declarative-shops) for more information.
* `ModNPC.OnChatButtonClicked` ->  Parameters changed
* `ModNPC.ModifyActiveShop` ->  Parameters changed. Shops have drastically changed, see [Shop Changes](#shop-changes-aka-declarative-shops) for more information.
* `GlobalNPC.ModifyActiveShop` ->  Parameters changed. Shops have drastically changed, see [Shop Changes](#shop-changes-aka-declarative-shops) for more information.
* `ModPylon.GetNPCShopEntry` ->  Parameters changed. Shops have drastically changed, see [Shop Changes](#shop-changes-aka-declarative-shops) for more information.
* `ModPylon.IsPylonForSale` ->  `ModPylon.GetNPCShopEntry`, see ExamplePylonTile for an example. To register to specific NPC shops, use the new shop system directly in ModNPC.AddShop, GlobalNPC.ModifyShop or ModSystem.PostAddRecipes
* `ModItem/GlobalItem.PreReforge` return type changed from `bool` to `void`, no longer used to prevent reforging. Use `CanReforge` for that purpose.
* `Player.CanBuyItem` removed, use `Player.CanAfford` instead. Note that the method will no longer mistakenly attempt to consume custom currencies.
* `Player.rocketDamage` -> `Player.specialistDamage`
* `AmmoID.Sets.IsRocket` -> `AmmoID.Sets.IsSpecialist`
* `ModPrefix.GetTooltipLines` added. Some modders might want to move prefix specific tooltip lines from `GlobalItem.ModifyTooltips` to it for better code maintainability.
* `Mods.{ModName}.TownNPCMood.{NPCName}.*` localization entries relocated to `Mods.{ModName}.NPCs.{NPCName}.TownNPCMood.*`. See the porting notes in [PR 3446](https://github.com/tModLoader/tModLoader/pull/3446) for more information.
* `ModItem.AutoLightSelect` removed. See the porting notes in [PR 3479](https://github.com/tModLoader/tModLoader/pull/3479) for more information on how to adapt to this change.
*  `Player.BiomeTorchPlaceStyle` and `Player.BiomeCampfirePlaceStyle` parameters changed

## Big change concepts

### Localization Changes Details
[Issue 3074](https://github.com/tModLoader/tModLoader/pull/3074) contains the proposed changes.    
[PR 3101](https://github.com/tModLoader/tModLoader/pull/3101/files) contains the actual changes, as well as ExampleMod changes.  
[PR 3302](https://github.com/tModLoader/tModLoader/pull/3302) contains additional changes specific to `ModConfig`.    
[Localization](https://github.com/tModLoader/tModLoader/wiki/Localization) explains localization and porting instructions for modders.    
[Contributing Localization](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization) contains instructions for translators and translation mod makers.

**Short Summary**:    
* Translations are now fully in localization files (.hjson files). `ModItem.DisplayName` and `ModItem.Tooltip`, for example, can no longer be assigned in code.
  * `ModConfig` is now fully localizable by default. (See [PR #3302](https://github.com/tModLoader/tModLoader/pull/3302) for more information)
* Localization files are automatically updated with entries for new content and managed by tModLoader. More organization options available.
  * New content will appear in localization files after loading. Edits to .hjson files will be detected by tModLoader and loaded into the game as soon as they are saved, allowing modders and translators to quickly test and update translations.
* All `ModTranslation` usages are now replaced with `LocalizedText`
* All translation keys now follow a more predictable pattern: `Mods.ModName.Category.ContentName.DataName`
* Contributing translations, directly or through translation mods, has been streamlined.

**Porting Notes**:    
* Modders **MUST** follow the **Migrating from 1.4.3 to 1.4.4** section in the [localization guide on the wiki](https://github.com/tModLoader/tModLoader/wiki/Localization#migrating-from-143-to-144) prior to switching to `1.4.4-preview`.
* Modders with hardcoded values in localization files, such as "10% increased melee damage", "5 armor penetration", etc should consider updating their code to have less duplication by following the [Binding Values to Localizations](https://github.com/tModLoader/tModLoader/wiki/Localization#binding-values-to-localizations) section on the wiki.
  * [ExampleStatBonusAccessory](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Accessories/ExampleStatBonusAccessory.cs) showcases our recommended approach for writing clean and maintainable item tooltips. In the example, all the numbers denoting the effects of the accessory only exist in code, not in both code and translation files. Most notably with this new approach, there is no risk of the tooltip and actual behavior being out of sync. Previously a modder could update an accessory but forget to update the tooltip, or forget to update tooltips for other languages, resulting in items that behave differently than how they claim to behave.
  * [AbsorbTeamDamageBuff](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Buffs/AbsorbTeamDamageBuff.cs), [AbsorbTeamDamageAccessory](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Accessories/AbsorbTeamDamageAccessory.cs), and [ExampleDamageModificationPlayer](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Common/Players/ExampleDamageModificationPlayer.cs) further build on this idea. In these examples, the actual damage absorption stat, `DamageAbsorptionPercent` (set to 30), only exists in `AbsorbTeamDamageAccessory`. `AbsorbTeamDamageBuff` references that value for the buff tooltip via `public override LocalizedText Description => base.Description.WithFormatArgs(AbsorbTeamDamageAccessory.DamageAbsorptionPercent);`. `AbsorbTeamDamageAccessory` also references the value in it's own tooltip. `ExampleDamageModificationPlayer` also references that value in `ModifyHurt` and `OnHurt` via the `DamageAbsorptionMultiplier` property. If the modder wished to change this accessory to absorb 35% of damage instead of 30% of damage, the modder would only need to change the value in 1 place, rather than several places in multiple `.cs` and `.hjson` files. This shows the power of properly using localization files and binding values to them.
* `ModConfig` is now localized by default, see the porting notes section of [PR #3302](https://github.com/tModLoader/tModLoader/pull/3302) for porting information specific to `ModConfig`.

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
* [ExampleWhipDebuff](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Buffs/ExampleWhipDebuff.cs) and ExampleWhipAdvancedDebuff: Show proper tag damage, both flat and scaling varieties.
* [PartyZombie](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/NPCs/PartyZombie.cs): Shows an NPC that is "resistant" to a specific damage class, taking less damage
* [ExampleAdvancedFlailProjectile](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Projectiles/ExampleAdvancedFlailProjectile.cs): Shows ModifyDamageScaling replacement, as well as proper usage of various `HitModifiers` fields and properties to scale damage and knockback dynamically.
* [ExamplePaperAirplaneProjectile](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Projectiles/ExamplePaperAirplaneProjectile.cs): Shows a projectile dealing bonus damage to an npc that is weak to it.

tModPorter will be able to convert the hook signatures, but you will need to read the Porting Notes (in particular the FAQ), to figure out how to convert old damage modification code into the new system.

**Porting Notes:**    
Too many to write here, please read the PR descriptions if you have questions after running tModPorter.

### Shop Changes (aka Declarative Shops)
[PR 3219](https://github.com/tModLoader/tModLoader/pull/3219) has changed how NPC shops are done.

**Short Summary:**    
* NPC shops are now declarative, meaning they get registered, and items can be added to them with conditions, similarly to NPC drops (loot) and recipes    
* Adding items to shops, or hiding items from shops is now as easy as adding or disabling recipes    
* Info mods can traverse the NPCShopDatabase to show how to obtain an item. All the conditions for items appearing in shops have been localized.    
* Registering multiple shops per NPC is perfectly fine, and selecting the shop to be opened when chatting can now be done via the ref string shop parameter in OnChatButtonClicked

**Porting Notes:**    
* `ModPylon.IsPylonForSale` has been replaced by `ModPylon.GetNPCShopEntry`. Please see the documentation and [ExamplePylonTile](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Tiles/ExamplePylonTile.cs)    
* `Mod/GlobalNPC.SetupShop` is now `ModifyActiveShop`. This hook still exists to allow for custom shop modification, but you should consider moving to the new `ModNPC.AddShops` and `GlobalNPC.ModifyShop` hooks    
* The `Item[]` in `ModifyActiveShop` will now contain null entries. This distinguishes slots which have not been filled, from slots which are intentionally left empty (as in the case of DD2 Bartender)    
* Please take a look at the changes to Example Mod in the PR to more easily understand the new system.    

### Tile Drop Changes
[PR 3210](https://github.com/tModLoader/tModLoader/pull/3210) has changed how tiles drop items.

**Short Summary:**    
* Tiles and walls now automatically drop the items that place it. This process supports tiles with multiple styles.
* Block Swap feature now supports modded torches, chests, and drawers. 
* Other miscellaneous fixes. 

**Porting Notes:**    
* The vast majority of `ModTile.KillMultitile` code and `ModTile.Drop` code can be deleted or simplified by following the porting notes.
* All mods with any `ModTile` must follow the porting notes, failure to adjust your code for this major change will result in duplicate drops.
* There are too many changes to write here. Please read the Porting Notes section in the [Pull Request](https://github.com/tModLoader/tModLoader/pull/3210) after running tModPorter.

### Improve Player.clientClone performance
[PR 3174](https://github.com/tModLoader/tModLoader/pull/3174) has added new approaches to `Player.clientClone` to improve performance.

**Short Summary:**   
`Item.Clone` can become very performance expensive with many mods. Only type, stack and prefix are required to tell if an item has changed in the inventory and needs to be re-synced.

This PR replaces usages of `Item.Clone` in `Player.clientClone` with `Item.CopyNetStateTo`. Additionally, a single Player (and `ModPlayer`) instance is reused for all `clientClone`/`CopyClientStateTo` calls, acting as a 'storage copy' rather than making a new one each frame.

Please note that tModPorter is not smart enough to identify `Item.Clone` usages in `ModPlayer.CopyClientStateTo` calls automatically. You will need to replace these yourself or an exception will be thrown at runtime.

**Porting Notes:**    
* `ModPlayer.clientClone` -> `ModPlayer.CopyClientState`
* Use `Item.CopyNetStateTo` instead of `Item.Clone` in `ModPlayer.CopyClientState`
* Use `Item.IsNetStateDifferent` instead of `Item.IsNotSameTypePrefixAndStack` in `ModPlayer.SendClientChanges`
* Item instances altered by `CopyNetStateTo` are not initialized! Do not attempt to read properties or retrieve `ModItem` or `GlobalItem` instances from them! The only valid operation on an `Item` which was updated with `CopyNetStateTo` is `IsNetStateDifferent`

[HoldStyleShowcase](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/HoldStyleShowcase.cs), [HitModifiersShowcase](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Weapons/HitModifiersShowcase.cs), and [UseStyleShowcase](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/UseStyleShowcase.cs) briefly show usage of `Item.NetStateChanged();` to trigger an item to sync.

### Max Health and Mana Manipulation API
[PR 2909](https://github.com/tModLoader/tModLoader/pull/2909) greatly changed the API for modifying the player's max health and mana, rendering custom sprites over the player's hearts and mana stars and even added an API for creating custom display sets.

**Short Summary:**
* Adds `ModPlayer.ModifyMaxStats` with `StatModifier` arguments for permanent adjustments to max health/mana
  * While this hook is intended to be used for permanent stat adjustments, it can also easily support temporary adjustments as well
  * However, do note that this hook runs very early in the player update logic.  It runs shortly after `ModPlayer.ResetEffects`
    * Temporary adjustments that rely on buffs or other player variables affected by armor, accessories, etc. should still use the old method of modifying `Player.statLifeMax2` in one of the update hooks in `ModPlayer`
* Adds `Player.ConsumedLifeCrystals`, `ConsumedLifeFruit` and `ConsumedManaCrystals` properties
  * These properties are directly tied to the vanilla stat increases and can also be manually set by modders
* Adds helper methods `Player.UseHealthMaxIncreasingItem` and `Player.UseManaMaxIncreasingItem` for displaying the visual effects
* Adds `ModResourceDisplaySet` allowing for custom life/mana draw styles (similar to boss bar styles) that can be selected in settings
* Adds `ModResourceOverlay` to allow for drawing custom hearts/mana/effects over the vanilla (or modded) life/mana UI elements

ExampleMod contains examples for a [custom display set](https://github.com/tModLoader/tModLoader/tree/1.4.4/ExampleMod/Common/UI/ExampleDisplaySets) and [custom resource overlays](https://github.com/tModLoader/tModLoader/tree/1.4.4/ExampleMod/Common/UI/ResourceOverlay).

[ExampleStatIncreasePlayer](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Common/Players/ExampleStatIncreasePlayer.cs), [ExampleLifeFruit](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Consumables/ExampleLifeFruit.cs) and [ExampleManaCrystal](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Consumables/ExampleManaCrystal.cs) showcase how to handle permanent stat increases.

**Porting Notes:**
* Refer to `ExampleStatIncreasePlayer`, `ExampleLifeFruit` and `ExampleManaCrystal` on how to properly set permanent stat increases
* Temporary stat increases to `Player.statLifeMax2` and `Player.statManaMax2` can still be performed as usual
  * Though they should not be changed in `ModPlayer.ModifyMaxStats`, since that runs too early for the changes to be kept
* Use `Player.ConsumedLifeCrystals`, `Player.ConsumedLifeFruit` and `Player.ConsumedManaCrystals` instead of checking `Player.statLifeMax` or `Player.statManaMax` due to the stat fields being adjustable by mods

### Extra Jump API
[PR 3552](https://github.com/tModLoader/tModLoader/pull/3552) added a proper API for implementing and modifying extra mid-air jumps.

**Short Summary:**
* Adds `ExtraJump`, a singleton type representing an extra jump that is controlled via the `ExtraJumpState` structure
  * The `ExtraJumpState` object for an extra jump can be obtained via the `GetJumpState` methods in `Player`
  * Extra jumps can be disabled for the "current update tick" or consumed entirely
    * **NOTE:** `ExtraJumpState.Disable()` will consume the extra jump if it is available and prematurely stops it if it's currently active
* Adds new methods related to checking the state of the player's extra jumps to `Player`
  * Also includes a `blockExtraJumps` field, which prevents any extra jumps from being used, but does not stop the currently active jump nor consume any remaining extra jumps
* Flipper swimming is now considered an "extra jump" and can be accessed via `Player.GetJumpState(ExtraJump.Flipper)`
* `Player.sandStorm` is now more directly related to the state of the *Sandstorm in a Bottle* extra jump

ExampleMod contains examples for [simple](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Accessories/ExampleExtraJumpAccessory.cs) and [complex](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Content/Items/Accessories/ExampleMultiExtraJumpAccessory.cs) extra jumps, as well as an example for [modifying an extra jump](https://github.com/tModLoader/tModLoader/blob/1.4.4/ExampleMod/Common/Players/ExampleExtraJumpModificationPlayer.cs).

The PR's original comment contains snippets for enabling/disabling an extra jump, temporarily disabling extra jumps in a `ModBuff` and changing the horizontal movement modifications for an extra jump.

**Porting Notes:**
* `Player.hasJumpOption_X`, `Player.canJumpAgain_X` and `Player.isPerformingJump_X` for all vanilla jumps are now accessed via the `ExtraJumpState` for the respective extra jump
  * If you were setting one or more of these fields to `false` in order to disable the extra jump, use `ExtraJumpState.Disable()` instead
  * If you were disabling all extra jumps:
    * Set `Player.blockExtraJumps = true;` for temporary disabling
    * Call `Player.ConsumeAllExtraJumps()` (optionally followed by `Player.StopExtraJumpInProgress()`) for permanent disabling until the player lands
* `Player.accFlipper` and `Player.sandStorm` are now properties, so any code that was using them will have to be rebuilt
  * For `Player.accFlipper` in particular, reading `accFlipper` is prohibited; use `Player.GetJumpState(ExtraJump.Flipper).Available` instead

## Smaller Changes
### [PR 3063](https://github.com/tModLoader/tModLoader/pull/3063): Fix Quick Heal and Quick Mana consuming non-consumables
**Short Summary:** Adds `item.consumable` as a check in Quick Heal and Quick Mana.     
**Porting Notes:** For Quick heal, Quick Mana non-consummables, it is recommended to use `Item.consumable` field instead of overriding `ConsumeItem()`

### [PR 3360](https://github.com/tModLoader/tModLoader/pull/3360): Add ModSystem.ClearWorld
**Short Summary:** Adds ModSystem.ClearWorld which runs on world clear. The new best place to clear/initialize world related data structures.    
**Porting Notes:** Consider replacing overrides of `OnWorldLoad` with `ClearWorld` to reset world related data. Generally no need to override `OnWorldUnload` anymore. Use Unload if you want to fully clean up all memory, but empty lists are nothing to worry about.

### [PR 3341](https://github.com/tModLoader/tModLoader/pull/3341): Unify Localized Conditions
**Short Summary:** `Terraria.Recipe.Condition` has been moved to `Terraria.Condition` and can now be applied to more things. Recipes, item variants, drops and shops. Added `SimpleItemDropRuleCondition` class to help make drop conditions more easily.    
**Porting Notes:** `Terraria.Recipe.Condition` -> `Terraria.Condition` (tModPorter). `Recipe` parameter removed from condition delegate, since it was almost always unused. Custom conditions will have to change from `_ => calculation` or `recipe => calculation` to `() => calculation`

### [Commit 56f6657](https://github.com/tModLoader/tModLoader/commit/56f66570219a66095120e64e7f0640d6ba0a6999): Change HookGen Namespace Style
**Short Summary:**     
* Hookgen namespaces (`IL.` and `On.`) have been removed in favor of `On_` and `IL_` prepended to the type name.
* No longer will you get 3 different namespace suggestions when you use VS to import a Terraria type name.
* Want On Item hooks? Just use `On_Item` it's in the same namespace as `Item`!    

**Porting Notes:**     
* `On.Terraria.Player` -> `Terraria.On_Player`
* `IL.Terraria.Player` -> `Terraria.IL_Player`
* As usual, tModPorter can convert your old code automatically.

### [PR 3242](https://github.com/tModLoader/tModLoader/pull/3242): Default Research Unlock Value changed to 1
**Short Summary:**     
* All items will now default to needing 1 item to research. 
* The previous value of 0 left items unresearchable since many modders don't bother to implement journey mode features
* Modders can clean up their code:
  * `ModItem.SacrificeTotal` has been removed and tModPorter should have changed it to `Item.ResearchUnlockCount` already.
  * `CreativeItemSacrificesCatalog.Instance.SacrificeCountNeededByItemId[Type] = #;` lines can be changed to `Item.ResearchUnlockCount = #;`
  * `ModItem`s with `Item.ResearchUnlockCount = 1;` or `CreativeItemSacrificesCatalog.Instance.SacrificeCountNeededByItemId[Type] = 1;` lines can be deleted, since the default is 1.

**Porting Notes:**    
* This is almost entirely optional. The only thing is `ModItem`s that should never exist in the inventory, or should otherwise not be researchable, now need `Item.ResearchUnlockCount = 0;` added where previously, they would be unsearchable by default. (Note: `CreativeItemSacrificesCatalog.Instance.SacrificeCountNeededByItemId[Type] = 0;` will result in bugs, use `Item.ResearchUnlockCount = 0;`)
* Modders might consider using this opportunity to add research values matching vanilla values to their `ModItem`: [Journey Mode Research wiki page](https://terraria.wiki.gg/wiki/Journey_Mode#Research).


