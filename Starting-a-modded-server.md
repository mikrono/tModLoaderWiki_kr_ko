# Server
For the most part, launching a tModLoader server is the exact same as launching a Terraria server. 

## Setting Up a Dedicated Server
If you have an extra old computer sitting around or leave your computer on all the time, you can use that computer to host a dedicated server.

### Configure the server
You can change `serverconfig.txt` with your server configurations which will be loaded by the tModLoader server when using the instructions below.

#### Getting the server to load mods

##### Windows

On your client, create a modpack of all the mods you want to use on the server, then copy that modpackName.json file to the server.  Create a folder to hold the mods "C:\Server\Mods" then create a ModPacks folder in that folder.  Copy your modpack.json file in the ModPacks folder and the associated tmod files into the Mods folder.  In your serverconfig.txt file, add the following two lines at the end of the file

modpath=c:\Server\Mods
modpack=modpackName

##### Linux (tested on Ubuntu 20.04 LTS)
Go to `/home/<user>/.local/share/Terraria/ModLoader/Mods` and create a new directory called ModPack:  
`mkdir ModPack`  
Upload your mod pack JSON file there. You might need to upload mods manually to the mod folder.  
  
If you are missing `serverconfig.txt`, you have to create one manually. You can use config preset from the internet or make one yourself.  
Some recommended parameters to have there:  
#Load the world automatically when you run ./tModLoaderServer -config *configname*.txt, this one is required for some other arguments to take effect  
`world=path to world file, for example /home/<user>/.local/share/Terraria/ModLoader/Worlds/<worldname>.wld` **(it must be .wld and not .twld!)**  
#Sets the max number of players allowed on a server. The value must be between 1 and 255. (default=8)  
`maxplayers=<number>`  
#Set the port number (default=7777)   
`port=<number>`  
#Set the server password  
`password=<password>`  
#Reduces enemy skipping but increases bandwidth usage. The lower the number the less skipping will happen, but more data is sent. 0 is off.  
`npcstream=<number>` (recommended around 2-6, increases required bandwidth but reduces teleporting)  
#Link the mods folder (might default to that so could be irrelevant)  
`modpath=/home/<user>/.local/share/Terraria/ModLoader/Mods`  
#Select the modpack to load for the specific world  
`modpack=<name of modpack without .json>` (maybe works with .json too, not sure)  
For full list go to: https://terraria.gamepedia.com/Server#Server_config_file  
Note: Journey's End config commands are NOT going to work because tModLoader runs on an older version of terraria!  
  
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
    1. Launch a server normally, people with the server IP and port can join the server.
1. `start-tModLoaderServer-steam-friends.bat`
    1. Launch a server open to your steam friends, who can join using the steam UI. You can also send invites to them.
1. `start.tModLoaderServer-steam-private.bat`
    1. Launch a server explicitly with a closed steam lobby so steam friends cannot join you through the steam UI.

### Starting server on Linux or Mac
The tModLoader installation should have come with a script called `tModLoaderServer`. This script will start an instance of a server.
1. Browse to the directory of installation using `cd /desiredPath/`.
    1. desiredPath is the installation folder path name of your Terraria Server patched with tModLoader.
    2. For Mac, the installation folder is generally in `~/Library/Application\ Support/Steam/steamapps/common/Terraria/Terraria.app/Contents/MacOS`.
2. Verify correct path using `pwd` or make sure the files are there by typing `ls`.
3. Run the script using `./tModLoaderServer`.

If the script is refusing to behave properly (e.g. it is not running as expected), then there might be a permission issue. Simply type `sudo chmod u+x tModLoaderServer` while being on the installation directory of tModLoader to fix this issue.

Optional: it is recommended running the server inside of a screen or tmux session. This way, when you need to close the terminal, you don't have to close the server. This is especially useful for headless servers where the owner will only be connecting via SSH.