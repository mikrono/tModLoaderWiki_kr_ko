*** 
This Page Has Been Updated for 1.4. For 1.3 Instructions, Click [Here](https://github.com/tModLoader/tModLoader/wiki/Starting-a-modded-server/dac6879dd891bfc74695d51a822379189d69f189)
***

***
This page is outdated for 1.4.4 dedicated servers. 
Please refer to our [DedicatedServerUtils Readme](https://github.com/tModLoader/tModLoader/tree/1.4.4/patches/tModLoader/Terraria/release_extras/DedicatedServerUtils)
***

# Dedicated Server Setup Pre-word

There are a few options for establishing a Dedicated Server on 1.4 tModLoader.
We will cover just one of these options for simplicity, but do not be discouraged to experiment!

All of this information is included in the DedicatedServerUtils folder in any tmodloader install.

## Setting Up a Dedicated Server on Linux

If you have an extra old computer sitting around or leave your computer on all the time, you can use that computer to host a dedicated server.

## Docker vs Management Script
While both the Docker container and the management script can install and update tModLoader and any mods, there are a few key differences. Docker isolates tModLoader from your host system and increases security. The management script allows for direct access to your server and increased control as a result. If you are a public server operator or just prefer Docker, then go with the [Docker Container](#using-the-docker-container), otherwise make use of the [management script](#using-the-management-script).

## Using The Management Script
The `manage-tModLoaderServer.sh` script can be used to install tModLoader either directly from the GitHub release or from SteamCMD. The script is made to run fully standalone, so just download it to your server and run it. No other files from the repo are needed.

### Installing tModLoader
#### Via SteamCMD (recommended)
* Ensure SteamCMD is installed and on your PATH. You can install SteamCMD from your package manager or [Valve's Wiki](https://developer.valvesoftware.com/wiki/SteamCMD).
* Run `./manage-tModLoaderServer.sh --install --username your_steam_username`.
* You will be prompted for your password (and your 2fa code if applicable).
* By default, tModLoader will be installed to `~/Steam/steamapps/common/tModLoader`. To specify an installation directory, use `--folder /full/path/from/root`

#### From GitHub
* Run `./manage-tModLoaderServer.sh --install --github`.
* By default, tModLoader will be installed to `~/tModLoader`. To specify an installation directory, use `--folder /path/to/install`.
* This will install the latest GitHub release, which is the same version as released on Steam.

### Installing Mods
Mods will be automatically installed during the tModLoader installation step, but can also be installed separately using the `--mods-only` argument. Simply place any `.tmod` files, `install.txt` for workshop mods, and `enabled.json` into the same directory as the script. Additionally, you can avoid updating or installing mods with the `--no-mods` argument.

#### Obtaining install.txt
Because the steam workshop does not use mod names to identify mods, you must create a modpack to install mods from the workshop. To get an `install.txt` file and its accompanying `enabled.json`:
* Go to Workshop
* Go to Mod Packs
* Click `Save Enabled as New Mod Pack`
* Click `Open Mod Pack Folder`.
* Enter the folder with the name of your modpack

You can copy `enabled.json` and `install.txt` to your script directory and they will be used next time the script is run (run `./manage-tModLoaderServer.sh --mods-only` to install mods immediately).

### Launching
To run tModLoader, you just need to navigate to your install directory (`~/tModLoader` for GitHub, `~/Steam/steamapps/common/tModLoader` for SteamCMD, by default), and run `./start-tModLoaderServer.sh`. There is also a `--start` argument that will launch the game.

#### Automatically Selecting A World
If you want to run tModLoader without needing any input on startup (such as from an init system), then all you need to do is copy the example [serverconfig.txt](https://github.com/tModLoader/tModLoader/tree/1.4.4/patches/tModLoader/Terraria/release_extras/DedicatedServerUtils/serverconfig.txt) and change the settings how you like. Additional options can be found [on the Terraria wiki](https://terraria.wiki.gg/wiki/Server#Server_config_file)

## Updating
If an update for `manage-tModLoaderServer.sh` is available, a message will be printed letting you know one is available. It can be updated using `./manage-tModLoaderServer.sh --update-script`. An outdated script may contain bugs or lack features, so it is usually a good idea to update.

When using `manage-tModLoaderServer.sh`, tModLoader updates can be performed with `./manage-tModLoaderServer.sh --update`. When using a GitHub install, use `--github`. Use`--folder` if your install is in a non-standard location. Mods will be updated as well.

When using the Docker container, simply rebuild the container using `docker-compose build` to update tModLoader. Mods will be updated as well.

## Using The Docker Container
To install and run the container:
* Ensure `docker` and `docker-compose` are installed. They can be installed from your package manager or [Docker's Documentation](https://docs.docker.com/engine/install/)
* Download [docker-compose.yml](https://github.com/tModLoader/tModLoader/tree/1.4.4/patches/tModLoader/Terraria/release_extras/DedicatedServerUtils/Docker/docker-compose.yml) and the [Dockerfile](https://github.com/tModLoader/tModLoader/tree/1.4.4/patches/tModLoader/Terraria/release_extras/DedicatedServerUtils/Docker/Dockerfile).
* Next to those docker files, create a folder named `Terraria`, and place `enabled.json`, [install.txt](#obtaining-install.txt), [serverconfig.txt](#automatically-selecting-a-world), your worlds, and any `.tmod` files inside.
* Edit `docker-compose.yml` with your GID and UID. These can be found by running `id`.
* Run `docker-compose build`
* Run `docker-compose up`

The server will be available on port 7777.

To run without any interactivity, use `docker-compose up -d`, and include [serverconfig.txt](#automatically-selecting-a-world) in the `Terraria` directory.

## Autostarting On Boot
When using `manage-tModLoaderServer.sh`, refer to your distro's documentation. You can likely use a startup script with your init system.

When using the Docker container, add `restart: always` within `services.tml` inside of `docker-compose.yml`, then rebuild with `docker-compose build`.

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
