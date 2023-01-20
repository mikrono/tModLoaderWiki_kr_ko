***
This Guide was written for tModLoader v1.4.4, an upcoming version of tModLoader. In current tModLoader, localization files won't auto update and some methods mentioned in this guide do not exist. The basic concepts still apply.
***

[中文版 | Chinese version](https://github.com/tModLoader/tModLoader/wiki/zh-Localization-%E6%9C%AC%E5%9C%B0%E5%8C%96)

---

# What is Localization?
Localization provides the ability for a mod to be played and enjoyed by users who speak a language different from the language the mod author uses. Every piece of text that a user might see while playing a mod is contained in text files called localization files. For example, ExampleMod has an item called "Paper Airplane" in English, but "Бумажный самолётик" in Russian. Through the use of localization, Russian users can understand and enjoy the content of ExampleMod without learning English.

These localization files are easy to work with, allowing users without technical skills to translate mods. These translations can then be provided to the mod author or published as a new mod, allowing more people to enjoy the mod.

This wiki page will cover localization topics intended for a mod developer. If you are interested in localizing an existing mod, or providing localization to tModLoader itself, please read the [Contributing Localization wiki page](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization).

# Migrating from 1.4.3 to 1.4.4
Beginning in tModLoader v2023.01, all localization is now done completely in the `.hjson` files. Declaring translations in code is no longer supported. This change will greatly streamline localization management and make mod translations easier to create. See the [Major Localization Changes feature proposal](https://github.com/tModLoader/tModLoader/issues/3074) if you are interested in more of the rationale for these changes. 

**If you are not using Git or some form of version control, it is recommended that you make a backup of your mod's source code folder before continuing.**

## Generate Localization Files on 1.4.3
To begin, we need to use an older tModLoader to export localization files. You'll need to use Steam to switch to the `None` branch. ([Switching between tModLoader versions instructions](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#switch-to-stable-tmodloader-or-to-preview-tmodloader))

Once the game is on the right version, open up tModLoader, enable the mod, and visit the `Mod Sources` menu. Find your mod in the listing. You will find a green arrow that says "Export 1.4.4+ localization files" when hovered, press it.    
![image](https://user-images.githubusercontent.com/4522492/210681409-a659670d-5908-4e5d-bd45-74c0545f4666.png)    
Now navigate to the `"ModSources"` folder and navigate to your mod's localization files. You should see newly generated `.hjson.new` files:    
![image](https://user-images.githubusercontent.com/4522492/210681629-8aad2234-bd56-40c8-b03f-7e36b98d4486.png)    
Open up the new files in a text editor and confirm that they look correct. There should be all the entries that were previously in the current `.hjson` files as well as newly generated entries for all other content in the mod. If it looks good, continue to the next steps.

## Switch to 1.4.4 tModLoader and Build Mod
Use Steam to switch to the `1.4-preview` branch. ([Switching between tModLoader versions instructions](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#switch-to-stable-tmodloader-or-to-preview-tmodloader))

After launching tModLoader, you'll notice that your mod (and most likely all other mods you had enabled) fails to load, this is expected. Visit the Mod Sources menu and press the "Run tModPorter" button. Along with other changes, this will remove all code using the old localization approach.     
![image](https://user-images.githubusercontent.com/4522492/210683375-43816104-2812-4db2-bac6-813ebb47a089.png)    
After this, you'll want to find those `.hjson.new` files and use them to replace the existing `.hjson` files. Delete the current `.hjson` files and then rename the `.hjson.new` files to the `.hjson` extension. (If you can't rename the extensions for the files, you'll need to [enable "File Name Extensions"](https://gfycat.com/TheseSameGrasshopper) so you can.)

Now, you might need to open up Visual Studio and fix any remaining compilation issues. Once you have fixed any remaining issues, you can rebuild your mod and it should work. Once things are working, you can search through your mods source code for lines like `// Tooltip.SetDefault("This is a modded Item.");` or `// DisplayName.SetDefault("Example Sword");` and delete them. They will no longer be used. (You can search all files in the project for the search term `.SetDefault(` to easily find most of these lines.)

# Localization Workflow
Localization files are updated at the end of mod loading. This means that a modder will need to build and load a mod after adding content in order for the localization files to update. After they update, the modder can edit the `.hjson` files to add translations. After this is done, the mod can be built and reloaded once again for the translations to appear in-game.

To avoid losing work, please be aware of the intended workflow:

1. Add new content to mod, such as a new `ModItem`
2. Build and Reload Mod
3. Edit `.hjson` files to give new content an English name
4. Build and Reload Mod
5. Non-English `.hjson` files are now updated with appropriate placeholders and can be updated as well by translators or mod makers

If a translator sends you an updated `.hjson` file to add to your mod directly, be aware that it might get overwritten if the mod loads and tModLoader detects that the `.tmod` file is newer than the `.hjson` file for some reason. If this is the case, you might need to copy the file into the mod sources folder after the mod loads.

# How Localization Works
Every piece of text in the game, from the names of items to the words on the main menu, use localization. Each piece of text in the game is actually a pair of data: a localization key and a localization value. For example, when the player is creating a small world, the game uses the localization key of `UI.WorldSizeSmall` to look up the correct translation value for the currently loaded language, and displays the word "Small" to the user if English is selected. If another language is selected, the game still looks up `UI.WorldSizeSmall` but the localization value will be different. As the creators of Terraria write code in English, most localization keys are very similar to their English translation value.

In tModLoader, mods use `.hjson` files to cleanly contain localization keys and localization values. Each language has their own `.hjson` file. If you are familiar with JSON, these `.hjson` files will be familiar. 

Here is a simple example:

Filename: **tModLoader/ModSources/ExampleMod/en-US.hjson**
```
Mods: {
	ExampleMod: {
		Items: {
			ExampleItem: {
				DisplayName: Example Item
				Tooltip: This is a modded Item.
			}
		}
	}
}
```

In this example, we can see 2 main concepts: localization keys and localization translations. The localization key is derived from the combination of all the text to the left of `:` at each level of nesting. The text to the right of the `:` is the localization value. This example communicates 2 localization keys and their corresponding localization values: `Mods.ExampleMod.Items.ExampleItem.Displayname` corresponds to `Example Item` and `Mods.ExampleMod.Items.ExampleItem.Tooltip` corresponds to `This is a modded Item.`. If the syntax seems complicated, do not worry, modders do not need to manually edit these files, the game will update them automatically.

When this mod is loaded, tModLoader will find all localization files corresponding to the current language and store them in memory. When the game needs to display text to the user, a key is used to query the stored data and retrieve the correct text. Translations are stored in memory as a `LocalizedText` object, modders can use the `Language.GetText` method to retrieve the `LocalizedText` object from a localization key. The `Value` property can be used to retrieve the localization value from the `LocalizedText` object. Alternatively, the `Language.GetTextValue` method returns the localization value directly from a localization key:

```cs
string hivePackDialogue = Language.GetTextValue("Mods.ExampleMod.Dialogue.ExampleTravelingMerchant.HiveBackpackDialogue");
or
string hivePackDialogue = Language.GetText("Mods.ExampleMod.Dialogue.ExampleTravelingMerchant.HiveBackpackDialogue").Value;
or
LocalizedText hivePackDialogueLocalizedText = Language.GetText("Mods.ExampleMod.Dialogue.ExampleTravelingMerchant.HiveBackpackDialogue");
string hivePackDialogue = hivePackDialogueLocalizedText.Value;
```

## Localization Keys
tModLoader will automatically assign translation keys to most content. The key pattern is `Mods.{ModName}.{Category}.{ContentName}.{DataName}`, where `ModName` is the internal name of the mod, `Category` is supplied by the type of content, `ContentName` is the internal name of the content (commonly the classname), and `DataName` specifies the key within the class. 

For example, `ModItem` has the `Category` set as `Items`. It also has 2 separate pieces of data, the `DisplayName` and the `Tooltip`. If a mod named `ExampleMod` adds a `ModItem` class named `ExampleItem`, then 2 keys will be generated and added to the `.hjson` files: `Mods.ExampleMod.Items.ExampleItem.DisplayName` and `Mods.ExampleMod.Items.ExampleItem.Tooltip`.

## Substitutions
If there is text that repeats often in your localization files, or if you wish to use existing text in the game, you can use substitutions to keep your localization files clean and organized. Substitutions take the form of `{$KeyHere}` in localization values. When the game loads, these sections will be replaced by the localized text corresponding to the key provided.

For example, the game already has translations for the text `Right Click To Open`, stored in the `CommonItemTooltip.RightClickToOpen` key. A mod can utilize substitutions to reuse that value. The entry `Tooltip: "{$CommonItemTooltip.RightClickToOpen}"` would end up with the text `Right Click To Open` in the users language for this item. Other existing translations such as item names and other common tooltips are also available for use.

Translations from within the mod can also be used. For example in [ExampleMod's localization files](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Localization/en-US.hjson), `ExamplePylonTile.MapEntry: "{$Mods.ExampleMod.ItemName.ExamplePylonItem}"` is used to reuse the translations contained in the `Mods.ExampleMod.ItemName.ExamplePylonItem` key.

### Scope Simplification

If substitution keys share a scope with the value for a localization key they are being used in, the substitution key can be simplified. For example, in [ExampleMod's localization files](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Localization/en-US.hjson), the value of the `Mods.ExampleMod.ExamplePetItem.DisplayName` key is set to `"{$Common.PaperAirplane}"`. In this case, the game knows to check for keys within the current scope resulting in the value of the key `Mods.ExampleMod.Common.PaperAirplane` being found and substituted in. Using this technique, for example, `Mods.ModName` can be omitted from substitution keys.

### Overriding Content Localization Keys
If many items in your mod share a common translation, you can have them all point to the same translation key. To do this, override the property and return the result of `Language.GetText` using the translation key you wish to use:
```cs
public override LocalizedText Tooltip => Language.GetOrRegister("Mods.ExampleMod.Common.SomeSharedTooltip");
```
If you are using inheritance, you only need to do this in the base class and can even override it again in child classes if a specific child class needs a different localization. See [Adding Localizable Properties](#adding-localizable-properties) for how to add extra localizations to your content, beyond the default properties provided by tModLoader.

## String Formatting
Modders can use [string formatting](https://learn.microsoft.com/en-us/dotnet/api/system.string.format?view=net-7.0#insert-a-string) to leave places in translations for text to be filled in when used. This is a normal feature of c#. Modders can use the `string.Format` method or `Language.GetTextValue` overloads to use string formatting. The [Placeholders section](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization#placeholders) has more info on this.

## Pluralization
When using placeholders for numbers, such as in `{0} minutes ago`, you'll run into issues in English when the number is exactly 1. When the number is 1, the text should say "1 minute ago" instead of "1 minutes ago". This issue can be solved with pluralization. The [Plurals section](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization#plurals) has more info on this feature. 

## Chat Tags
Color and item icons can be added to localization values using [Chat Tags](https://terraria.wiki.gg/wiki/Chat#Tags). Find `ExampleTooltipsItem` in [ExampleMod's localization files](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Localization/en-US.hjson) for an example of this.

# Automatic Localization Files
tModLoader will automatically update `.hjson` files when new content or translation keys are used. The English files will be used as the template for other languages, which will inherit comments and layout automatically. 

## Adding Content
When a modder adds new content to their mod, such as a `ModItem`, that piece of content will initially not be localized. The modder should build and reload their mod. Once the mod loads, the `.hsjon` files will automatically update. The English files will now contain default translation entries for the new content. The non-English files will contain those same entries, but commented out. To localize the content, the modder needs to edit the .hjson files with the desired text, save, and build and reload their mod again.

## HJSON syntax
`.hjson` files contain Hjson data. Hjson is similar to JSON, but is intended to be human readable. See [the Hjson website](https://hjson.github.io/) explains the details of the Hjson syntax, but most modders can just follow examples to get a feel for the syntax. 

### Multiline
If a line of text needs multiple lines, use the following syntax. Make sure the indentation is consistent:
```
			SomeKey: 
			'''
			This translation key has 2 lines.
			This is the 2nd line!
			'''
```

### Special Characters
If a translation value needs to start with `{}[],:` or whitespace, you'll need to quote the translation. You can omit quotes in other situations. If your value needs a literal `"`, you can use the multiline syntax:
```
ExamplePetBuff: {
	DisplayName: "{$Mods.ExampleMod.Common.PaperAirplane}"
	Description: '''"Let this pet be an example to you!"'''
}
```

## Comments
`.hjson` files can contain a variety of comment styles. tModLoader uses Hjson comments to convey 2 separate concepts. 

Comments using the `#` at the beginning of the line are actual comments, useable by mod authors to remind themselves of useful things. These comments should be placed immediately above the key they pertain to. Failure to place comments above the item will result in the comment being lost or misplaced when tModLoader automatically updates the localization files.

Example:
```
...
			ExampleCanStackItem: {
				DisplayName: Example CanStack Item: Gift Bag
				# References a language key that says "Right Click To Open" in the language of the game
				Tooltip: "{$CommonItemTooltip.RightClickToOpen}"
			}
...
```

These comments will be copied from the English files to the non-English files as well, where they can remind translators of where custom translation keys are used, for example.

Comments using the `/* */` or `//` style are used by tModLoader to indicate that a non-English translation key has yet to be translated. This serves as an indicator to the modder about which languages are missing translations. A translator can translated the translation value into their language and remove the comment syntax. Modders should not use this comment syntax for normal comments as they will be lost when the game automatically updates the `.hjson` files.

## Translation File Names
tModLoader will attempt to load all `.hsjon` files in the mod as localization files. As such, localization files can be placed in any folder path, but by convention we recommend placing them in a folder named "Localization" at the root of your mod's source folder. The mod generator follows this convention and will generate `Localization/en-US.hjson` in your mod to get started.

A valid culture code must be present at the start of the file-name, or as the name of a containing folder to determine the language.

### Culture

The following languages, also known as cultures, are supported: English ("en-US"), German ("de-DE"), Italian ("it-IT"), French ("fr-FR"), Spanish ("es-ES"), Russian ("ru-RU"), Chinese ("zh-Hans"), Portuguese ("pt-BR"), or Polish ("pl-PL"). These codes are used to indicate which language the `.hjson` file pertains to. To start supporting a new language, see [Adding a new Language](#adding-a-new-language).

### Prefix
The prefix of a `.hjson` file indicates that all localization entries in the file share a common prefix. The most common usage of this is to omit the `Mods` and `ModNameHere` entries from localization files. By omitting these, the file is less indented and easier for some to work with. The vast majority of mods won't use localization values outside their mods prefix.

For example, a file called `Localization/en-US_Mods.ExampleMod.hjson` will inherit the `Mods.ExampleMod` prefix, meaning that the file can start directly with an entry for `Items`.

The pattern for this is as follows: The file path is split by folder, then by underscore. After culture is found, the next result will be used as the prefix. The following are all examples of options to indicate that a file is intended for English and should use the `Mods.ExampleMod` prefix.

```
Localization/en-US_Mods.ExampleMod.hjson
Localization/en-US/Mods.ExampleMod.hjson
en-US_Mods.ExampleMod.hjson
en-US/Mods.ExampleMod.hjson
Localization/CoolBoss/en-US_Mods.ExampleMod.hjson
```

## Multiple Files
Modders can use multiple `.hjson` files to organize their translations. For example, if a mod contained `en-US_Mods.ExampleMod.Items.hjson` and `en-US_Mods.ExampleMod.hjson`, the `en-US_Mods.ExampleMod.Items.hjson` file could contain all item localization while the other file contains the rest of the localization entries. New content will automatically end up in an existing `.hjson` file that has existing entries most similar to the translation key.

If you split up localization files, you only need to edit the English files and then build and reload the mod. The other languages will automatically adjust themselves to match the same layout.

# Adding new Translation Keys
Entries for new content will automatically populate the `.hjson` files, but custom translation keys can also be added to the files.

## Manually Adding Keys
To add custom keys, a modder can follow the `.hjson` syntax to add the localization entry directly. For example, ExampleMod has a category called "Common", these entries were all manually added as they are not used by tModLoader classes directly.

For example, lets start with this `.hjson` file:
```
Mods: {
	ExampleMod: {
    		Common: {
			PaperAirplane: Paper Airplane
		}
        
		Currencies.ExampleCustomCurrency: example currency
   	 }
}
```

We can add a new entry for a `Mods.ExampleMod.Common.NewKey` key by adding a line and following the `PaperAirplane` example. We can add a new category called `Uncommon` by following the syntax shown in the `Common` category. We can also add an entry that only takes a single line rather than specifying the category separately by following the `Currencies.ExampleCustomCurrency` example. The following shows each of these approaches:
```
Mods: {
	ExampleMod: {
    		Common: {
			PaperAirplane: Paper Airplane
        		HotDog: Hot dog
		}
        
 		Uncommon: {
            		Helicopter: Example Helicopter
       		}
        
		Currencies.ExampleCustomCurrency: example currency
        	Currencies.DirtCurrency: piles of dirt
    	}
}
```

Do note that when the localization files are automatically updated, tModLoader will decide how to organize and format the file, which will result in entries moving, but no data will be lost.


## Adding Localizable Properties
Modders can add `LocalizedText` properties to their classes. When correctly implemented, these properties will automatically populate the `.hjson` files and be ready for localization work. 

For example, the following property could be added to a `ModItem` class:

```cs
public LocalizedText SwitchingToMessage => this.GetLocalization(nameof(SwitchingToMessage));
```

The above code defines a get-only property of `Type` `LocalizedText`. The `GetLocalization` method will attempt to retrieve the localization for the derived key. If this localization is not found, the key will be remembered and added to the localization files when they are updated. The localization value automatically added to the `hjson` files will be the key itself, hinting that the entry has not been translated yet.

The key `GetLocalization` generates will be of the form `Mods.{ModName}.{LocalizationCategory}.{ContentName}.{suffix}`. If a specific key outside the expected pattern is needed, a modder could use `Language.GetOrRegister("Full.Key.Here");` instead. Note that `GetLocalization` must be invoked prefixed by `this.` due to the design of C#, it can not be omitted.

**In Depth:** `GetLocalization` is a helper method to simplify code and avoid typos. `GetLocalization` is equivalent to calling `Language.GetOrRegister` with the full key passed in. Similarly, `GetLocalizedValue` is equivalanet to `Language.GetTextValue` in the same manner. `GetLocalizationKey` can be used to retrieve the generated key if desired.

`GetLocalization` and `Language.GetOrRegister` have an optional 2nd parameter named `makeDefaultValue` that defines a function that will be used to make the default value that will be assumed if the localization does not exist. For example, passing in `() => ""`, will result in the default value being an empty string rather than the key. Modders can pass in `PrettyPrintName` to achieve the typical behavior of taking the internal name of a piece of content and adding a space between capital letters. This approach should be used if the localization is optional, or you have a sensible default value for it.

### Registering Localizable Properties

To facilitate automatically registering these keys so that they populate the `.hjson` files, modders will need to access the getter during mod loading. An easy way to do this would be in a `SetDefaults` or `SetStaticDefaults` method:

```cs
public override void SetDefaults() {
	_ = SwitchingToMessage;

    ... other code
}
```

### Retrieving text from Localizable Properties

Elsewhere in the code, that property can be used to display localized text to the user:

```cs
Main.NewText(SwitchingToMessage.Value);
```

### Another Example

[ExampleChest.cs](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Tiles/Furniture/ExampleChest.cs) serves as an example of using a custom key. By default, tModLoader will register a single translation key for each `ModTile` in the form `Mods.{ModName}.Tiles.{ContentName}.MapEntry`. This key makes it easy to add a map entry to a tile. (Map entries control the text shown to the user when the tile is hovered over in the fullscreen map.) `ExampleChest`, however, needs 2 map entries. Using `GetLocalization`, new keys can easily be added to the localization files:

```cs
AddMapEntry(new Color(200, 200, 200), this.GetLocalization("MapEntry0"), MapChestName);
AddMapEntry(new Color(0, 141, 63), this.GetLocalization("MapEntry1"), MapChestName);
```

The result of this code is that the localization file now contains these keys, ready for localizing into other languages:
```
ExampleChest: {
	MapEntry0: Example Chest
	MapEntry1: Locked Example Chest
}
```

Elsewhere in ExampleChest.cs, these localization keys are dynamically retrieved using `GetLocalization`:

```cs
public override LocalizedText ContainerName(int frameX, int frameY) {
	int option = frameX / 36;
	return this.GetLocalization("MapEntry" + option);
}
```

This approach is useful for localization keys that are dynamic.

### ModType and ILocalizedModType
Mods implementing a custom `ModType` can implement `ILocalizedModType` to easily facilitate localization. This is as simple as adding `, ILocalizedModType` to the class inheritance and implementing the `LocalizationCategory` property by adding `public string LocalizationCategory => "MyModTypeCategory";`. For each `LocalizedText` in your custom `ModType` class, you can use `public virtual LocalizedText DisplayName => this.GetOrAddLocalization(nameof(DisplayName), PrettyPrintName);`, allowing them to be categorized properly in `.hjson` files as other existing `ModType` classes.

# Adding a new Language
By default, tModLoader will only generate and update localization files for languages already appearing in `.hjson` files. To add a new language, simply make a text file and name it in the same manner as your existing localization files. You only need to make one. The file or folder path needs to contain the language code for the language: English ("en-US"), German ("de-DE"), Italian ("it-IT"), French ("fr-FR"), Spanish ("es-ES"), Russian ("ru-RU"), Chinese ("zh-Hans"), Portuguese ("pt-BR"), or Polish ("pl-PL"). Once the file is made and has the correct file extension, rebuilt the mod. The file will update with entries ready for translating. Other files will also be generated following the organization of the English hjson files.

Comments and organization of localization files other than English are all inherited from the English files. If you wish to add credits for translators, add them all to a comment at the top of the English files and they will propagate to the non-English files from there.

Modders can freely organize the English localization files to their liking and the other language files will update to match them. If you split up localization files, you only need to edit the English files and then build and reload the mod. The other languages will automatically adjust themselves to match the same layout. Keys removed from the English files will similarly result in the key being removed from other language files. In short, modders typically only need to touch the English files and the other language files will automatically adjust themselves.