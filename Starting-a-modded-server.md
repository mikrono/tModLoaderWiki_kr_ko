*** 
This Page Has Been Updated for 1.4. For 1.3 Instructions, Click [Here](https://github.com/tModLoader/tModLoader/wiki/Starting-a-modded-server/dac6879dd891bfc74695d51a822379189d69f189)
***

# Server

There are a few options for establishing a Dedicated Server on 1.4 tModLoader.
We will cover just one of these options for simplicity, but do not be discouraged to experiment!

## Setting Up a Dedicated Server

If you have an extra old computer sitting around or leave your computer on all the time, you can use that computer to host a dedicated server.

Grab a download of tModLoader from the [github releases](https://github.com/tModLoader/tModLoader/releases)
and extract it to the following location: 
1) Linux: `/home/<user>/.local/share/Steam/tmod`

## Prepare the Server

In the `tmod` folder, you can change `serverconfig.txt` with your server configurations which will be loaded by the tModLoader server when using the instructions below. Change modpath=steamapps.

## Getting mods on to the Server

On your client, create a modpack of all the mods you want to use on the server.
Open the Mod Pack Folder.
Go in to the <ModPackName>/Mods folder.
Grab a copy of all the files - the .tmod(s), install.txt, and enabled.json

On the server, in the `tmod` folder,
Create a folder to hold the files `steamapps`
Paste all the files to this created folder. 

### Downloading & Updating Workshop Mods
In order to quickly install and be able to quickly update your mods, we recommend using SteamCMD and the Setup_tModLoaderServer.sh script provided.

Start by setting up a folder called `steamcmd` under the `Steam` folder.
Setup steamcmd in that folder
Place Setup_tModLoaderServer.sh, instructions.txt, and a copy of install.txt in that folder.
Run Setup_tModLoaderServer.sh to bulk download every mod listed in install.txt.

The downloaded mods will appear in `Steam/tmod/steamapps`

Alternatively, if you are just after getting mods on to the server and not concerned with updating them, you can copy the entirety of the `steamapps/workshop/1281930` folder from your computer to `Steam/tmod/steamapps/1281930`


## Server Configuration

If you are missing `serverconfig.txt`, you have to create one manually. You can use config preset from the internet or make one yourself.

Some recommended parameters to have there:

Load the world automatically when you run `./tModLoaderServer -config *configname*.txt`, this one is required for some other arguments to take effect

##### Set the world loaded on the server

`world=path/to/WorldFile`

For example, `world=/home/<user>/.local/share/Terraria/ModLoader/Worlds/<worldname>.wld` **(it must be `.wld` and not `.twld`!)**

##### Set the max number of players allowed on a server. The value must be between 1 and 255. (default=8)

`maxplayers=<number>`

##### Set the port number (default=7777)

`port=<number>`

##### Set the server password

`password=<password>`

##### Reduces enemy skipping but increases bandwidth usage. The lower the number the less skipping will happen, but more data is sent. 0 is off.

`npcstream=<number>` (recommended around 2-6, increases required bandwidth but reduces teleporting)

##### Link the mods folder (might default to that so could be irrelevant)

`modpath=/home/<user>/.local/share/Terraria/ModLoader/Mods`

##### Select the modpack to load for the specific world. The `.json` is optional

`modpack=/home/<user>/.local/share/Terraria/ModLoader/Mods/ModPacks/<modpack name>/mods/enabled.json>`

For full list go to: https://terraria.wiki.gg/Server#Server_config_file

**Note**: Journey's End config commands are NOT going to work because tModLoader runs on an older version of terraria!

### Portforwarding

If you want other people to connect to your server from another network, you must open up the port your server will run on.

#### What is a port?

Your computer has access to many ports. When an incoming network packet is received by your internet router, it will come bundled with an associated IP-address and port number. If the IP matches your server IP, it will be sent to your server machine. The port number tells the computer _exactly_ to which application the packet should be sent. This allows computers to run many applications simultaneously and communicate between networks. You can read more [here](https://en.wikipedia.org/wiki/Port_(computer_networking))

#### How do I portforward?

In order to open a port to other networks, it must be explicitly told to open inside your network router. If you live at home, you'll have to discuss with your parents and have them allow you to do so. Opening the specific port for a terraria server will allow other computers to communicate with yours through the terraria application, _but nothing else because the terraria server application will 'consume' the port: no other application can then use this port._

#### How to access the router

Your typical home IP starts with 192.168, the router is often located on 192.168.1.1
You can type this number in your internet browser to navigate to the router webpage. You'll need the password to login, which is typically displayed on a sticker somewhere on the physical router device. It is recommended to change this password if possible to increase security. It is possible your internet provider did this for you, if you do not know the password you'll have to call them and discuss how you can retrieve it. You'll likely have to reset the router, so make sure to remember the password next time.

### Starting server on Windows

The tModLoader installation should've come with several .bat files, including:
1. `start-tModLoaderServer.bat`
    1. Launches a server with serverconfig.txt and some questions about Steam.
2. If you want to customize your server start via launch args only, we recommend using the following: `start-tModLoader.bat -server <LaunchArgs>`

### Starting server on Linux or Mac

The tModLoader installation should have come with a script called `start-tModLoaderServer.sh`. This script will start an instance of a server.

1. Browse to the directory of installation using `cd /desiredPath/`.
   1. desiredPath is the installation folder path name of your Terraria Server patched with tModLoader.
   2. For Mac, the installation folder is generally in `~/Library/Application\ Support/Steam/steamapps/common/Terraria/Terraria.app/Contents/MacOS`.
2. Verify correct path using `pwd` or make sure the files are there by typing `ls`.
3. Run the script using `./tModLoaderServer`.

If the script is refusing to behave properly (e.g. it is not running as expected), then there might be a permission issue. Simply type `sudo chmod u+x tModLoaderServer` while being on the installation directory of tModLoader to fix this issue.

Optional: it is recommended running the server inside of a screen or tmux session. This way, when you need to close the terminal, you don't have to close the server. This is especially useful for headless servers where the owner will only be connecting via SSH.

### Starting server headless

To start the server headless (and not prompt anything) use the command:

`start-tModLoader.sh/bat`

With the arguments:

`-server -config serverconfig.txt <other parameters desired>`

For example, on windows, you can create a `start-tModLoaderServer-NoAsk-CoolWorld.bat` file in the install folder and place the text `start-tModLoader.bat -server -config serverconfig.txt -world "C:\Documents\My Games\Terraria\tModLoader\Worlds\Cool_World.wld"` in it and save. When launched, the sever will launch and load the specified world. Change the world argument filename as necessary. You can also change serverconfig.txt or another config file and use the world argument in there as well. Adding the usual `-steam -lobby friends` or `-steam -lobby private` will make the server joinable via steam invites or friends. You can create a desktop shortcut to this .bat file for convenience.

#### Additional server arguments
##### Set the Steam workshop content directory, useful for using SteamCMD to install workshop items to a custom directory
`-steamworkshopfolder /home/<user>/<Custom Workshop Mod Folder>/steamapps/workshop/`
