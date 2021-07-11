Player item animation seems simple at first, but it begs a few questions.  
Notably, how does the Clockwork Assault rifle work like it does?  Why can't you switch items for a short while after using some weapons?  

All of these questions will be answered below.

## itemTime, itemAnimation and reuseDelay
In order to keep track of when the player is using an item, the game uses three variables in `Player`: `itemTime`, `itemAnimation` and `reuseDelay`.

### itemTime
`player.itemTime` determines how long the current "use" for the player's held item will last for.  When this value reaches zero and `player.itemAnimation` hasn't reached zero yet, then the use code for the item will repeat.

Most codes in vanilla, including tile placing, first check if `player.itemTime == 0` (among other conditions), then set `player.itemTime = player.useTime;` (give or take a call to `PlayerHooks.TotalUseTime()` or use speed modifiers such as `player.wallSpeed`) if that was true.  
After that, the actual "use code" for the item will run and the game will wait until `player.itemTime == 0 && player.itemAnimation > 0` is true (among other conditions depending on the item).

Example (`item.shoot > 0` code):
```cs
if (item.type == 2223)
    shoot = 357;

itemTime = PlayerHooks.TotalUseTime(item.useTime, this, item);
Vector2 vector = RotatedRelativePoint(MountedCenter);
bool flag9 = false;
```

### itemAnimation
`player.itemAnimation`, in conjunction with `player.itemAnimationMax`, is used to determine how far along in the use animation the player is in.  
Any item use is wrapped around a check of `player.itemAnimation > 0`, so this field is what drives the use.

**Noteworthy Cases:**
- If the player's held item's `useTime` is **equal to** its `useAnimation`, nothing spetacular happens.  
- If the player's held item's `useTime` is **less than** its `useAnimation`, the item use code will be called multiple times during the animation due that was mentioned in the previous sub-section.  
- If the player's held item's `useTime` is **greater than** its `useAnimation`, then the player will be unable to switch items nor use the item again until `player.itemTime` has reached zero, which would happen *after* the use animation finishes.
  - However, if `player.reuseDelay` is greater than `0` when `player.itemAnimation` reaches 0 in this case, the leftover timer from `player.itemTime` will be overwritten.

Example (`item.shoot > 0` check):
```cs
flag8 = flag8 && ItemLoader.CheckProjOnSwing(this, item);
if (item.shoot > 0 && itemAnimation > 0 && itemTime == 0 && flag8) {
    int shoot = item.shoot;
    // ...
```

For reference, this is what `ItemLoader.CheckProjOnSwing(Player, Item)` does:
```cs
public static bool CheckProjOnSwing(Player player, Item item){
    return item.modItem == null || !item.modItem.OnlyShootOnSwing || player.itemAnimation == player.itemAnimationMax - 1;
}
```
Basically, it makes sure that the player is just starting the item use animation.  
Items that use `item.useStyle = 5;` bypass the above method call.

### reuseDelay
`player.reuseDelay` is the intended way to force the player to wait a bit longer after using an item (via `item.reuseDelay`).  
However, for any items where `item.melee`, `item.createTile > 0` or `item.createWall > 0` is true, this field **is ignored**.

If `item.reuseDelay > 0` is true for the player's held item, the following code is ran just before the check for if the player just clicked the "use item" button (usually Left Mouse):
```cs
// Found in Player.ItemCheck(int)
if (itemAnimation == 0 && reuseDelay > 0){
    itemAnimation = reuseDelay;
    itemTime = reuseDelay;
    reuseDelay = 0;
}
```

Thus, the player has to wait this additional time *and* an additional item use doesn't occur since `player.itemAnimation > 0 && player.itemTime == 0` will never be true after the above code snippet runs.

# Other Information
> How does the Clockwork Assault rifle work like it does?

The **Clockwork Assault Rifle** item has the following code in its defaults:
```cs
useAnimation = 12;
useTime = 4;
reuseDelay = 14;
```
Due to `useTime` being less than `useAnimation`, `player.itemTime` will end up looping `12 / 4 = 3` times before forcing the item animation to continue for `14` frames.

> Why can't you switch items for a short while after using some weapons?

In `Player.Update(int)`, the condition `player.itemAnimation == 0 && player.itemTime == 0 && player.reuseDelay == 0` must be true before checking the triggers in `PlayerInput.Triggers.Current` for the 10 hotbar slots.
```cs
if (PlayerInput.Triggers.Current.Hotbar1) {
    selectedItem = 0;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar2) {
    selectedItem = 1;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar3) {
    selectedItem = 2;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar4) {
    selectedItem = 3;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar5) {
    selectedItem = 4;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar6) {
    selectedItem = 5;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar7) {
    selectedItem = 6;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar8) {
    selectedItem = 7;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar9) {
    selectedItem = 8;
    flag7 = true;
}

if (PlayerInput.Triggers.Current.Hotbar10) {
    selectedItem = 9;
    flag7 = true;
}
```

On the other hand, if that condition was false, the game instead checks the number keys on the main keyboard (`D0` through `D9`) specifically instead of the triggers set.
```cs
if (Main.keyState.IsKeyDown(D1)) {
    selectedItem = 0;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D2)) {
    selectedItem = 1;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D3)) {
    selectedItem = 2;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D4)) {
    selectedItem = 3;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D5)) {
    selectedItem = 4;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D6)) {
    selectedItem = 5;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D7)) {
    selectedItem = 6;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D8)) {
    selectedItem = 7;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D9)) {
    selectedItem = 8;
    flag10 = true;
}

if (Main.keyState.IsKeyDown(D0)) {
    selectedItem = 9;
    flag10 = true;
}
```