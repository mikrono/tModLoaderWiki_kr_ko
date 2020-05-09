The Mod Browser is an online component of tModLoader where Mod Makers can publish their mods. Users are able to easily find, download, and install mods they are interested in directly from tModLoader. It is highly recommended that you visit the website and/or read the description of mods you want to download.

### Direct Download
If you are having trouble in-game, you can use [Direct Mod Download Listing](http://javid.ddns.net/tModLoader/DirectModDownloadListing.php) to download mods directly. Place them in `%userprofile%/Documents/My Games/Terraria/ModLoader/Mods/`

### Mod Browser
That's easy, just download from Terraria. If that doesn't work, refer to Option 1.

### Steam Workshop
This will only work for the Steam version of tModLoader when it is available. Workshop support will come after the release of tModloader on Steam.
***
**Modders wishing to publish their mod to the Mod Browser should continue reading.**
***

## Getting Started
The first time you attempt to publish a mod by pressing the `Publish Mod` button in the `Mod Sources` menu, you will be directed to the [Mod Browser Registration page](http://javid.ddns.net/tModLoader/register.php) where you will authenticate via Steam to verify your identity. (You don't need to own Terraria on Steam, you just need an account) The Mod Browser, having confirmed your identity, will provide you with a `Passphrase` that will be pasted into tModLoader. Now that you have a passphrase, you can now publish mods on the Mod Browser.

## Publishing Mods
Publishing a mod is simple, after making sure your mod is ready for release, you just press the `Publish` button in the `Mod Sources` menu. Before publishing, however, do the following:

### Set a Version
Set a version in your build.txt. Something like `0.1` is good for an initial release. Please use semantic versioning: `major.minor.patch` You are not allowed to use letters for the version

### Fill out description.txt
This is the description that potential users will read when deciding if they will download, make it good.

### Test
A lot. You don't want your mod to be quickly judged to be bad because of a poor first release. 

### Make a Website
Most mods make a thread on the [Terraria Community Forums](https://forums.terraria.org/index.php?forums/client-server-mods-tools.116/) Making a thread provides a great way to interact with your users. They can give bug reports and criticism or praise. Once you have a thread, copy the URL and paste it into your build.txt on the `homepage` property. See ExampleMod if you are unclear on the layout of the build.txt file. Save build.txt.

### Build
Save all your files and Build the mod in the Mod Sources menu.

### Github
If you have a Github repository for your Mod, push all changes.

### Publish
Click Publish in the Mod Sources Menu. Within a few minutes, your users will now see that an update to their favorite mod is now available for download.

## Updating Mods
To update a mod you have published, first do the following:

### Update Version
Increment the version in your build.txt. Your Version should be 2 to 4 numbers separated by periods. Do not add letters such as "v" or "beta" to your version. [Read about versions](https://msdn.microsoft.com/en-us/library/system.version(v=vs.110).aspx#Anchor_6). Note: Some examples: 0.1 is the same as 0.001, and 0.0.0.10 comes right after 0.0.0.9. 

### Update Description
Update your description.txt with any new info you'd like potential users to know about. A brief recent changelog might be nice here.

### Test
Test your changes a lot. Test the Save and Load code in your ModPlayer and ModWorld classes by loading old saves from previous versions of your mod.

### Build
Save all your files and Build the mod in the Mod Sources menu.

### Github
If you have a Github repository for your Mod, push all changes now so your releases stay in line with the source code.

### Publish
Click Publish in the Mod Sources Menu. Within a few minutes, your users will now see that an update to their favorite mod is now available for download.

### Update Website
If you have a website, now would be a good time to advertise the update and changelog.

## Unpublishing Mods
Warning! No confirmations are given for the buttons in this menu! You can unpublish a mod in-game by going to `Mod Sources` and then `Manage Published` and clicking `Unpublish`. Only do this if the mod is of low quality.

## .tmod File size limitations
The default limit for mods is 5 MB. If you have linked the Mod Browser to your GitHub repository, it can be up to 50 MB. If you find that your mod is too big, check your Mod's source folder for large files that shouldn't be there. By default, all files in the folder will be packaged into the .tmod file. Use buildIgnore in build.txt to specify folders or files to ignore, or delete the files in question. Common culprits include photoshop or other image editing project files.

## Mod Icon
A Mod icon can be added to a mod by adding an 80x80 icon.png to the root of the Mod's source folder. As an incentive for using GitHub and linking your repository, your Icon will show in the Mod Browser menu in addition to the Mods menu.

Here is a template if you wish to use it:  
![](https://i.imgur.com/uluOTmD.png)

## GitHub integration
The Mod Browser uses a lot of bandwidth, more than we can afford. In an effort to alleviate these limitations and encourage Open Source software, mods that have their GitHub repository linked to the Mod Browser will enjoy several benefits including larger files size (up to 50 MB from 5 MB), a history of old releases, and Mod Icon visible in the Mod Browser menu.

To integrate, visit the [Mod Browser Registration page](http://javid.ddns.net/tModLoader/register.php), log in, click "Manage Mods", then "Submit GitHub Authorization". After that, follow the instructions very, very carefully. After success, the next time you publish your mod you will enjoy the benefits.

## Limitations
### Terraria Community Forum Rules
Mods published to the Mod Browser must abide by the [TCF Modding rules](https://forums.terraria.org/index.php?threads/player-created-game-enhancements-rules-guidelines.286/) Rules of note include the rule against porting content from non-PC versions of Terraria and the rules against plagiarism. Violating these will result in revoked Mod Browser privileges.

### Mod Icon Guidelines
Any nudity or inappropriate content in a Mod Icon will result in especially severe response.
What will be deemed inappropriate? You can probably guess it, but still here's a few examples: nudity/porno, swearing, violence/harm to other humans or animals, and also icons that encourage vandalism, crime, terrorism, racism, eating disorders, or suicide.. The list can go on.. just use an appropriate icon please.

### Malicious Code
Don't do it. The mods you upload are tagged with your Steam identity. Your will be **permanently banned** from the browser and all of your mods will be removed. We will also notify Terraria and Steam staff about you and your malicious code.