# What is TagCompound

TagCompound is the data format for custom data saved using tModLoader. We use TagCompound in ModWorld, ModItem, ModPlayer, GlobalItem, and ModTileEntity. If you are familiar with the concepts, thinking of TagCompound as a nestable dictionary or JSON comes pretty close. Like a dictionary, we provide string keys and values of any supported type.

Please use [NBTExplorer](https://github.com/jaquadro/NBTExplorer/releases/tag/v2.7.6.-win) to visualize TagCompounds by opening .twld or .tplr files with it.

# Key Ideas
Below are some things to keep in mind as you use TagCompound.

## Mod Version Updates
Using TagCompound helps modders update mods smoothly. For example, if v1.0 of a mod saves only `a`, but v2.0 saves both `a` and `b`, the modder doesn't need to make extra checks validating the value or presence of `b` for most situations. They'd only need to do extra effort if `b` has a non-default value. For example, `b = tag.GetInt("b");` will return the default value of int if the value does not exist in the TagCompound, and the default value of int is 0. If 0 is the value the mod would expect for a missing entry, this works out well. If a non-default value is what the mod expects for a missing entry, the following approach can be used:

```cs
public override void Initialize()
{
	QuestsLeft = 10;
}

public override void Load(TagCompound tag)
{
	if (tag.ContainsKey("QuestsLeft"))
		QuestsLeft= tag.GetInt("QuestsLeft");
}
```

Notice how initialize sets the value to 10, the default value our mod expects. We then check that the tag has an entry for "QuestsLeft", and if it does, we retrieve that value. Without this check, `tag.GetInt` would return 0 if the key did not exist. Designing your variables such that the default values correspond to the expected default values might be useful if you wish to avoid checking `ContainsKey`.

## Initialize
Make sure to initialize values in appropriate methods, constructors, or field initializers. This is because Load will not be called if no TagCompound has previously been saved for this entity. For example, always make sure to reset `ModWorld` values in `ModWorld.Initialize`, if you don't data from other worlds will cross over into other worlds as the player goes in and out of worlds. Here is an example:

```cs
internal class MyModWorld : ModWorld
{
	public static bool MySpecialBool;

	public override void Initialize()
	{
		MySpecialBool = false;
	}

	public override void Load(TagCompound tag)
	{
		MySpecialBool = tag.GetBool("MySpecialBool");
	}

	public override TagCompound Save()
	{
		return new TagCompound {
			{"MySpecialBool"), MySpecialBool }
		};
	}
}
```

In the above example, we make sure to set `MySpecialBool` to false in `ModWorld.Initialize`. If we forgot to do this, the following could happen: Player enters World A, `MySpecialBool` is set to true because of some event (such as defeating a boss), player exits World A and enters World B, World B doesn't have TagCompound data yet, so Load is not called, `MySpecialBool` is still true despite the fact that what `MySpecialBool` represents has never happened in World B. Always remember to set values to default values in the appropriate method, constructor, or field initializer. 

# Simple Example

Lets start with ExamplePlayer.cs:

```cs
public override TagCompound Save()
{
	return new TagCompound {
		{"score", score},
		{"exampleLifeFruits", exampleLifeFruits},
	};
}

public override void Load(TagCompound tag)
{
	score = tag.GetInt("score");
	exampleLifeFruits = tag.GetInt("exampleLifeFruits");
}
```

Here we see a very simple example of saving and loading 2 int variables. In Save, we create a single TagCompound and add data to it. In Load we are provided a TagCompound named tag and retrieve values from it. We must use the appropriate methods in Load that match the data type of the data stored. If you are confused by this Save example, see below for an example that does the same thing, but is a little easier to understand if you are unfamiliar with some c# syntax:

```cs
TagCompound saveData = new TagCompound();
saveData.Add("score", score);
saveData.Add("exampleLifeFruits", exampleLifeFruits);
return saveData;
```

# List Example
A common mistake that modders make with TagCompound is saving lists of data as individual entries in a TagCompound. For example, the following is a bad approach:

```cs
TagCompound saveData = new TagCompound();
for (int i = 0; i < stats.Count; i++)
{
	saveData.Add("stats_" + i, stats[i]);
}
return saveData;
```

Saving individual entries like this is not how we should be using TagCompound. Here is what this approach looks like in NBTExplorer:    
![](https://i.imgur.com/puTAMHM.png)    

TagCompound supports Lists of compatible data types, here is a proper approach:

```cs
// Save
TagCompound saveData = new TagCompound();
saveData.Add("stats", stats);
return saveData;

// Load
stats = tag.GetList<int>("stats");
// or
stats = tag.Get<List<int>>("stats");
```    
![](https://i.imgur.com/dBLlbVu.png)    

# Dictionary Example
Saving and loading a dictionary can be done, but take a little effort. One approach is to save the keys and values as lists and then reconstruct the Dictionary by using the Zip method:

```cs
// This code can be found in TEScoreBoard in ExampleMod: https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Tiles/TEScoreBoard.cs
using System.Linq;

internal Dictionary<string, int> scores = new Dictionary<string, int>();

public override TagCompound Save()
{
	return new TagCompound
	{
		{"scoreNames", scores.Keys.ToList()},
		{"scoreValues", scores.Values.ToList()}
	};
}

public override void Load(TagCompound tag)
{
	var names = tag.Get<List<string>>("scoreNames");
	var values = tag.Get<List<int>>("scoreValues");
	scores = names.Zip(values, (k, v) => new { Key = k, Value = v }).ToDictionary(x => x.Key, x => x.Value);
}
```
Here is a more complex example of a `Dictionary<ulong, Tuple<string, int>>`. Using Zip to do this would be a little hard, so we take an alternate approach here. Rather than store keys and values as lists separately, we construct a list of TagCompound, each TagCompound in that list representing an entry in the Dictionary:

```cs
public Dictionary<ulong, Tuple<string, int>> complexDictionary;

public override void Initialize()
{
	complexDictionary = new Dictionary<ulong, Tuple<string, int>>();
}

public override TagCompound Save()
{
	var list = new List<TagCompound>();
	foreach (var item in complexDictionary)
	{
		list.Add(new TagCompound() {
			{ "pid", item.Key },
			{ "name", item.Value.Item1 },
			{ "deaths", item.Value.Item2 },
		});
	}
	return new TagCompound
	{
		{ "complexDictionary", list },
	};
}

public override void Load(TagCompound tag)
{
	var list = tag.GetList<TagCompound>("complexDictionary");
	foreach (var item in list)
	{
		ulong pid = item.Get<ulong>("pid");
		string name = item.GetString("name");
		int deaths = item.GetInt("deaths");
		complexDictionary[pid] = new Tuple<string, int>(name, deaths);
	}
}
```

![](https://i.imgur.com/Ecu3gc3.png)

# Nested Example

**TODO: Finish this section with an example.**

# Item Example
A common task for modders is saving an Item. Don't attempt to hack out your own approach by saving the itemid or name of the item and then attempting to restore it during Load. The Item class is natively supported by TagCompound. Unloaded items will persist through mods unloading and loading as expected.

# Custom TagCompound Example

**TODO: Finish this section with an example.**

# TagSerializer and TagSerializable Examples
Creating a TagSerializer or implementing the TagSerializable interface is another approach to simplifying saving and loading custom data to TagCompounds. We can create and register a TagSerializer for classes that are not a part of our mod, while implementing TagSerializable is preferable for classes defined in our mod.

## TagSerializer Examples
We create TagSerializers for classes not under the control of our mod. Adding TagSerializers allows us to save and load the class directly in TagCompound. Here is an example:

```cs
public class RectangleSerializer : TagSerializer<Rectangle, TagCompound>
{
	public override TagCompound Serialize(Rectangle value) => new TagCompound
	{
		["x"] = value.X,
		["y"] = value.Y,
		["width"] = value.Width,
		["height"] = value.Height
	};

	public override Rectangle Deserialize(TagCompound tag) => new Rectangle(tag.GetInt("x"), tag.GetInt("y"), tag.GetInt("width"), tag.GetInt("height"));
}

// In Mod class:
public override void Load()
{
	Terraria.ModLoader.IO.TagSerializer.AddSerializer(new RectangleSerializer());
```

Having registered the TagSerializer, we can now freely use Rectangle as if it were a natively supported data type:
```cs
public override TagCompound Save()
{
	return new TagCompound {
		{"rectangle", rectangle},
		{"rectangles", rectangles},
	};
}

public override void Load(TagCompound tag)
{
	rectangle = tag.Get<Rectangle>("rectangle");
	rectangles = (List<Rectangle>)tag.GetList<Rectangle>("rectangles");
}
```
![](https://i.imgur.com/dBpWSf0.png)    

Please be aware that RectangleSerializer is already implemented in tModLoader natively, and any attempt to register a TagSerializer for Rectangle will cause your mod to fail to load. Several other common classes already have TagSerializers made for them such as Vector2, Vector3, Color, Point16, and Rectangle. Since multiple TagSerializers for the same class will cause errors, if you implement a TagSerializer for a class that might be useful for other modders, please reach out to us with your implementation and we can add it in directly to the next tModLoader release.

## TagSerializable Examples
If a class is defined in your mod, it is better to implement TagSerializable directly on the class.

**TODO: Finish this section with an example.**

# Other Topics
## nameof operator
The `nameof` operator is available to those using c#6, it can simplify some things, but be careful. (See [nameof documentation](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof)) The `nameof` operator can help avoid spelling mistakes between Load and Save. Be careful, however, of refactor-renaming variables in your mod (the F2 command in Visual Studio). If you rename a ModPlayer variable that is saved using `nameof`, for example, your mod will lose data when your users update the mod. See below for an example. Notice how we don't need to write `"CorruptionSpreadDisabled"` to specify the key, risking a spelling error that would be hard to catch. 

```cs
public override void Load(TagCompound tag)
{
	CorruptionSpreadDisabled = tag.GetBool(nameof(CorruptionSpreadDisabled));
}

public override TagCompound Save()
{
	return new TagCompound {
		{nameof(CorruptionSpreadDisabled), CorruptionSpreadDisabled}
	};
}
```

## Indexer Initializers
If you are using c#6, you can use this slightly simpler syntax. Compare the two below:
```cs
// The usual approach
return new TagCompound {
	{"score", score},
	{"exampleLifeFruits", exampleLifeFruits},
};
// Indexer Initializers approach
return new TagCompound
{
	["score"] = score,
	["exampleLifeFruits"] = exampleLifeFruits
};
```
