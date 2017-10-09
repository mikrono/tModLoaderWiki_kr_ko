This guide is to help those who are having troubles with playing multiplayer with tModLoader.

# I can't join my own server using Host and Play
## Verify that you are connecting to the correct server
### "You are not using the same version as this server"

You are trying to connect to a server that is not the correct version. You might be trying to connect to an un-modded server, or you might be connecting to an outdated tModLoader server. 

1. Open tModLoader
2. Open `tModLoaderServer.exe` directly by opening `C:\Program Files (x86)\Steam\steamapps\common\Terraria` and double clicking on the `tModLoaderServer.exe` file.
3. Verify that the tModLoader version of the Client and the Server match exactly. If not, reinstall the latest tModLoader.
4. You can also verify the filesizes: 
   * Windows Steam tModLoder v0.10.1 Client: 11,696 KB
   * Windows Steam tModLoder v0.10.1 Server: 11,684 KB
![](https://i.imgur.com/S7uPCUw.png)

# I can join my server, but my friend can't
## Verify that your server is visible to the internet
1. Open `tModLoaderServer.exe` directly by opening `C:\Program Files (x86)\Steam\steamapps\common\Terraria` and double clicking on the `tModLoaderServer.exe` file.
2. Select a world and start the server
3. Go to [canyouseeme.org](http://canyouseeme.org/) on the computer with the server running and type in 7777 as the port (adjust this if you changed it.)
4. Click Check Port
    * If the website says error, your ports are not open. You may need to enable UPnP on your router, unblock the tModLoaderServer.exe on the Windows Firewall, or manually port forward on the router. With the firewall, you'll want to click details and make sure the path matches the modded server path. There might be 4 separate entries depending on how you installed tModLoader, so you'll need to check all of them. You'll want to make sure both check boxes are checked. For router issues you'll just have to Google that and learn how to do that.
![](https://i.imgur.com/Ds6brPn.png)
    * If the website says Success, you should see a message like "Exception normal: Tried to send data to a client after losing connection" on the server console. This is fine. We now know that the server is visible to the internet, so the problem might be on your friends computer.  
![](https://i.imgur.com/xxPNTBN.png)
![](https://i.imgur.com/4Eo66NN.png)

## My connection is fine but my friend or I am stuck on "Connecting..."
This is where things get complicated. There could be many reasons for this error, and most of them are likely caused by poorly programmed mods. The best approach is to launch the server manually (NOT Host and Play) and watch for messages that appear to be errors in the output. 

First, I would suggest using fresh player and world files and attempting to connect to the server. If this works, the problem might be a mod that has trouble saving or loading its custom data correctly.

To figure out which mod is causing issues, you'll need to start the server many different times either adding or removing mods from your list of mods until you discover that you can join the server again. I'd start with no mods and then work my way up, adding a few mods at a time. Once you discover that you can't connect anymore, you have found a problem mod by the process of elimination. Unfortunately, the solution is to just not use that mod unless the modder who made the mod can diagnose the problem using your modlist, or world and player files.

The process:  
![](http://i.imgur.com/5nGP6Qi.png)