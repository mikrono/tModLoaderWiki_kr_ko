- **Learn how to contribute localization (translations) to your favorite mod or even to tModLoader itself**
- **This guide also covers a brief introduction for the [Git version control](https://en.wikipedia.org/wiki/Git) system, but if you want a full guide you should read the [Git & mod management](Intermediate-Git-&-mod-management)**

# Contributing Localization
Most mods should have their code on github, this guide will give a primer on how to provide translations if they do. _If the mod you are interested has their source code private (or doesn't have a Github), you will have to contact the mod developers and ask them how to go about contributing localization._

Currently, tML supports the following locales:

| Language  | Abbreviation | Filename
|----------:|:-------------|--------------|
|           |              |              |
| English   | en-US        | en-US.lang   |
| German    | de-DE        | de-DE.lang   |
| Italian   | it-IT        | it-IT.lang   |
| French    | fr-FR        | fr-FR.lang   |
| Chinese   | zh-Hans      | zh-Hans.lang |
| Spanish   | es-ES        | es-ES.lang   |
| Russian   | ru-RU        | ru-RU.lang   |
| Portugese | pt-BR        | pt-BR.lang   |
| Polish    | pl-PL        | pl-PL.lang   |

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
A localization file will be named by the language's abbreviation (see the list above) with the `.lang` extension. For example: `en-US.lang`. _These files should be located in a folder called `Localization`_.

Lang files follow the simple key=value format:
`key=value`
**Keys must not contain whitespaces**. The key is what you are translating, the value is the actual translation.
tML will only register lines that are in this format, so lines that do not contain the = sign are ignored and can be used as comments.

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