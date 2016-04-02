# ItemLoader

This serves as the central class from which item-related functions are carried out. It also stores a list of mod items by ID.

## Methods

Note that all calls to ModItem methods only take place if the item is a modded item.

### public static ModItem GetItem(int type)

Gets the ModItem instance corresponding to the specified type. Returns null if no modded item has the given type.

### public static bool CanUseItem(Item item, Player player)

Returns the "and" operation on the results of ModItem.CanUseItem and all GlobalItem.CanUseItem hooks.

### public static void UseStyle(Item item, Player player)

Calls ModItem.UseStyle and all GlobalItem.UseStyle hooks.

### public static void HoldStyle(Item item, Player player)

If the player is not holding onto a rope and is not in the middle of using an item, calls ModItem.HoldStyle and all GlobalItem.HoldStyle hooks.

### public static void HoldItem(Item item, Player player)

Calls ModItem.HoldItem and all GlobalItem.HoldItem hooks.

### public static void GetWeaponDamage(Item item, Player player, ref int damage)

Calls ModItem.GetWeaponDamage, then all GlobalItem.GetWeaponDamage hooks.

### public static void GetWeaponKnockback(Item item, Player player, ref float knockback)

Calls ModItem.GetWeaponKnockback, then all GlobalItem.GetWeaponKnockback hooks.

### public static void CheckProjOnSwing(Player player, Item item, ref bool canShoot)

If the item is a modded item, ModItem.checkProjOnSwing is true, and the player is not at the beginning of the item's use animation, sets canShoot to false.

### public static bool ConsumeAmmo(Item item, Item ammo, Player player)

Calls ModItem.ConsumeAmmo for the weapon, ModItem.ConsumeAmmo for the ammo, then each GlobalItem.ConsumeAmmo hook for the weapon and ammo, until one of them returns false. If all of them return true, returns true.

### public static bool Shoot(Item item, Player player, ref Vector2 position, ref float speedX, ref float speedY, ref int type, ref int damage, ref float knockBack)

Calls each GlobalItem.Shoot hook, then ModItem.Shoot, until one of them returns false. If all of them return true, returns true.

### public static void UseItemHitbox(Item item, Player player, ref Rectangle hitbox, ref bool noHitbox)

Calls ModItem.UseItemHitbox, then all GlobalItem.UseItemHitbox hooks.

### public static void MeleeEffects(Item item, Player player, Rectangle hitbox)

Calls ModItem.MeleeEffects and all GlobalItem.MeleeEffects hooks.

### public static bool? CanHitNPC(Item item, Player player, NPC target)

Gathers the results of ModItem.CanHitNPC and all GlobalItem.CanHitNPC hooks. If any of them returns false, this returns false. Otherwise, if any of them returns true then this returns true. If all of the returns null, this returns null.

### public static void ModifyHitNPC(Item item, Player player, NPC target, ref int damage, ref float knockBack, ref bool crit)

Calls ModItem.ModifyHitNPC, then all GlobalItem.ModifyHitNPC hooks.

### public static void OnHitNPC(Item item, Player player, NPC target, int damage, float knockBack, bool crit)

Calls ModItem.OnHitNPC and all GlobalItem.OnHitNPC hooks.

### public static bool CanHitPvp(Item item, Player player, Player target)

Calls all GlobalItem.CanHitPvp hooks, then ModItem.CanHitPvp, until one of them returns false. If all of them return true, this returns true.

### public static void ModifyHitPvp(Item item, Player player, Player target, ref int damage, ref bool crit)

Calls ModItem.ModifyHitPvp, then all GlobalItem.ModifyHitPvp hooks.

### public static void OnHitPvp(Item item, Player player, Player target, int damage, bool crit)

Calls ModItem.OnHitPvp and all GlobalItem.OnHitPvp hooks.

### public static bool UseItem(Item item, Player player)

Returns the "or" operation on the results of ModItem.UseItem and all GlobalItem.UseItem hooks.

### public static void ConsumeItem(Item item, Player player, ref bool consume)

If ModItem.ConsumeItem or any of the GlobalItem.ConsumeItem hooks returns false, sets consume to false.

### public static bool UseItemFrame(Item item, Player player)

Calls ModItem.UseItemFrame, then all GlobalItem.UseItemFrame hooks, until one of them returns true. Returns whether any of the hooks returned true.

### public static bool HoldItemFrame(Item item, Player player)

Calls ModItem.HoldItemFrame, then all GlobalItem.HoldItemFrame hooks, until one of them returns true. Returns whether any of the hooks returned true.

### public static bool AltFunctionUse(Item item, Player player)

Calls ModItem.AltFunctionUse, then all GlobalItem.AltFunctionUse hooks, until one of them returns true. Returns whether any of the hooks returned true.

### public static void UpdateInventory(Item item, Player player)

Calls ModItem.UpdateInventory and all GlobalItem.UpdateInventory hooks.

### public static void UpdateEquip(Item item, Player player)

Calls ModItem.UpdateEquip and all GlobalItem.UpdateEquip hooks.

### public static void UpdateAccessory(Item item, Player player, bool hideVisual)

Calls ModItem.UpdateAccessory and all GlobalItem.UpdateAccessory hooks.

### public static void UpdateVanity(Player player)

Calls each of the item's equipment texture's UpdateVanity hook.

### public static void UpdateArmorSet(Player player, Item head, Item body, Item legs)

If the head's ModItem.IsArmorSet returns true, calls the head's ModItem.UpdateArmorSet. This is then repeated for the body, then the legs. Then for each GlobalItem, if GlobalItem.IsArmorSet returns a non-empty string, calls GlobalItem.UpdateArmorSet with that string.

### public static void PreUpdateVanitySet(Player player)

If the player's head texture's IsVanitySet returns true, calls the equipment texture's PreUpdateVanitySet. This is then repeated for the player's body, then the legs. Then for each GlobalItem, if GlobalItem.IsVanitySet returns a non-empty string, calls GlobalItem.PreUpdateVanitySet, using player.head, player.body, and player.legs.

### public static void UpdateVanitySet(Player player)

If the player's head texture's IsVanitySet returns true, calls the equipment texture's UpdateVanitySet. This is then repeated for the player's body, then the legs. Then for each GlobalItem, if GlobalItem.IsVanitySet returns a non-empty string, calls GlobalItem.UpdateVanitySet, using player.head, player.body, and player.legs.

### public static void ArmorSetShadows(Player player, ref bool longTrail, ref bool smallPulse, ref bool largePulse, ref bool shortTrail)

If the player's head texture's IsVanitySet returns true, calls the equipment texture's ArmorSetShadows. This is then repeated for the player's body, then the legs. Then for each GlobalItem, if GlobalItem.IsVanitySet returns a non-empty string, calls GlobalItem.ArmorSetShadows, using player.head, player.body, and player.legs.

### public static void SetMatch(int armorSlot, int type, bool male, ref int equipSlot, ref bool robes)

Calls ModItem.SetMatch, then all GlobalItem.SetMatch hooks.

### public static bool CanRightClick(Item item)

Calls ModItem.CanRightClick, then all GlobalItem.CanRightClick hooks, until one of the returns true. If one of the returns true, returns Main.mouseRight. Otherwise, returns false.

### public static void RightClick(Item item, Player player)

If Main.mouseRightRelease is true, the following steps are taken:  
1. Call ModItem.RightClick  
2. Calls all GlobalItem.RightClick hooks  
3. Decrements the item's stack  
4. Sets the item's type to 0 if the item's stack is 0  
5. Plays the item-grabbing sound  
6. Sets Main.stackSplit to 30  
7. Sets Main.mouseRightRelease to false  
8. Calls Recipe.FindRecipes.

### public static bool IsModBossBag(Item item)

Returns whether ModItem.bossBagNPC is greater than 0. Returns false if item is not a modded item.

### public static void OpenBossBag(int type, Player player, ref int npc)

If the item is a modded item and ModItem.bossBagNPC is greater than 0, calls ModItem.OpenBossBag and sets npc to ModItem.bossBagNPC.

### public static bool PreOpenVanillaBag(string context, Player player, int arg)

Calls each GlobalItem.PreOpenVanillaBag hook until one of them returns false. Returns true if all of them returned true.

### public static void OpenVanillaBag(string context, Player player, int arg)

Calls all GlobalItem.OpenVanillaBag hooks.

### public static void DrawHands(Player player, ref bool drawHands, ref bool drawArms)

Calls the item's body equipment texture's DrawHands hook, then all GlobalItem.DrawHands hooks.

### public static void DrawHair(Player player, ref bool drawHair, ref bool drawAltHair)

Calls the item's head equipment texture's DrawHair hook, then all GlobalItem.DrawHair hooks.

### public static bool DrawHead(Player player)

Calls the item's head equipment texture's DrawHead hook, then all GlobalItem.DrawHead hooks, until one of them returns false. Returns true if none of them return false.

### public static bool DrawBody(Player player)

Calls the item's body equipment texture's DrawBody hook, then all GlobalItem.DrawBody hooks, until one of them returns false. Returns true if none of them return false.

### public static bool DrawLegs(Player player)

Calls the item's leg equipment texture's DrawLegs hook, then the item's shoe equipment texture's DrawLegs hook, then all GlobalItem.DrawLegs hooks, until one of them returns false. Returns true if none of them return false.

### public static void DrawArmorColor(EquipType type, int slot, Player drawPlayer, float shadow, ref Color color, ref int glowMask, ref Color glowMaskColor)

Calls the item's equipment texture's DrawArmorColor hook, then all GlobalItem.DrawArmorColor hooks.

### public static void ArmorArmGlowMask(int slot, Player drawPlayer, float shadow, ref int glowMask, ref Color color)

Calls the item's body equipment texture's ArmorArmGlowMask hook, then all GlobalItem.ArmorArmGlowMask hooks.

### public static Item GetWing(Player player, bool social = false)

Returns the wing item that the player is functionally using. If player.wingsLogic has been modified, so no equipped wing can be found to match what the player is using, this creates a new Item object to return.

### public static void VerticalWingSpeeds(Player player, ref float ascentWhenFalling, ref float ascentWhenRising, ref float maxCanAscendMultiplier, ref float maxAscentMultiplier, ref float constantAscend)

If the player is using wings, this uses the result of GetWing, and calls ModItem.VerticalWingSpeeds then all GlobalItem.VerticalWingSpeeds hooks.

### public static void HorizontalWingSpeeds(Player player)

If the player is using wings, this uses the result of GetWing, and calls ModItem.HorizontalWingSpeeds then all GlobalItem.HorizontalWingSpeeds hooks.

### public static void WingUpdate(Player player, bool inUse)

If wings can be seen on the player, calls the player's wing's equipment texture's WingUpdate and all GlobalItem.WingUpdate hooks.

### public static void Update(Item item, ref float gravity, ref float maxFallSpeed)

Calls ModItem.Update, then all GlobalItem.Update hooks.

### public static void PostUpdate(Item item)

Calls ModItem.PostUpdate and all GlobalItem.PostUpdate hooks.

### public static void GrabRange(Item item, Player player, ref int grabRange)

Calls ModItem.GrabRange, then all GlobalItem.GrabRange hooks.

### public static bool GrabStyle(Item item, Player player)

Calls all GlobalItem.GrabStyle hooks then ModItem.GrabStyle, until one of them returns true. Returns whether any of the hooks returned true.

### public static bool OnPickup(Item item, Player player)

Calls all GlobalItem.OnPickup hooks then ModItem.OnPickup, until one of the returns false. Returns true if all of the hooks return true.

### public static Color? GetAlpha(Item item, Color lightColor)

Calls all GlobalItem.GetAlpha hooks then ModItem.GetAlpha, until one of them returns a color, and returns that color. Returns null if all of the hooks return null.

### public static bool PreDrawInWorld(Item item, SpriteBatch spriteBatch, Color lightColor, Color alphaColor, ref float rotation, ref float scale)

Returns the "and" operator on the results of ModItem.PreDrawInWorld and all GlobalItem.PreDrawInWorld hooks.

### public static void PostDrawInWorld(Item item, SpriteBatch spriteBatch, Color lightColor, Color alphaColor, float rotation, float scale)

Calls ModItem.PostDrawInWorld, then all GlobalItem.PostDrawInWorld hooks.

### public static bool PreDrawInInventory(Item item, SpriteBatch spriteBatch, Vector2 position, Rectangle frame, Color drawColor, Color itemColor, Vector2 origin, float scale)

Returns the "and" operator on the results of all GlobalItem.PreDrawInInventory hooks and ModItem.PreDrawInInventory.

### public static void PostDrawInInventory(Item item, SpriteBatch spriteBatch, Vector2 position, Rectangle frame, Color drawColor, Color itemColor, Vector2 origin, float scale)

Calls ModItem.PostDrawInInventory, then all GlobalItem.PostDrawInInventory hooks.

### public static void HoldoutOffset(float gravDir, int type, ref Vector2 offset)

### public static void HoldoutOrigin(Player player, ref Vector2 origin)

### public static bool CanEquipAccessory(Item item, int slot)

### public static void ExtractinatorUse(ref int resultType, ref int resultStack, int extractType)

### public static void AutoLightSelect(Item item, ref bool dryTorch, ref bool wetTorch, ref bool glowstick)

### public static void CaughtFishStack(Item item)

### public static void IsAnglerQuestAvailable(int itemID, ref bool notAvailable)

### public static void AnglerChat(bool turningInFish, bool anglerQuestFinished, int type, ref string chat, ref string catchLocation)

### public static void OnCraft(Item item, Recipe recipe)