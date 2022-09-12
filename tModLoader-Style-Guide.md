The number one rule of collaborative development is _pick a style and stick to it_. The following style guidelines should apply to all future contributions to tModLoader. Please be mindful of past developers and don't reformat code just for the sake of it.

The following are useful resources for general C# design and style
* https://google.github.io/styleguide/csharp-style.html
* https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/
* https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md

If something is not listed in this guide, use your best judgement to pick between matching existing code and the advice listed above.

## Identifier Names
Use `camelCase` for
* local variables
* method parameters
* private fields

Use `PascalCase` for everything else.

> _**Why: With the exception of method parameters, camelCase indicates a variable that is not part of any public APIs.**_

Use `_camelCase` for 'backing fields' on properties. Private fields may also optionally be prefixed with an _.

> _**Why: `_` indicates the field is an implementation detail of specific members, and not key to the overall function of the class.**_

Classes (and structures that contain logic) should not contain non-private fields, and should instead use properties everywhere. Structures that do not contain logic and are only used as data are better off using fields instead.

> _**Why: APIs' implementation details are always likely to change in the long future. Using properties and auto-properties allows us to reduce compatibility breaks from such changes. Using fields in the context of data structures allows for using reference variables. Terraria itself rarely uses properties. Microsoft guidelines recommend avoiding public fields entirely.**_

## Braces
Use [K&R Style](https://en.wikipedia.org/wiki/Indentation_style#K&R_style). Braces on the same line for statements, method and property declarations. New line for all other declarations. This is mostly enforced by .editorconfig

Braces may be omitted on _single_ line `if/else/using` statements. For example:
```cs
if (canRestoreFlag) {
    for (int k = 0; k < canRestore.Count; k++) {
        if (canRestore[k] > 0)
            infos[k] = null;
    }
}
```

The following is **forbidden**

```cs
if (canRestoreFlag) // Body spans multiple lines
    for (int k = 0; k < canRestore.Count; k++) // Braces required on for loops
        if (canRestore[k] > 0) // Ok
            infos[k] = null;
```

**Do not** mix and match missing braces
```cs
if (ConfigManager.AnyModNeedsReload()) // Forbidden. Must have braces to match `else`
	needsReload = true;
else {
	foreach (NetConfig pendingConfig in pendingConfigs)
		ConfigManager.GetConfig(pendingConfig).OnChanged();
}
```

**Exception**: ExampleMod contributions; _single_ line statements and method bodies should receive braces. Only properties (i.e. `bool SomeProperty => true;`) can be kept as defined above.

_**Why: Balance between good visual separation and keeping code on-screen when Terraria is full of small control flow statements.**_

## Comments
Short comments about a single line should go on the same line.
```cs
public override void SetDefaults() {
	item.damage = 12; // The damage for projectiles isn't actually 12, it actually is the damage combined with the projectile and the item together.
	item.DamageType = DamageClass.Ranged;
	...
}
```
**Do not** use block comments. Use multiple line comments instead
```cs
public override bool GetDefaultVisiblity(PlayerDrawSet drawInfo) {
	// The layer will be visible only if the player is holding an ExampleItem in their hands. Or if another modder forces this layer to be visible.
	return drawInfo.drawPlayer.HeldItem?.type == ModContent.ItemType<ExampleItem>();

	// If you'd like to reference another PlayerDrawLayer's visibility,
	// you can do so by getting its instance via ModContent.GetInstance<OtherDrawLayer>(), and calling GetDefaultVisiblity on it
}
```

## Member Order
Follow the general order: inner classes, static members, fields, properties, constructors, methods. Group fields and methods by function. Start with the largest/most used features and end with the smallest.

```cs
public class TmodFile : IEnumerable<TmodFile.FileEntry>
{
	public class FileEntry
	{
		...
	}

	public const float CompressionTradeoff = 0.9f;

	private static string Sanitize(string path) => path.Replace('\\', '/');

	public readonly string path;

	private FileStream fileStream;
	private IDictionary<string, FileEntry> files = new Dictionary<string, FileEntry>();
	private FileEntry[] fileTable;

	...
	
	private bool? _validModBrowserSignature;
	internal bool ValidModBrowserSignature {
		get { ... }
	}

	internal TmodFile(string path, string name = null, Version version = null) { ... }

	public bool HasFile(string fileName) {...}
	public byte[] GetBytes(FileEntry entry) {...}
	public byte[] GetBytes(string fileName) {...}
	
	...
	
	internal void AddFile(string fileName, byte[] data) {...}
	internal void RemoveFile(string fileName) {...}
	
	...
}
```

Backing fields may be declared right before the corresponding properties.
```cs
private readonly List<PlayerDrawLayer> _childrenBefore = new List<PlayerDrawLayer>();
public IReadOnlyList<PlayerDrawLayer> ChildrenBefore => _childrenBefore;

private readonly List<PlayerDrawLayer> _childrenAfter = new List<PlayerDrawLayer>();
public IReadOnlyList<PlayerDrawLayer> ChildrenAfter => _childrenAfter;
```

Supporting 'lookup fields' can go with the methods they serve.
```cs
private static readonly char[] nameSplitters = new char[] { '/', ' ', ':' };
public static void SplitName(string name, out string domain, out string subName) {
	int slash = name.IndexOfAny(nameSplitters); // slash is the canonical splitter, but we'll accept space and colon for backwards compatability, just in case
	if (slash < 0)
		throw new MissingResourceException("Missing mod qualifier: " + name);

	domain = name.Substring(0, slash);
	subName = name.Substring(slash + 1);
}
```

## Visual Newlines
Use blank lines to break up long methods into logical blocks. Always put a blank line after any indented statement if the method continues. Do not put a space between every field or every line.
```cs
internal void AddFile(string fileName, byte[] data) {
	fileName = Sanitize(fileName);
	int size = data.Length;

	if (size > MIN_COMPRESS_SIZE && ShouldCompress(fileName)) {
		using (var ms = new MemoryStream(data.Length)) {
			using (var ds = new DeflateStream(ms, CompressionMode.Compress))
				ds.Write(data, 0, data.Length);

			var compressed = ms.ToArray();
			if (compressed.Length < size * COMPRESSION_TRADEOFF)
				data = compressed;
		}
	}

	lock (files) {
		files[fileName] = new FileEntry(fileName, -1, size, data.Length, data);
	}

	fileTable = null;
}
```

## Comment out vanilla code, rather than removing it
```cs
else if (buffType[j] == 117) {
	allDamage += 0.1f;
	/*
	meleeDamage += 0.1f;
	rangedDamage += 0.1f;
	magicDamage += 0.1f;
	minionDamage += 0.1f;
	*/
}
```

When modifying a single line. Leave a commented version of the vanilla code **only when** it is not obvious what has been added by tML and what is vanilla.

Also use a hashtag when multiple changes are part of the same fix/feature/refactor
```cs
//if (item.maxStack == 1 && item.Prefix(-3))
if (item.IsCandidateForReforge && item.Prefix(-3)) // TML: #StackablePrefixWeapons


//if (stack <= 0)
if (stack <= 0 && mouseItem.maxStack == 1) // TML: #StackablePrefixWeapons: Gameplay impact: stackable items will not get a prefix on craft


// TML attempts to make ApplyItemTime calls run on remote players, so this check is removed. #ItemTimeOnAllClients
// if (whoAmI == Main.myPlayer) {
if (true) {
	...
}
```

❌ Unnecessary comment
```cs
//if (Main.mouseItem.IsTheSameAs(inv[slot])) {
if (Main.mouseItem.IsTheSameAs(inv[slot]) && ItemLoader.CanStack(inv[slot], Main.mouseItem)) {
```


## Keep Patches Small
The source code of Terraria is not stored on git, instead tML changes are stored in .patch files in the patches/ directory. Keeping patches as small as possible makes handling Terraria updates and identifying the exact changes tML requires much easier. 

Use return/continue or goto to avoid changing indentation. **Only when 5+ lines would be indented**
```cs
if (!WallLoader.PreDraw(j, i, wall, spriteBatch))
	goto PostDraw;

...

PostDraw:
WallLoader.PostDraw(j, i, wall, spriteBatch);
```

```cs
private bool ItemCheck_CheckCanUse(Item sItem) {
	if(!CombinedHooks.CanUseItem(this, sItem))
		return false;

	...
}
```

Wrap a method to insert a hook at the end of a method with multiple return statements
```cs
public void HitEffect(int hitDirection = 0, double dmg = 10.0) {
	VanillaHitEffect(hitDirection, dmg);
	NPCLoader.PostHitEffect(this, hitDirection, dmg);
}

public void VanillaHitEffect(int hitDirection = 0, double dmg = 10.0) {
	if (!active)
		return;
	
	if (...) {
		return;
	}
	
	...
}
```

Always check your patches when committing and see if there's a way to minimize them.

## Be Aware of Breaking Changes
Some changes made to the tModLoader source code will cause issues for modders and players once they become part of an official release. For example, renaming a field used by mods will cause those mods to break when tModLoader updates. Because of this, we have various strategies for maintaining compatibility. Breaking changes come in 2 varieties, binary incompatibilities and source incompatibilities. When a change results in a binary incompatibility, mods built on an earlier version will either not load or not work properly. When a change results in a source incompatibility, the modder will have to change code the next time they build the mod, but the mod will continue to function otherwise.

The lengths we go to preserve compatibility and maintain functionality depends on how stable the release should be. Currently we make breaking changes without maintaining compatibility on the 1.4 branch quite often, but as 1.4 becomes more stable we will need to adapt many of the techniques below.

### New Hook
Adding a new hook is no issue.

### Adding a parameter to a hook
If `public virtual void SomeHook()` becomes `public virtual void SomeHook(int someParameter)`, mods using the old approach will find that their mod is limited in functionality since the old method is no longer being called. To preserve compatibility, sometimes we keep the old method and mark it as `Obsolete`

```cs
// New hook in ModItem.cs
public virtual void PickAmmo(Item weapon, Player player, ref int type, ref float speed, ref int damage, ref float knockback) {
}

// Old hook in ModItem.cs
[Obsolete("PickAmmo now has a weapon parameter that represents the item using the ammo.")]
public virtual void PickAmmo(Player player, ref int type, ref float speed, ref int damage, ref float knockback) {
}

// Calling site in ItemLoader.cs
public static void PickAmmo(Item weapon, Item ammo, Player player, ref int type, ref float speed, ref int damage, ref float knockback) {
	ammo.modItem?.PickAmmo(weapon, player, ref type, ref speed, ref damage, ref knockback);
	ammo.modItem?.PickAmmo(player, ref type, ref speed, ref damage, ref knockback); // deprecated

```


### Changing a return type
Changing a return type causes incompatibilities. TODO, more info.