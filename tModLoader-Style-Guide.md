The number one rule of collaborative development is _pick a style and stick to it_. The following style guidelines should apply to all future contributions to tModLoader. Please be mindful of past developers and don't reformat code just for the sake of it.

The following are useful resources for general C# design and style
* https://google.github.io/styleguide/csharp-style.html
* https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/
* https://github.com/ktaranov/naming-convention/blob/master/C%23%20Coding%20Standards%20and%20Naming%20Conventions.md

If something is not listed in this guide, use your best judgement to pick between matching existing code and the advice listed above.

## Identifier Names
Use `camelCase` for
* local variables
* method arguments
* instance fields
* non-public fields in static classes

Use `PascalCase` for everything else.

_**Why: camelCase indicates a 'normal' variable for the context. No further thought required. internal/private fields on static classes are akin to instance fields on a singleton.**_

Use `_camelCase` for 'backing fields' on properties. Private fields may also optionally be prefixed with an _.

_**Why: _ indicates the field is an implementation detail and not key to the overall function of the class.**_

A class should have either public properties or public fields. not both. If you write a public property, convert all public fields to auto properties. Properties are recommended for complex objects or data containers, but simple structs and classes are easier to reason about in game development when only fields are present.

_**Why: Avoids confusion and inconsistency in the public 'contract' of the class. Terraria itself rarely uses properties. Microsoft guidelines recommend avoiding public fields entirely.**_

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
	
## Keep Patches Small
The source code of Terraria is not stored on git, instead tML changes are stored in .patch files in the patches/ directory. Keeping patches as small as possible makes handling Terraria updates and identifying the exact changes tML requires much easier. 

Use block comments rather than removing code
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
Use goto to avoid changing indentation. **Only when 5+ lines would be indented**
```cs
if (!WallLoader.PreDraw(j, i, wall, spriteBatch))
	goto PostDraw;

...

PostDraw:
WallLoader.PostDraw(j, i, wall, spriteBatch);
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

Always check your patches when committing and see if there's a way to minimise them.

## Special Cases
The patches of tModLoader contain some strong preferences for handling particular additions, that otherwise aren't covered in preceding documentation.

If working within a method that has code nested within an ```if(statement){}``` block, and there exists no subsequent code within the method that is required to run following the ```statement``` returning false, then the following applies  
DO:
```void method(){
//something code here
if (data == null)
    return;

//the nested code

//nothing here
}```

DON'T:
```void method(){
//something basic here

if (data != null){
    //the nested code
}

//nothing here
}```