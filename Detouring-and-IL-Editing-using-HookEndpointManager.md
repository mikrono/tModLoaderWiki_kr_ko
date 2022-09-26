There is also another way to modify other mod methods. If you looked into dlls generated with MonoMod's HookGen, you would see that it uses `HookEndpointManager`:
![](https://i.imgur.com/nKp8Vt3.png)

You can skip creating intermediate dll with HookGen and call `HookEndpointManager` directly. However, it's not possible (at least for C#10 and .NET 6) to create hooks exactly like HookGen does it, so doing this will require some knowledge of working with Reflection.

## Preparations

Start by getting `MethodInfo` of the method you want to detour or IL-edit:

![](https://i.imgur.com/3kmQ8Bm.png)

## Detouring

Add delegates for original method and hook method:

![](https://i.imgur.com/HsHthgw.png)

`orig_PlaceRoxShrine` delegate matches with original method signature (return type and parameters).

`hook_PlaceRoxShrine` matches original method but with added first parameter, `orig_PlaceRoxShrine` delegate.

Detour method (`PlaceRoxShrine_Detour`) matches `hook_PlaceRoxShrine` signature.

<b>Please do note that instance methods will have first parameter as class instance, for example:</b>
![](https://i.imgur.com/tIRDylX.png) 

And finally, call `HookEndpointManager.Add`:

![](https://i.imgur.com/05NkaDX.png)

<b>tModLoader will make sure all detours and IL edits are unloaded during mod unloading, there is no need to manually unregister them</b>

## IL Editing

IL Editing is easier to setup than detouring, just create a method thar retuns `void` and has single parameter - `ILContext` to manipulate and call `HookEndpointManager.Modify`.

![](https://i.imgur.com/srGTVi8.png)