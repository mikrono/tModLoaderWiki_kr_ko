`WIP`
# What is C#7?
C#7 is the seventh (\`\`7´´) version of the c-sharp language. It builds on the sixth (\`\`6´´) version of the compiler, Roselyn. To learn more about C#6, see the [specific page about it](Intermediate-modding-with-c%236).

# How to build with C#7?
**To build with C#7, you need a tML version that is _above v0.10.1.5_.** Because internally there are no changes in compiler, you do not need to set languageVersion to a different version. Instead, if you've set it to `6` (\`\`six´´) you can begin using language-level-7 features.
```
languageVersion = 6
```

# Most useful features
Below is a list of features C#7 has to offer that are most useful when modding.

## Nested functions ( Local functions )
With C#7, you can create nested functions.
```c#
public void MyMethod() {
    void MyNestedFunc() {
        // I do something
    }

    MyNestedFunc(); // calling my nested function
}
```
This is an awesome feature that allows you to break up larger methods into smaller chunks. Nested functions can be reused within the same method as well. These functions are in scope in the enclosing block.

## Inline out variables
```cs
public void PrintCoordinates(Point p) {
    p.GetCoordinates(out int x, out int y);
    mod.Logger.InfoFormat("({0}, {1})", x, y);
}
```
Note that the variables are in scope in the enclosing block, so that the subsequent line can use them. Many kinds of statements do not establish their own scope, so out variables declared in them are often introduced into the enclosing scope.

This also allows "discards" as out parameters as well, in the form of a _, to let you ignore out parameters you don’t care about:
```cs
p.GetCoordinates(out var x, out _); // I only care about x
```

## Pattern Matching
### Constant patterns
E.g:
```cs
if (obj is ModItem) {
    // obj is a ModItem!
} else // or else if (!(obj is ModItem))
    // obj is not a ModItem!
}
```

### Type patterns
E.g:
```cs
if (obj is ModItem modItem) {
    // I can use modItem here!
    modItem.item.damage = ....
}
```
or: (reduced nesting like this is often favorable)
```cs
if (!(obj is ModItem modItem)) return;
// I can use modItem here!
modItem.item.damage = ....
```

### Patterns in switch statements
```cs
switch(modItem)
{
    case ModItem a:
        // I got a ModItem!
        break;
    case ModItem b when (b.damage == 10):
        // I got a ModItem with a damage of 10!
        break;
    case MyModItem c when (b.MyCustomType == MyCustomType.Something):
        // ....
        break;
    default:
        // ...
        break;
    case null:
        throw new ArgumentNullException(nameof(modItem));
}
```

## Tuple types and literals
```cs
(int, float, int) GetItemInfo(Item item) // tuple return type
{
    return (dmg: item.damage, kb: item.knockBack, proj: item.shoot); // tuple literal
}
.....

var itemInfo = GetItemInfo(myItem);
mod.Logger.Info(itemInfo.dmg.ToString());
// itemInfo.kb .... itemInfo.proj
```

Allows discarding too:

```cs
var (dmg, kb, _) = GetItemInfo(myItem);
// I can use dmg, kb (variables)
// Same as:
int dmg;
float kb;
(dmg, kb, _) = GetItemInfo(myItem);
// or:
(int dmg, float kb, _) = GetItemInfo(myItem);
```