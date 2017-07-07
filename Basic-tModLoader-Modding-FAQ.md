### "Terraria.ModLoader.Mod.GetTexture(String name)" error
This error means that tModLoader can't find the Texture that you have specified. You may be thinking, "Where did I specify the Texture to use?", and to answer that, you should read the [Autoload](https://github.com/blushiemagic/tModLoader/wiki/Basic-Autoload) page.  

Now that you read about Autoloading, you now know that the texture is derived from the namespace and classname of the class. If you don't know what namespace and classname are: [Google](http://www.google.com)

Imagine a class named `CoolGun` in the namespace `ExampleMod.Items` in a file named `MyGun.cs` which resides in the folder `\Mod Sources\CoolMod\Items\Weapons\` alongside the texture file `MyGun.png`.

The texture the modder assumed would be used: `\Mod Sources\CoolMod\Items\Weapons\MyGun.png`  
The texture autoload will actually look for: `\Mod Sources\ExampleMod\Items\CoolGun.png`  

Lets go through the problems a novice modder will run into here:
- Expecting the .cs filename and .png filename have to match.
  - Not true, the classname needs to match the texture filename. It is helpful, however, to keep the .cs filename and the class the same.
- Expecting the folder path of the .cs file to matter
  - Not true, the namespace of the class within the .cs file matters for autoloading the texture correctly. It is helpful, however, to match the namespace structure to the folder structure.

Seeing the error, the novice modder will take the following steps:
- Keep .cs filenames, classnames, and .png filenames consistent. 
- Make sure your base namespace and Mod Sources folder are equal
- Make sure your namespace and folder structure match: ExampleMod.Items.Weapons becomes `\Mod Sources\ExampleMod\Items\Weapons\`

### "Sequence contains no matching element" error
This means your Mod doesn't contain any class extending from Mod. If you went through the [Basic Modding Guide](https://github.com/blushiemagic/tModLoader/wiki/Basic-tModLoader-Modding-Guide) you should notice that one of the files has `class MyMod : Mod`. Make sure you have 1 and only 1 of these classes in your mod.

### "error CS0234: The type or namespace name 'X' does not exist in the namespace 'Y' (are you missing an assembly reference?)"

This usually happens when your code has namespaces or classes that conflict with vanilla Terraria classes or namespaces. For example, if you happen to have a namespace named Projectile, and try to use Projectile.NewProjectile, the computer will be confused if you are referring to the Terraria.Projectile class or something in your Projectile namespace. The easiest solution is to not name your namespaces the same as vanilla classes. For example, ExampleMod has namespaces named Dusts, Projectiles, and Items as to not conflict with vanilla classes of Dust, Projectile, and Item.

### Error CS0246: The type or namespace name 'Player' (or Item, Projectile, etc) could not be found (are you missing a using directive or an assembly reference?)

This is because the computer doesn't know what the Player class is. You need to tell the computer where Player is found. Do this by adding "using Terraria;" to the top of the file so the computer knows how to find the Player class. (Another common one is Vector2, which needs "using Microsoft.Xna.Framework;")

### Error CS0103: The name 'ItemID' (or ProjectileID, NPCId, etc) does not exist in the current context

This is because the computer doesn't know what the ItemID class is. You need to tell the computer where ItemID is found. Do this by adding "using Terraria.ID;" to the top of the file so the computer knows how to find the ItemID class.

### Error CS0161: 'ExampleItem.UseItem(Player)' (or some other hook): not all code paths return a value

Some methods return values. When using tModLoader hooks, you need to be aware that methods return values and not returning a value is an error. For example, UseItem has the method signature of: `bool UseItem(Player player)`, and that `bool` there tells us that we have to return a bool value. Consult the [documentation](http://bluemagic123.github.io/tModLoader/html/index.html) to know what value to return.

### TileObjectData.AddTile Issues

Make sure you call `TileObjectData.AddTile` after all of your statements regarding `TileObjectData.newTile`. Misordered code involving these statements will break Terraria.