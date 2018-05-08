# Contributing Localization
This guide will teach you how to contribute localization ("translations") to your favorite mod or tModLoader itself. This guide will teach you how to do it through GitHub. If the mod you are interested has their source code private (or doesn't have a Github), you will have to just contact the mod developers and ask them how to go about contributing localization.

Currently Terraria supports English, German, Italian, French, Spanish, Russian, Chinese, Portuguese, and Polish. If you wish to make a mod you like usable in one of these languages, read on.

## Why Help Localize Mods?
The vast majority of tModLoader mods are made in English, and if you are reading this, you probably understand English well enough, but we all have friends who aren't quite as good at English as we are. By contributing localization to mods you enjoy, your friends can also enjoy them as well, as well as everyone else who speaks your language.

# Contributing Localization

## Finding a Mod to Localize
First, we need to find a mod that is willing to support localization (and has prepared support for it, if applicable). To do this, a good first step is visiting the homepage of the mod. To find a homepage of a mod, simply click the `Mod Info` button in either the `Mod Browser` or `Mods` menu, and then click the `Visit the Mod's Homepage` button. Once there, look around for hints that the Mod Developer is interested in Localizing their mod. If there is no specific mention of them looking for localization for their mod, contact the modder directly and ask just to be safe. If the Mod's homepage is on the Terraria Community Forums, either making a post on that thread or Direct Messaging the modder is appropriate. If they have a Discord server, that is another place you can ask. Lets assume that we found a mod that would appreciate localization. Since this guide focuses on Github, we will assume the Mod has a Github. A Github is a place where the source code of a Mod is stored, making the source available for viewing by interested people. For this guide, we will be Localizing a mod called [The Luggage](https://github.com/JavidPack/TheLuggage) into Spanish as an example.

## Create a Github Account
Visit [github.com](https://github.com/) and create an account. This part is easy and straightforward, but necessary. Do not skip this step. Go with the Free plan and do whatever you want on the "Tailor your experience" page. (I clicked skip this step.) Once your account is created, we are done with this section.

![](https://i.imgur.com/90jwn0c.png)    

## Fork a Repository
First, lets briefly explain some terms. "Repository" correlates to "Mod" here in this guide. "Fork" or "Forking" is a term that basically means we are taking an existing "Mod/Repository" and creating a personal copy of that Mod on our own account. This personal copy of our mod can be freely edited without affecting the original copy, so don't worry. 

We don't have permission to directly edit a Mod/Repository on Github, so what we do is we "fork" a "repository", make our changes, and then make a "pull request" with our changes, which the Mod owner will likely gladly accept if done properly. Don't worry too much if those terms are confusing, just follow along step by step.

Lets open the Github page for the Mod we want to localize:
![](https://i.imgur.com/eo4FMAs.png)     
Now, click the fork button. (Make sure to click Fork itself, not the right side of the button with the number.) When I tried this, it made me verify my email address, so I did that first. Now you should see the following:
![](https://i.imgur.com/wTUWS9R.png)    
![](https://i.imgur.com/4JtqCLg.png)    
Alright, we are now in business. To recap, what we have done is made a copy of the original mod onto our own account so that we can begin to work on it.

## Add Translations
Look for .lang files in the repository. They might be anywhere or non-existent. If you see a folder called Localization, they are probably in it. (If you search for `filename:.lang` you can quickly search the mod for files with `.lang` in the filename. Unfortunately, you need to do this search on the original repository since GitHub doesn't support searching on Forks currently.) By the way, `.lang` is the file extension used by tModLoader for translation files. You can open `.lang` files in any text editor, but we will be doing our work directly on GitHub for the convenience of this tutorial.

If you find .lang files, great, just click on it to open it in your browser, then click the edit button on the top right.    
![](https://i.imgur.com/FoPruy5.png)    

If you do not find a .lang file for the language you are localizing to, we will need to create a new file. The file name needs to be exact. The filenames are as follows: English ("en-US.lang"), German ("de-DE.lang"), Italian ("it-IT.lang"), French ("fr-FR.lang"), Spanish ("es-ES.lang"), Russian ("ru-RU.lang"), Chinese ("zh-Hans.lang"), Portuguese ("pt-BR.lang"), and Polish ("pl-PL.lang"). Below we see the process creating the file:

![](https://i.imgur.com/59DLyZy.png)     
![](https://i.imgur.com/OAliVHo.png)   
![](https://i.imgur.com/P8oT1yA.png)    

We now have a new file ready for adding new translations. Open a new tab in your browser and navigate back to your fork or the original repository. We must now find Item names and other translation "keys" to translate. As a simple example, lets start with an Item. We must find classnames of items in the mod so our translations will be properly linked up to the item in-game. Navigating to `TheLuggage/Items/`, we see `OddKey.cs`. We can't assume `OddKey` is the classname, so we need to click on the file and find the name of the class. Basically look for `public class ThisIsClassName : ModItem`. We find `public class OddKey : ModItem` in OddKey.cs, so lets add a Spanish Translation to the translation file we have open in the other tab in our browser. (Translation files have translation "keys" and translation "values". The "keys" are unchanging identifiers that tModLoader uses to match up the "values" to things the Mod adds. To avoid overlap, such as an Item and Projectile that share the same name, each "Key" starts with a prefix. The key prefixes are as follows: `ItemName, ItemTooltip, BuffName, BuffDescription, ProjectileName, NPCName, MapObject, and Prefix`) 

Add the following line to the translation file we have open: `ItemName.OddKey=Clave extraña`. `ItemName` is necessary for tModLoader to correctly link translations, `OddKey` is the classname, `=` is used to separate the key from the value, and `Clave extraña` is the Spanish translation for `Odd Key`, which we see in OddKey.cs: `DisplayName.SetDefault("Odd Key");`. Since we are here, lets add the tooltip as well. Looking at the previous paragraph, we see that `ItemTooltip` is the prefix we need. We see that the English is `Tooltip.SetDefault("Summons the Luggage");`, so lets add `ItemTooltip.OddKey=Convoca el equipaje` to our file. 

Now that we are getting use to this, continue to go through files and add translations to our .lang file. Remember to change the translation key prefix according to the type of file you are localizing. Once you are done, go to the next step.

## Save and Pull Request. 
Your new file page should now look like this:     
![](https://i.imgur.com/XIgFH2B.png)    
Scroll down and optionally add a message, and then click `Commit New File` (or Commit Changes if you are editing an existing file.):     
![](https://i.imgur.com/JEHKfFt.png)    
Your file is now saved and "commited" to your "fork". We are not done yet. Now we must make a "pull request". This is basically notifying the mod owner that you have made some changes and you wish for them to accept them. Click the `Pull request` button:      
![](https://i.imgur.com/0jw3dBh.png)    
On the next page, you'll see a summary of your changes. Simply click `Create Pull Request`, give the Pull Request a name like "Spanish Localization", and then `Create Pull Request` again to finalize your pull request.     
![](https://i.imgur.com/jiSQ3le.png)     
Congratulations! You have now completed your translation. The mod owners will very likely accept your pull request and your translations will be in the next release of the mod.     

## Local Testing
If you are familiar with building mods, feel free to test your translations in game by downloading your fork and building the mod on your computer. For the most part, you can trust the .lang file to work properly as long as you name it correctly and structure the translation keys properly.

## tModLoader Translations
tModLoader itself doesn't use .lang files, but .json files. The process is roughly the same, except for the structure of the files. Navigate to the [tModLoader source folder](https://github.com/blushiemagic/tModLoader/tree/master/patches/tModLoader) and find the .json file you want to edit. Open the file and look for lines with Key Values that have `//` before them. These files are missing translations. Follow the same procedure as with translating mods. Fork, edit, and pull request. Remember to only change the right side and remove the `//` from the lines you localize.

## Miscellaneous 
* Newlines can be added with `\n` interspersed in the Value.   
* Don't put spaces in Keys. See [ExampleMod .lang files](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Localization/en-US.lang) for more info on spaces in Keys.
* Reuse vanilla and modded keys via `{$Full.Key.Name}`. See [ExampleMod .lang files](https://github.com/blushiemagic/tModLoader/blob/master/ExampleMod/Localization/en-US.lang) for this as well.

# Other Topics Not Yet Covered
* Preparing other texts for Localization
* Automatic Retrieval of Localization Keys