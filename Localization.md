# What is Localization?
Localization provides the ability for a mod to be played and enjoyed by users who speak a language different from the language the mod author uses. ExampleMod Every piece of text that a user might see while playing a mod is contained in text files called localization files. For example, ExampleMod has an item called "Paper Airplane" in English, but "Бумажный самолётик" in Russian. Through the use of localization, Russian users can understand and enjoy the content of ExampleMod without learning English.

These localization files are easy to work with, allowing users without technical skills to translate mods. These translations can then be provided to the mod author or published as a new mod, allowing more people to enjoy the mod.

This wiki page will cover localization topics intended for a mod developer. If you are interested in localizing an existing mod, or providing localization to tModLoader itself, please read the [Contributing Localization wiki page](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization).

# Migrating from 1.4.3 to 1.4.4
Beginning in tModLoader v2023.01, all localization is now done completely in the .hjson files. Declaring translations in code is no longer supported. This change will greatly streamline localization management and make mod translations. See the [Major Localization Changes feature proposal](https://github.com/tModLoader/tModLoader/issues/3074) if you are interested in more of the rationale for these changes.

## Generate Localization Files on 1.4.3
To begin, we need to use an older tModLoader to export localization files.

Open up tModLoader, enable the mod, and visit the Mod Sources menu. Find your mod. You will find a green arrow that says "Export 1.4.4+ localization files", press it.    
![image](https://user-images.githubusercontent.com/4522492/210681409-a659670d-5908-4e5d-bd45-74c0545f4666.png)    
Now navigate to the ModSources folder and navigate to your mod's localization files. You should see newly generated `.hjson.new` files:    
![image](https://user-images.githubusercontent.com/4522492/210681629-8aad2234-bd56-40c8-b03f-7e36b98d4486.png)    
Open up the new files in a text editor and confirm that they look correct. There should be entries that were previously in the current .hjson files as well as newly generated entries for all other content in the mod. If it looks good, continue to the next steps.

## Switch to 1.4.4 tModLoader and Build Mod
You'll need to use Steam to switch to the branch where 1.4.4 tModLoader. This branch is currently the `1.4-preview` branch. After launching tModLoader, you'll notice that your mod (and most likely all other mods you had enabled) fails to load, this is expected. Visit the Mod Sources menu and press the "Run tModPorter" button. Along with other changes, this will remove all code using the old localization approaches.     
![image](https://user-images.githubusercontent.com/4522492/210683375-43816104-2812-4db2-bac6-813ebb47a089.png)    
After this, you'll want to find those `.hjson.new` files and use them to replace the existing `.hjson` files. (Make backups of these files first if you aren't using Git to backup your files.) Delete the `.hjson` files and then rename the `.hjson.new` files to `.hjson`.

Now, you might need to open up Visual Studio and fix any remaining compilation issues. Once you have fixed any remaining issues, you can rebuild your mod and it should work.

# Automatic Localization Files
tModLoader will automatically update .hjson files using the English files as a guide. 

## Prefix
Modders can use a special filename pattern to indicate that all localization entries in a file share a common prefix. The most common usage of this is to omit the "Mods" and "ModNameHere" entries from localization files. By omitting these, the file is less indented and easier for some to work with.

For example, a file called "Localization/en-US_Mods.ExampleMod.hjson" will inherit the "Mods.ExampleMod" prefix, meaning that the file can start directly with an entry for "Items".

The pattern for this is as follows: The file path is split by folder, then by underscore. After culture is found, the next result will be used as the prefix. The following are all examples of options to indicate that a file is intended for English and should use the "Mods.ExampleMod" prefix.

```
Localization/en-US_Mods.ExampleMod.hjson
Localization/en-US/Mods.ExampleMod.hjson
en-US_Mods.ExampleMod.hjson
en-US/Mods.ExampleMod.hjson
```

# Adding new Translations
## Custom Keys
To add new translations, simply follow the hjson syntax. For example, a modder could add a category

## Custom Fields
Modders can add `LocalizedText` properties to their classes.

# Adding a new Language
By default, tModLoader will only generate localization files for languages it sees the mod supports. To add a new language, simple make a text file and name it in the same manner as your existing localization files. You only need to make one. The file or folder path needs to contain the language code for the language: English ("en-US"), German ("de-DE"), Italian ("it-IT"), French ("fr-FR"), Spanish ("es-ES"), Russian ("ru-RU"), Chinese ("zh-Hans"), Portuguese ("pt-BR"), or Polish ("pl-PL"). Once the file is made and has the correct file extension, rebuilt the mod. The file will update with entries ready for translating. Other files will also be generated following the organization of the English hjson files.

# ModType and ILocalizedModType
Mods implementing a custom `ModType` can implement `ILocalizedModType` to easily facilitate localization. This is as simple as adding `, ILocalizedModType` to the class inheritance and implementing the `LocalizationCategory` property by adding `public string LocalizationCategory => "MyModTypeCategory";`. For each `LocalizedText` in your custom `ModType` class, you can use `public virtual LocalizedText DisplayName => this.GetOrAddLocalization(nameof(DisplayName), PrettyPrintName);`, allowing them to be categorized properly in hjson files as other existing ModType classes.