Under construction.

Here are a couple example videos until someone writes this:

Discovering the exact cause of `Object reference not set to an instance of an object` errors: In this [video](https://gfycat.com/CluelessFineDarklingbeetle), we can easily diagnose where the error is happening. When the error happens in the game, Visual Studio is brought to the front and the line where the error occurs is highlighted. We can then hover over things and inspect their values. Here we notice that `instance` is null.

Now we can fix the error, we can use `Find all references` to confirm that yes, we forgot to set `instance` to a value. You might notice the green underline under `instance` warning us that `instance` is never assigned. We should have paid attention! Now we can easily fix the error: [video](https://gfycat.com/RedBlackCollardlizard)