***
This Guide is for 1.4.4, the current version of tModLoader. If you need to view the 1.4 version of this wiki page, click [here](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization/5c2723d318427b2ed8ac35ef0ce4d8f9cc3f450f)
***

- **Learn how to contribute localization (translations) to your favorite mod or even to tModLoader itself**
- **This guide also covers a brief introduction for the [Git version control](https://en.wikipedia.org/wiki/Git) system, but if you want a full guide you should read the [Git & mod management](Intermediate-Git-&-mod-management)**

# Localization Workflow
1. [Choose an approch](#Localization-Approaches)
2. [Get a template](#Localization-Template-Files)
3. [Translate](#Editing-Localization-Files)
4. [Publish translation](#Publishing-Localization-Files)
5. [Maintain translation](#Maintaining-Localization-Files)

# Localization Approaches
There are a few approaches to localize a mod. Some mod developers welcome translators to contribute translations directly to their project, either through GitHub or in some other manner. Other mod developers would prefer that translations are done as separate mods dependent on the original mod. Whatever the approach, this guide will cover how to localize a mod and how to get your localizations available to other users.

### GitHub
Mods that host their code on GitHub are the easiest to contribute translations to. A translator can download the mods source code and work directly in it. 

### Contact Mod Developer
If a mod does not have a public GitHub, you might want to consider contacting the mod developer. They might be happy to accept translations directly from the community and integrate them into their mod.

### New Translation Mod
If a mod developer can't be contacted or doesn't want to deal with translations, a last resort is making a new mod that contains the translations for the mod. This approach is less convenient for users and more work for the translators, but it is an option. (In the future, we plan to support localizations in resource packs.)

# Localization Template Files
The first step to localizing a mod is to obtain the localization files. These files have the file extension `.hjson` and can be edited in any normal text editor.

***

### GitHub
If you are working via GitHub, look for the `hjson` files belonging to the language you wish to use. If no files exist for the language, see the [Adding a new Language](https://github.com/tModLoader/tModLoader/wiki/Localization#adding-a-new-language) section to learn how to generate the files.

### Contact Mod Developer or New Translation Mod
If you are working without the source code, the easiest method is to make a new translation mod. Follow the [Basic tModLoader Modding Guide](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide#your-first-mod) to generate a mod skeleton. I'd recommend that the mod be named: `{OriginalModInternalNameHere}{LanguageCodeHere}`. For example, a French translation of `ExampleMod` might be named `ExampleModFr`. The display name should follow a similar pattern, maybe `Example Mod Traduction française`. While making the mod skeleton, don't generate the template sword item by leaving the entry for `BasicSword` blank.

Now, find `build.txt` in your mod's ModSources folder and open it in a text editor. Add the following line: `modReferences = OriginalModInternalNameHere`, replacing `OriginalModInternalNameHere` with the appropriate internal mod name (A mods internal mod name can be seen by finding the mod in `enabled.json` in the [Mods folder](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-Guide#mods)). Next, add another line with `translationMod = true`. Save `build.txt`. Next, find `en-US.hjson` in `ModSources\YourModNameHere\Localization` and delete it, then make a `.hjson` file for the language you wish to support using the [culture abbreviation](https://github.com/tModLoader/tModLoader/wiki/Localization#culture). 

You can now build the mod in game. Make sure the mod you wish to translate is also enabled and reload mods. Your mods source code folder will now be populated with `.hjson` files. Note that the files will be organized according to the original mods layout, so you might need to look around for the `.hjson` files. If you take this approach, follow the `New Translation Mod` steps in the `Testing Localization Files` section.

**An alternate approach** to generating a new mod if you intend to contribute the translations to the original mod developer is extracting the localization files directly. This approach is simple but will require more work later on. First, open up tModLoader and switch the game language to the language you wish to contribute. Next, navigate to the Mods menu and make sure the mod is enabled. Reload mods if necessary. In the Mods menu, click the `More Info` button. On the bottom right is a button for `Extract Localization`, press it. Once you press the button, the file explorer should appear opened to `Terraria\tModLoader\ModLocalization\ModNameHere`. This folder now contains all localization files in the same layout as the original mod. Do not move or rename files within this folder, that would make it harder for the original mod developer to integrate your changes. You'll likely only find a single `.hjson` file, this is typical. If you take this approach, follow the `Contact Mod Developer` steps in the `Testing Localization Files` section.

***

# Editing Localization Files

Now that you have the `.hjson` file or files for your language, open it in a text editor. You should see entries for content in the mod. As a translator, you only need to edit the right side of lines with the format `key: translation`. Leave category names and keys untouched (text to the left of `:`). For example:

```
Mods: {
	ExampleMod: {
		Config: {
			# This is a comment
			ExampleWingsToggle: {
				/* Label: ExampleWings Toggle */
				/* Tooltip: Enables or disables the ExampleWings item */
			}
		}

		Keybinds.RandomBuff.DisplayName: 随机增益
	}
}
```
This example shows 3 translation entries: `Mods.ExampleMod.ExampleWingsToggle.Label`, `Mods.ExampleMod.ExampleWingsToggle.Tooltip`, and `Mods.ExampleMod.Keybinds.RandomBuff.DisplayName`. The `Label` and `Tooltip` entries are "commented out", while the `Keybinds.RandomBuff.DisplayName` entry has already been translated into Chinese. After translating the right side of the `Label` and `Tooltip` entries, the translator should remember to un-comment the entry by deleting the `/*` and `*/` surrounding the line. 

You will also see lines with comments in the form `# Comment here`. These lines should not be translated, they are simply there for context and other notes the original author wrote down. 

After translating, make sure to save the file.

# Testing Localization Files
If there are any localization keys that are complicated or seem ambiguous, you may want to test your edits.

***

### GitHub
If you are working on the source code directly, you simply need to build and reload your mod in game. The [Learn how to Build the Mod](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide#learn-how-to-build-the-mod) section can teach you how to do that. The [Live Updating feature](https://github.com/tModLoader/tModLoader/wiki/Localization#live-updating) can be used to test changes further while in-game without requiring a rebuild and reload. 

### Contact Mod Developer
Since you don't have the mod code, you'll have to leave this step up to the mod author, or follow the `New Translation Mod` steps below to make a temporary mod to test your changes. If you do, remember not to publish the mod.

### New Translation Mod
To test a translation mod, first make sure the original mod is enabled, then build and reload your mod in game. You should see your translations working.  The [Live Updating feature](https://github.com/tModLoader/tModLoader/wiki/Localization#live-updating) can be used to test further changes while in-game without requiring a rebuild and reload.  

***

# Publishing Localization Files
Once you have checked over everything and made necessary changes, you'll want to publish your localization. This step also depends on which localization approach you are using. 

### GitHub
If you are familiar with GitHub, or want to learn how, you can make a pull request. More info on how to work with GitHub is found in the []() section below. If learning to use GitHub is too much work, there are other ways to contact the author with your translations. One simple way is to make an Issue on their GitHub page and post the files there. If you intent to localize many mods, it may be worth it to learn the basics of GitHub. 

### Contact Mod Developer
Send the finished files to the mod developer. The mod developer will then use your files in their code. The mod developer might want to track translation credits in a comment at the top of their English translation file, and also on their workshop homepage.

### New Translation Mod
If you would like, make an icon for your translation mod. With permission, you might be able to modify the original mods icon. [Mod Skeleton Contents](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Modding-Guide#mod-skeleton-contents) has info on the dimensions of the icon files. You'll also want to edit `description.txt` with information about the translation mod. Once everything is ready, should build the mod once more and then publish the mod. This is done in the Mod Sources menu.

# Maintaining Localization Files
The mod you localized might update with new content. If that happens, you might want to update the translations.

### GitHub
Pull or download the latest code. The template file should be updated already with missing translations. If it isn't, build and reload the mod.

### Contact Mod Developer
If you made a translation mod, reload the mods to update the localization files. Find new untranslated entries, translate them, and get the new files to the mod developer once again.

If you extracted the localization files instead, follow the same steps as before with the updated mod downloaded. If the author integrated your translations from earlier into the latest release, those entries should already be translated in the newly extracted localization files.

### New Translation Mod
Reloading the mods should update the localization files. Find new untranslated entries, translate them, and publish.


***
**Old Instructions Below**
***


# Contributing Localization
Most mods should have their code on github, this guide will give a primer on how to provide translations if they do. _If the mod you are interested has their source code private (or doesn't have a Github), you will have to contact the mod developers and ask them how to go about contributing localization._

Currently, tML supports the following locales:

| Language             | Abbreviation | Filename
|---------------------:|:-------------|---------------|
|                      |              |               |
| English              | en-US        | en-US.hjson   |
| German               | de-DE        | de-DE.hjson   |
| Italian              | it-IT        | it-IT.hjson   |
| French               | fr-FR        | fr-FR.hjson   |
| Simplified Chinese   | zh-Hans      | zh-Hans.hjson |
| Spanish              | es-ES        | es-ES.hjson   |
| Russian              | ru-RU        | ru-RU.hjson   |
| Brazilian Portuguese | pt-BR        | pt-BR.hjson   |
| Polish               | pl-PL        | pl-PL.hjson   |

# Git version control primer
Most mods will publish their source using the git version control system. Github, the site you're on, provides a nice global place for everyone to contribute to projects using git.

In order to contribute, you must know what a commit is, a project fork, and a merge request.

## Commit
A commit in git is a push of changes to the project. Every commit made has changed certain files in the project. When you have created translations for a mod, those will be pushed to the project via a commit.

## Fork
Since you are likely not an official contributor to the project, you cannot push commits to the official project. That is why you must fork the project. A fork is a copy of the project under your own account. This lets you modify files and show them to the world, but not under the official project.

## Merge request
When you've created your translations and committed them to your fork, they are not yet in the official project. In order to do this, you must create a merge request to the official project. On the page of your fork github should provide you with a button to submit a merge request.

# Contributing Localization

## Steps to fulfill
1. Login to your Github account
    1. If you do not have an account, you can register yourself for free on Github [here](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwje9sLuxr7hAhXGUlAKHZBtCXEQFjAAegQIAhAB&url=https%3A%2F%2Fgithub.com%2Fjoin&usg=AOvVaw0H9TK-nu7JfXaoNeNMgJEk).
1. Go to the official project, fork the project to your account
    1. [Example](https://camo.githubusercontent.com/5351d225f62166697dca7309f35590e74d23f9cf/68747470733a2f2f692e696d6775722e636f6d2f656f34464d41732e706e67), [Second example](https://camo.githubusercontent.com/af90abbcfeeb516b0c852abfa54310be4ed267a8/68747470733a2f2f692e696d6775722e636f6d2f344a7471434c672e706e67)
1. Create the translations and commit them to your fork
1. Submit a merge request

## Translation file format
Before you can begin translating, you must understand the localization file format that tML uses.
A localization file will be named by the language's abbreviation (see the list above) with the `.lang` extension. For example: `en-US.lang`. _These files should be located in a folder called `Localization`_. Make sure the file encodings are UTF-8, not UTF-8-BOM, and the file uses consistent line separators (usually `CL RF` aka `\r\n`). Due to a bug (?), the first line of a `.lang` file will not be parsed as a translation, so make a newline there or a comment (`# this is a comment`).

Lang files follow the simple key=value format:
`key=value`
**Keys must not contain whitespaces**. The key is what you are translating, the value is the actual translation.
tML will only register lines that are in this format, so lines that do not contain the = sign are ignored and can be used as comments.

### Placeholders
You may see things like `Missing mod: {0} required by {1}` in translation entries. The `{0}` and `{1}` are placeholders for text that the game will fill in depending on the logic of the code. This is similar to how `string.Format` is used in normal C# programming. You may have to see the text in game to make sense of the usage in order to make accurate localization.

### Plurals
As an extension to placeholders, tModLoader also supports language rule defined cardinal pluralization. In English, nouns have two forms, one for a single count of that noun, and one for multiple of that noun. For example, "1 dog" and "3 dogs". Other languages have different rules. To support this, special syntax is used to provide translations the ability to pluralize properly. In this example, `{0} {^0:mod;mods} filtered by enabled filter.`, the `{0}` is the placeholder where a number will be populated, and the `{^0:mod;mods}` tells the game to check the 0-th placeholder and use its value to choose between `mod` and `mods`. 

English, German, Italian, Spanish, and Portuguese all use the first form for values of 1, and the 2nd form for other values. French uses the first form for values of 0 or 1, and the 2nd form for other values. Chinese only has 1 form regardless of count. Polish and Russian form rules are a bit more complicated and can be found on [the unicode Language Plural Rules webpage](https://www.unicode.org/cldr/charts/43/supplemental/language_plural_rules.html). The order of cardinal forms in these charts correspond to the order tModLoader will use.

## How to add translations
For this example we will use [The Luggage Mod](https://github.com/JavidPack/TheLuggage)
The key you need to use must directly correlate to the class you wish to translate for. Open a new tab in your browser and navigate back to your fork or the original repository. We must now find Item names and other translation "keys" to translate. As a simple example, lets start with an Item. We must find classnames of items in the mod so our translations will be properly linked up to the item in-game. Navigating to `TheLuggage/Items/`, we see `OddKey.cs`. We can't assume `OddKey` is the classname, so we need to click on the file and find the name of the class. Basically look for `public class ThisIsClassName : ModItem`. We find `public class OddKey : ModItem` in OddKey.cs, so lets add a Spanish Translation to the translation file we have open in the other tab in our browser. (Translation files have translation "keys" and translation "values". The "keys" are unchanging identifiers that tModLoader uses to match up the "values" to things the Mod adds. To avoid overlap, such as an Item and Projectile that share the same name, each "Key" starts with a prefix. The key prefixes are as follows: `ItemName, ItemTooltip, BuffName, BuffDescription, ProjectileName, NPCName, MapObject, and Prefix`) 

Add the following line to the translation file we have open: `ItemName.OddKey=Clave extraña`. `ItemName` is necessary for tModLoader to correctly link translations, `OddKey` is the classname, `=` is used to separate the key from the value, and `Clave extraña` is the Spanish translation for `Odd Key`, which we see in OddKey.cs: `DisplayName.SetDefault("Odd Key");`. Since we are here, lets add the tooltip as well. Looking at the previous paragraph, we see that `ItemTooltip` is the prefix we need. We see that the English is `Tooltip.SetDefault("Summons the Luggage");`, so lets add `ItemTooltip.OddKey=Convoca el equipaje` to our file. 

Now that you know how to add translations, continue to add them as needed. You can add them for the entire mod if you have time. Remember to change the translation key prefix according to the type of file you are localizing, also remember that the name of the file should correspond to the abbreviation of the language the localization is for.

## Tips
**The following part of this guide explains the process of contributing translations in greater depth, and can be especially useful if you do not understand the process as it also shows example images.**

### Finding localization files
Look for .lang files in the repository. They might be anywhere or non-existent. If you see a folder called Localization, they are probably in it. (If you search for `filename:.lang` you can quickly search the mod for files with `.lang` in the filename. Unfortunately, you need to do this search on the original repository since GitHub doesn't support searching on Forks currently.)

If you find .lang files, great, just click on it to open it in your browser, then click the edit button on the top right. ([Example](https://i.imgur.com/FoPruy5.png))

### Creating new localization files
If you do not find a .lang file for the language you are localizing to, we will need to create a new file. The file name needs to be exact, see the table at the beginning of this guide to find the filename you should use.
1. [Example 1](https://i.imgur.com/59DLyZy.png)     
1. [Example 2](https://i.imgur.com/OAliVHo.png)   
1. [Example 3](https://i.imgur.com/P8oT1yA.png)    

### Save and Merge Request. 
Your new file page should now look like this: [Example](https://i.imgur.com/XIgFH2B.png)    
Scroll down and optionally add a message, and then click `Commit New File` (or Commit Changes if you are editing an existing file.): [Example](https://i.imgur.com/JEHKfFt.png)  
  
Your file is now saved and "commited" to your "fork". We are not done yet. Now we must make a "merge request". This is basically notifying the mod owner that you have made some changes and you wish for them to accept them. Click the `Pull request` button: [Example](https://i.imgur.com/0jw3dBh.png)  
  
On the next page, you'll see a summary of your changes. Simply click `Create Pull Request`, give the Pull Request a name like "Spanish Localization", and then `Create Pull Request` again to finalize your pull request. [Example](https://i.imgur.com/jiSQ3le.png)     

Congratulations! You have now contributed the localization, and if the mod owners accept your merge requests the translations will be in the next release of the mod.     

## Local Testing
You can test your localizations by building the mod source from your personal fork. If you have not setup a modding environment then you should probably leave this to the owner of the mod you are helping out.

## tModLoader Translations
tModLoader itself doesn't use .lang files, but .json files. The process is roughly the same, except for the structure of the files. Navigate to the [tModLoader source folder](https://github.com/tModLoader/tModLoader/tree/master/patches/tModLoader) and find the .json file you want to edit. Open the file and look for lines with Key Values that have `//` before them. These files are missing translations. Follow the same procedure as with translating mods. Fork, edit, and pull request. Remember to only change the right side and remove the `//` from the lines you localize.

## Miscellaneous 
* Newlines can be added with `\n` interspersed in the Value.   
* Don't put spaces in Keys. See [ExampleMod .lang files](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Localization/en-US.lang) for more info on spaces in Keys.
* Reuse vanilla and modded keys via substitution syntax: `{$Full.Key.Name}`. See [ExampleMod](https://github.com/tModLoader/tModLoader/blob/master/ExampleMod/Localization/en-US.lang) for use in .lang files. This syntax also works in code, such as `Tooltip.SetDefault("{$Full.Key.Name}"))`;.