# Server
For the most part, launching a tModLoader server is the exact same as launching a Terraria server. 

## Setting Up a Dedicated Server
If you have an extra old computer sitting around or leave your computer on all the time, you can use that computer to host a dedicated server.

### Configure the server
You can change `serverconfig.txt` with your server configurations which will be loaded by the tML server when using the instructions below.

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
The tML installation should've come with several .bat files, including:
1. `start-tModLoaderServer.bat`
    1. Launch a server normally, people with the server IP and port can join the server.
1. `start-tModLoaderServer-steam-friends.bat`
    1. Launch a server open to your steam friends, who can join using the steam UI. You can also send invites to them.
1. `start.tModLoaderServer-steam-private.bat`
    1. Launch a server explicitly with a closed steam lobby so steam friends cannot join you through the steam UI.

### Starting server on Linux
TODO (Undocumented)

### Starting server on Mac
TODO (Undocumented)
