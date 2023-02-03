***
此指南是为1.4.4版tModLoader, 一个即将到来的版本而作. 在当前版本的tML中, 本地化文件不会自动更新, 此指南中的一些方法也不存在. 基本的概念仍有效.
***

[Original Page (English) | 原页面 (英语)](https://github.com/tModLoader/tModLoader/wiki/Localization)

---

# 什么是本地化?
本地化使得与作者使用不同语言的玩家也能畅玩模组. 每一条用户可能在游玩时看见的文本都被储存在一种叫做本地化文件的文本文件里. 举个例子, ExampleMod有个物品叫做 "纸飞机", 但在英语中叫 "Paper Airplane". 通过本地化, 无论是汉语还是英语的使用者都可以看懂, 而不需要先去学一门语言. 

这些本地化文件很容易制作, 使得不懂编程的人也能翻译模组. 然后作者便可以将这些翻译加入模组中, 允许更多人游玩. (译注: 而无需加载翻译补丁)

此指南将涵盖模组作者需要知道的本地化专题教程. 如果你想要翻译一个已有的模组, 或者是翻译tML本身, 请参阅[贡献本地化](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization).

# 从1.4.3迁移到1.4.4
从tMLv2023.01开始, 所有的本地化都在`.hjson`文件中完成. 不再支持在代码中声明翻译 (译注: 然而可以硬写文本). 这项改动将大幅精简改良本地化管理并使翻译模组更简单. 如果你对逻辑层面的改动更感兴趣的话, 参阅[重大本地化改动建议](https://github.com/tModLoader/tModLoader/issues/3074). 

**如果你没在使用Git或其它形式的版本控制的话, 建议在迁移之前备份好你的模组.**

## 在1.4.3生成本地化文件
首先, 我们需要一个较 "老" 的tML来导出本地化文件. 用Steam将tML的测试版调至`无`. ([切换tML版本的教程](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#switch-to-stable-tmodloader-or-to-preview-tmodloader))

切换至正确的版本后, 启动游戏, 启用模组, 然后进入`开发模组`菜单. 在模组列表里找到你的模组. 你会看到一个绿色箭头按钮, 鼠标停留于其上时显示 "Export 1.4.4+ localization files", 点它.  
![image](https://user-images.githubusercontent.com/4522492/210681409-a659670d-5908-4e5d-bd45-74c0545f4666.png)  
现在去到`ModSources`文件夹, 然后是你模组里的`Localization`文件夹. 你将会看到新生成的`.hjson.new`文件:  
![image](https://user-images.githubusercontent.com/4522492/210681629-8aad2234-bd56-40c8-b03f-7e36b98d4486.png)  
在文本编辑器里打开上述文件, 确认一下它们都是好的. 原有`.hjson`里的条目和新生成的条目都应该在新文件里. 如果一切正常, 下一步. 

## 切换至1.4.4并生成模组
用Steam将tML的测试版调至`1.4.4-preview`. ([切换tML版本的教程](https://github.com/tModLoader/tModLoader/wiki/Basic-tModLoader-Usage-FAQ#switch-to-stable-tmodloader-or-to-preview-tmodloader))

启动tML后, 你会发现你的模组 (大概你启用的其它模组也会) 加载失败, 这是正常现象. 进入`开发模组`菜单并点击 "Run tModPorter" 按钮. 这会将过时的本地化方法连同其它过时内容一起移除 (译注: 但这并不能解决所有问题, 该手动改的还是逃不过的).  
![image](https://user-images.githubusercontent.com/4522492/210683375-43816104-2812-4db2-bac6-813ebb47a089.png)  
在此之后, 你应该用`hjson.new`文件替换现有的`.hjson`文件. 删除`.hjson`文件并将`.hjson.new`重命名为`.hjson`文件 (如果你不能修改文件扩展名, 你需要先[启用 "文件扩展名"](https://gfycat.com/TheseSameGrasshopper))

现在, 你可能需要打开VS, 修复剩下的问题. 修好以后, 你就可以重新生成你的模组. 确保一切正常后, 你就可以在模组源码里搜索`// Tooltip.SetDefault("这是一个模组物品.");`和`// DisplayName.SetDefault("示例剑");`之类的东西, 删掉. 它们不再有用了. (你可以在项目中的所有文件搜索`.SetDefault(`以轻松找到这些该删的代码)

# 本地化流程
本地化文件再模组加载过程的最后更新. 这意味着模组作者需要在添加内容后生成并加载模组以更新本地化文件. 更新后即可修改`.hjson`文件以添加翻译. 翻译完以后, 需要重新生成并加载模组来使翻译出现在游戏中. 

为了避免丢失翻译, 请关注这一段设计好的工作流程: 

1. 向模组添加内容, 比如一个`ModItem`
2. 生成并加载模组
3. 完成上一步后, `hjson`文件就会自动更新并生成新内容的条目. 编辑`.hjson`文件, 给上述内容一个英文翻译
4. 生成并加载模组
5. 非英语`.hjson`文件自动根据英语文件更新并生成合适的占位条目以供译者翻译

如果译者发给你翻译好的`.hjson`文件, 要小心, 如果tML检测到`.tmod`文件新于`.hjson`文件, `.hjson`文件可能被覆盖. 在这种情况下, 最好的办法是在模组加载前生成一遍. 你可以在tML未开启时用VS生成, 也可以在启动tML时按住Shift键跳过模组加载再进入模组源码页面生成模组. 如果你忘了这一步, 发现tML把新翻译重置成旧的了, 你就得再来一遍. 

# 本地化如何运作
从物品名字到主菜单文字, 每一条文本都使用本地化. 游戏里的每一条文本实际上是一对数据: 一个 "键" 和一个 "值" ("键值对"). 举个例子, 当玩家创建一个小世界时, 游戏用键`UI.WorldSizeSmall`来寻找当前语言的对应翻译, 若为汉语则显示 "小", 若为英语, 游戏依然会寻找键`UI.WorldSizeSmall`, 但此时它的值就不一样了, 是 "Small". 由于泰拉瑞亚的作者用英语写代码, 大部分原版的本地化键很接近它们在英语下的值. 

在tML中, 模组使用`.hjson`文件来整齐地存放本地化的键与值. 每一种语言有它自己的`.hjson`文件. 如果你熟悉JSON, 那么[HJSON](https://hjson.github.io)用起来也不会陌生. 

下面是一个简单的示例: 

文件名: **tModLoader/ModSources/ExampleMod/zh-Hans.hjson**
```
Mods: {
	ExampleMod: {
		Items: {
			ExampleItem: {
				DisplayName: 示例物品
				Tooltip: 这是一个模组物品.
			}
		}
	}
}
```

在上面这个例子中, 我们可以找到两个概念: (本地化) 键和 (本地化) 值. 键从`:`号左侧每一级的文本组合而来. `:`号右侧则是值. 这个例子传达了两个键和它们对应的值: `Mods.ExampleMod.Items.ExampleItem.Displayname`对应`示例物品`, `Mods.ExampleMod.Items.ExampleItem.Tooltip`对应`这是一个模组物品.`. 如果这些语法看起来很复杂, 别担心, tML会自动更新这些文件的. 

当模组被加载时, tML会寻找当前语言下的所有本地化文件并将它们储存在内存中. 需要向玩家显示文本时, 就用一个键去储存的数据里查询并检索出正确的文本. 翻译以`LocalizedText`对象的形式存储于内存中. 模组作者可以用方法`Language.GetText`以键获取`LocalizedText`对象. `LocalizedText`的`Value`属性可获取其中的本地化值. 另外, 方法`Language.GetTextValue`直接由键返回值: 

```cs
string hivePackDialogue = Language.GetTextValue("Mods.ExampleMod.Dialogue.ExampleTravelingMerchant.HiveBackpackDialogue"); // 直接返回要显示的文本, string
或
string hivePackDialogue = Language.GetText("Mods.ExampleMod.Dialogue.ExampleTravelingMerchant.HiveBackpackDialogue").Value; // 返回LocalizedText, 使用其Value属性获取string
或
LocalizedText hivePackDialogueLocalizedText = Language.GetText("Mods.ExampleMod.Dialogue.ExampleTravelingMerchant.HiveBackpackDialogue"); // 返回LocalizedText
string hivePackDialogue = hivePackDialogueLocalizedText.Value; // 将其Value赋值给新声明的string
```

## 本地化键
tML会自动为大多数内容分配键. 这些键的模板是`Mods.{模组名}.{类别}.{内容名}.{数据名}`, `模组名`是是模组的内部名称 (类名), `类别`依内容的类型而定, `内容名`是内容的内部名 (一般是类名), `数据名`指明了该类中的键. 

例如`ModItem`有叫`Items`的`类别`. 它也有两条数据, 分别是`DisplayName`和`Tooltip`. 如果一个叫`ExampleMod`的模组加入了一个类名为`ExampleItem`的`ModItem`, 上述两个键会生成在`.hjson`文件里: `Mods.ExampleMod.Items.ExampleItem.DisplayName`和`Mods.ExampleMod.Items.ExampleItem.Tooltip`. 

译注: 关于自动分配的键, 可以参考译者的[内置本地化指南](https://github.com/lyc-Lacewing/tMLAllInOne/blob/master/Explained/LocalizationExplained/InternalLocalization.md)

### 自定义键
如果你有许多描述相同的物品, 你可以令它们的描述指向同一个键. 要这么做, 重写描述属性并返回你要的`LocalizedText`: 
```cs
public override LocalizedText Tooltip => Language.GetText("Mods.ExampleMod.Common.SomeSharedTooltip"); // 将该类的Tooltip属性重写为你需要的LocalizedText
```
如果你有其它的物品继承此类, 你只需要在此基类中重写描述. 当然, 如果有需要, 你还可以在子类中继续重写. 

## 文本换元
如果你的本地化文件里有重复出现的文本, 或你想要使用游戏里已有的文本, 你可以使用换元来保持文件整洁. 在值中使用`{$Key}`来换元. 当游戏加载时, 这一段将会被替换为其`Key`所指的文本. 

比如说, 游戏里已经有 "右键以打开" 的翻译, 储存在键`CommonItemTooltip.RightClickToOpen`中. 模组可以使用换元复用它的值. HJSON条目`Tooltip: "{$CommonItemTooltip.RightClickToOpen}"`将被展示为用户语言的 "右键以打开" 翻译. 其它已有翻译, 诸如物品名称和常用描述也都可以这样使用. 

模组内的翻译也可以换元. 比如[示例模组本地化文件](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Localization/en-US.hjson)中的例子, `ExamplePylonTile.MapEntry: "{$Mods.ExampleMod.ItemName.ExamplePylonItem}"`就复用了键`Mods.ExampleMod.ItemName.ExamplePylonItem`对应的翻译. 

许多替换里有`{0}`或`{1}`, 它们是可供模组作者传值的占位符. 这一点在[格式化字符串](#格式化字符串)有解释. 

### 已有的物品描述 (Tooltip)
在你的模组中使用游戏提供的描述是个好主意. 使用统一的语言和既有的翻译能提升你模组的吸引力. 仔细看看下面这个常见物品描述列表. 所有这些的键都以`CommonItemTooltip.`开头. 

<details>

<summary>CommonItemTooltip包含的键</summary>

```
// 由泰拉瑞亚添加
"SpecialCrafting": "用于特殊制作",
"DevItem": "“非常适合冒充开发者！”",
"FlightAndSlowfall": "可飞行和缓慢坠落",
"RightClickToOpen": "<right>可打开",
"MinorStats": "所有属性小幅提升",
"BannerBonus": "附近的玩家针对以下情况获得奖励：",
"Counterweight": "用悠悠球击中一个敌人后，投掷平衡锤",
"EtherianManaCost10": "在保卫埃特尼亚水晶时消耗10点天国魔力",
"MinuteDuration": "{0}分钟持续时间",
"PlaceableOnXmasTree": "可放置在圣诞树上",
"RestoresLife": "恢复{0}生命值",
"RestoresMana": "恢复{0}魔力",
"SecondDuration": "{0}秒持续时间",
"String": "扩大悠悠球效力范围",
"UsesLife": "使用{0}生命值",
"UsesMana": "使用{0}魔力",
"CreativeSacrificeComplete": "复制功能已解锁",
"CreativeSacrificeNeeded": "再研究{0}个即可解锁复制功能",
"GolfBall": "会被高尔夫球杆击中",
"GolfDriver": "一种适合远距离的强大高尔夫球杆\n高尔夫球会飞得很远，垂直倾角很小",
"GolfIron": "一种最适合中等距离的圆润高尔夫球杆\n高尔夫球会飞出一段中等距离，垂直倾角适当",
"GolfPutter": "一种专门用于最后进洞球的高尔夫球杆\n高尔夫球将在短距离内贴近地面，以达到精确击球的目的",
"GolfWedge": "一种专门用于沙坑或高障碍物的高尔夫球杆\n高尔夫球将获得很大的垂直倾角，但不会飞得很远",
"Kite": "有风的日子可以放风筝\n使用<right>收卷风筝线",
"LavaFishing": "可以在熔岩中钓鱼",
"MajorStats": "所有属性大幅提升",
"MediumStats": "所有属性中幅提升",
"PressDownToHover": "按DOWN可切换悬停\n按UP可停用悬停",
"PressUpToBooster": "按住UP可以加快强化！",
"Sentry": "召唤哨兵",
"TeleportationPylon": "当附近有2个村民时，传送至另一个晶塔\n在匹配的生物群落中，每个类型只能放置一个",
"TipsyStats": "近战属性小幅提升，防御力降低",
"Whips": "你召唤的仆从将集中打击被击中的敌人",
"WizardHatDuringAnniversary": "仆从数量上限提高1个"
// 由tModLoader加入
"IncreasesMaxLifeBy": "Increases maximum life by {0}",
"IncreasesMaxManaBy": "Increases maximum mana by {0}",
"IncreasesMaxLifeByPercent": "Increases maximum life by {0}%",
"IncreasesMaxManaByPercent": "Increases maximum mana by {0}%",
"IncreasesBowDamageByPercent": "Increases bow damage by {0}%",
"IncreasesGunDamageByPercent": "Increases gun damage by {0}%",
"IncreasesSpecialistDamageByPercent": "Increases specialist ranged damage by {0}%",
"IncreasesWhipRangeByPercent": "Increases whip range by {0}%",
"IncreasesMaxMinionsBy": "Increases your max number of minions by {0}",
"IncreasesMaxSentriesBy": "Increases your max number of sentries by {0}",
"IncreasesFishingPowerBy": "Increases fishing power by {0}",
"PermanentlyIncreasesMaxLifeBy": "Permanently increases maximum life by {0}",
"PermanentlyIncreasesMaxManaBy": "Permanently increases maximum mana by {0}",
"ReducesDamageTakenByPercent": "Reduces damage taken by {0}%",
"PercentChanceToSaveAmmo": "{0}% chance to save ammo",
"PercentReducedManaCost": "{0}% reduced mana cost",
"PercentIncreasedMiningSpeed": "{0}% increased mining speed",
"PercentIncreasedMovementSpeed": "{0}% increased movement speed",
"ArmorPenetration": "{0} armor penetration",
"PercentIncreasedDamage": "{0}% increased damage",
"PercentIncreasedCritChance": "{0}% increased critical strike chance",
"PercentIncreasedDamageCritChance": "{0}% increased damage and critical strike chance",
"PercentIncreasedMagicDamage": "{0}% increased magic damage",
"PercentIncreasedMagicCritChance": "{0}% increased magic critical strike chance",
"PercentIncreasedMagicDamageCritChance": "{0}% increased magic damage and critical strike chance",
"PercentIncreasedMeleeDamage": "{0}% increased melee damage",
"PercentIncreasedMeleeCritChance": "{0}% increased melee critical strike chance",
"PercentIncreasedMeleeDamageCritChance": "{0}% increased melee damage and critical strike chance",
"PercentIncreasedMeleeSpeed": "{0}% increased melee speed",
"PercentIncreasedRangedDamage": "{0}% increased ranged damage",
"PercentIncreasedRangedCritChance": "{0}% increased ranged critical strike chance",
"PercentIncreasedRangedDamageCritChance": "{0}% increased ranged damage and critical strike chance",
"PercentIncreasedSummonDamage": "{0}% increased summon damage",
"SummonTagDamage": "{0} summon tag damage",
"PercentSummonTagCritChance": "{0}% summon tag critical strike chance"
```

</details>

### 简化作用域声明
如果用于换元的键与其所在的值有重合的作用域, 相同的部分可以被省去. 例如, 在[示例模组本地化文件](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Localization/en-US.hjson)中, `Mods.ExampleMod.ExamplePetItem.DisplayName`的值是`"{$Common.PaperAirplane}"`. 在这个例子中, tML知道在当前作用域检索, 结果是键`Mods.ExampleMod.ExamplePetItem.DisplayName`的值被检索到并且被替换进去. 在此情况下, `Mods.模组名`可以被省去. 

## 格式化字符串
模组作者可以使用[字符串格式化](https://learn.microsoft.com/en-us/dotnet/api/system.string.format?view=net-7.0#insert-a-string)在翻译中给要填的文本留出空位. 这是个C#的普通特性. 你可以用方法`string.Format`或`Language.GetTextValue`来格式化字符串. 章节[占位符](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization#placeholders)有关于此特性的更多信息. 

### 往本地化中传入值
许多翻译条目中有类似于`{0}`, `{1}`或`{MyParam}`的占位符, 表示模组作者可以往这些地方传值. 比如, 键`CommonItemTooltips.IncreasesMaxMinionsBy`对应这个值`Increases your max number of minions by {0}`. 为了在饰品中使用它, 我们要提供一个填入`{0}`的数值. 

首先, 在`hjson`文件中, 把这条翻译放进我们的物品描述中: 
```
ExampleMinionBoostAccessory: {
	DisplayName: 仆从增幅器
	Tooltip: "{$CommonItemTooltip.IncreasesMaxMinionsBy}"
}
```

接下来, 我们需要将要传入的值与这条描述"绑定". 这个饰品提高3仆从上限. 为了传入这个数值, 我们重写属性`Tooltip`并调用方法`WithFormatArgs`. 这会向占位符输入你提供的值. 推荐使用一个`static`字段来储存此类数据. 在下面这个例子中, `MaxMinionIncrease`被用在了两个不同的地方. 用字段储存数据允许模组作者同步更改实际效果与描述. 这样可以避免打错或者描述与效果不一致. 

```cs
public class ExampleMinionBoostAccessory : ModItem
{
	public static int MaxMinionIncrease = 3; // 以一个静态字段储存数值
	public override LocalizedText Tooltip => base.Tooltip.WithFormatArgs(MaxMinionIncrease); // 重写Tooltip并传入数值
	public override void UpdateEquip(Player player) {
		player.maxMinions += MaxMinionIncrease; // 将玩家的仆从上限提高3
	}
	
	// 其它代码...
}
```

### 多个占位替换
当一个本地化条目中包含多个替换引用时, 可能会有替换名重复的问题. 例如, 一个描述用了`CommonItemTooltip.IncreasesMaxManaBy`和`CommonItemTooltip.IncreasesMaxMinionsBy`的饰品会有两个占位符`{0}`. 直接传值是无效的. 模组作者可以使用特殊语法修改特定替换中的占位符. 在一个换元键后加上`@数值`可以使其中的占位符增加其所声明的`数值`. 简单来说, 就是`{$键@增加的数值}`. [示例胸甲](https://github.com/tModLoader/tModLoader/blob/1.4.4-Localization-Overhaul/ExampleMod/Content/Items/Armor/ExampleBreastplate.cs)是个很好的例子: 

**已有的CommonItemTooltip条目**
```
"CommonItemTooltip": {
	"IncreasesMaxManaBy": "Increases maximum mana by {0}",
	"IncreasesMaxMinionsBy": "Increases your max number of minions by {0}",
	// 还有更多
```

**ExampleMod/Localization/en-US.hjson**
```
ExampleBreastplate: {
	DisplayName: Example Breastplate
	Tooltip:
		'''
		This is a modded body armor.
		Immunity to 'On Fire!'
		{$CommonItemTooltip.IncreasesMaxManaBy}
		{$CommonItemTooltip.IncreasesMaxMinionsBy@1}
		'''
}
```

**ExampleMod/Content/Items/Armor/ExampleBreastplate.cs**
```cs
public class ExampleBreastplate : ModItem
{
	public static int MaxManaIncrease = 20;
	public static int MaxMinionIncrease = 1;
	public override LocalizedText Tooltip => base.Tooltip.WithFormatArgs(MaxManaIncrease, MaxMinionIncrease);
	public override void UpdateEquip(Player player) {
		player.buffImmune[BuffID.OnFire] = true; // 使玩家免疫着火了!
		player.statManaMax2 += MaxManaIncrease; // 使魔力上限增加20
		player.maxMinions += MaxMinionIncrease; // 使仆从上限增加1
	}
}
```

我们可以看出`Tooltip.WithFormatArgs(MaxManaIncrease, MaxMinionIncrease)`尝试将`MaxManaIncrease`同`{0}`绑定并将`MaxMinionIncrease`同`{1}`绑定. 因为`{$CommonItemTooltip.IncreasesMaxMinionsBy@1}`里加入了`@1`, 原本的占位符`{0}`被视为了`{1}`, 允许游戏将`MaxMinionIncrease`的值绑定到正确的位置, 显示出物品描述. 

或许这看着有点复杂, 你可能觉得 "我不管什么替换, 直接写出描述不是更简单吗?", 但使用替换是大有优势的. 有了这些已有的条目, 你模组的许多文本将自动完成本地化. 这还能显著减少人工失误. 

更复杂的示例参见[ExampleStatBonusAccessory.cs](https://github.com/tModLoader/tModLoader/blob/1.4.4-Localization-Overhaul/ExampleMod/Content/Items/Accessories/ExampleStatBonusAccessory.cs)和与之对应的[en-US.hjson](https://github.com/tModLoader/tModLoader/blob/1.4.4-Localization-Overhaul/ExampleMod/Localization/en-US.hjson#L155)

### 为动态内容组合翻译

在少数情况下, 你需要引用其它模组内容的翻译, 但你无法创建一个特定的翻译, 因为翻译组合是在运行时生成的. 

**建议:**
> 避免用某种语言的语法来组合翻译, 因为在其它语言中可能就会出错. 相反, 你应该尽可能使用独特且详尽的翻译. 比如说, 你也许会用`你`, `是`和`谁`三条文本拼成一句话`你是谁?`, 但翻译成英语时, 这句话会被组合成`youarewho?`而不是正确的`Who are you?`, 因为你是逐字拼接的. 更合适的做法是为这句话添加一条独有的翻译`SomeKey: "你是谁?"`, 这样它可以被翻译为`SomeKey: "Who are you?"`

一个典例是 "为所有弹药添加对应的无限弹药". `WithFormatArgs`能接受`LocalizedText`作为参数. 你也可以重写一个`LocalizedText`属性以返回一个完全不同的`LocalizedText` (见下)

```
InfiniteAmmoItem.DisplayName: "Infinite {0}"
```
```cs
public class InfiniteAmmoItem : ModItem
{
    Item baseAmmoItem;
    
    public override LocalizedText DisplayName => base.DisplayName.WithFormatArgs(baseAmmoItem.DisplayName);
    public override LocalizedText Tooltip => baseAmmoItem.Tooltip;
}
```

## 复数化
现代汉语采用词汇手段（名词前加数词和量词）和语法手段（名词后加“们”）表示名词的复数, 故不存在诸如`{0} minutes ago`导致的复数化问题. 但是由中文翻译成其它语言时可能要注意这一点. 参阅章节[复数化](https://github.com/tModLoader/tModLoader/wiki/Contributing-Localization#plurals)

## 聊天标签 (Chat tag)
本地化值中可以加入颜色和物品图标等 (译注: 可以参考译者的[内置本地化指南](https://github.com/lyc-Lacewing/tMLAllInOne/blob/master/Explained/LocalizationExplained/InternalLocalization.md)或[泰拉瑞亚中文维基](https://terraria.wiki.gg/zh/wiki/%E8%81%8A%E5%A4%A9)) 聊天标签. 参考[示例模组的本地化文件](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Localization/en-US.hjson)中的`ExampleTooltipsItem`. 

# 自动更新本地化
tML会在有新内容或本地化键加入时自动更新`.hjson`文件. 英语的文件`en-US.hjson`将会被用作其它语言的模板, 注释和排版将会被自动继承. 

注意, 为了效率, 游戏只会在它认为合适的时候更新本地化文件. 例如, 模组必须被放在`ModSources`文件夹里. 模组必须是本地生成的. 仅在本地化文件的修改时间早于其模组或其模组引用的模组时更新. 要当心, 如果你加载旧版的`.tmod`文件, 你的`.hjson`文件的内容也可能会被换成旧版的, 推荐使用Git或手动备份进行版本控制. 

## 加入新内容
当模组作者向模组中添加内容时, 就拿一个`ModItem`来说, 一开始并没有被本地化. 模组作者应该生成并重载模组. 一旦重载完成, `.hjson`文件将会自动更新. 英语的文件`en-US.hjson`会包含所有新内容的默认本地化条目. 非英语的文件也会包含一样的条目, 但是都被注释掉了. 为了进行本地化, 需要修改`.hjson`文件, 填入翻译, 保存 (译注: 记得以`UTF-8`编码保存), 重新生成并加载. 

## HJSON语法
`.hjson`文件包含HJSON数据. 与JSON相似, 但是可读性更高. 详细语法参见[Hjson官网](https://hjson.github.io/), 但你也可以看看下面的示例初步了解一下. 

### 多行文本

如果文本需要换行，使用下面的语法. 确保缩进一致: 

```
			SomeKey: 
			'''
			这条翻译有两行.
			这是第二行!
			'''
```
你也可以使用`\n`作为备选方案, 但从可读性考虑, 并不推荐使用此方法: 
```
			SomeKey: "这条翻译有五行, 可读性低.\n 这是第二行!\n 这是第三行!\n 这是第四行!\n 这是第五行!"
```

### 特殊字符

如果一条翻译要以`{}[],:`或空格开头, 需要将其以双引号包裹. 其它情况下双引号可以省略. 如果你需要显示`"`, 可以使用`\`转义或多行文本的语法: 
```
ExamplePetBuff: {
	DisplayName: "{$Mods.ExampleMod.Common.PaperAirplane}"
	Description: '''"让它成为你的示例宠物!"'''
}
```

### tModLoader的HJSON特性

#### 颜色

`[c/color:text]`可显示带颜色的文本.  
其`color`是16位颜色代码.  

例: 

```
		Yes: "[c/008000:yes]"
		No: "[c/FF0000:no]"
```
显示时, ‘yes’是绿色的而’no’是红色的. 

#### 物品

`[i:ItemID]`和`[i:ItemClassName]`可以在消息中显示物品.  
`ItemID`是物品的`type`. 由于模组物品没有固定的`type`, 你可以用`[i:ModName/ItemName]`.  
`ModName`是模组的类名, `ItemName`是物品的类名. 

`[i/pPrefixID:ItemID]`可以显示带前缀的物品.  
`PrefixID`是前缀的`type`.  

`[i/sStack:ItemID]`可以显示特定堆叠的物品.  
`Stack`是物品的堆叠数. 

例: 
```
		Label: "[i:ImproveGame/StarburstWand] 超模启动!"
		Tooltip: "[i/p57:HiveBackpack]是个有趣的饰品但[i/s1145:2]只是土块"
```
在这个例子中, `Label`将会显示成`(星爆魔杖的物品图标) 超模启动!`, `StarburstWand`是*更好的体验*中一个模组物品的类名.  
`Tooltip`中会有一个`无情的 蜂巢背包`和一个堆叠到1145的`土块`. 

#### 键位

`<键位名称>`可以显示绑定至某键位的按键.  
`键位名称`是键位的内部名称. 

例: 
```
		Tip: "<right>以使用其特殊攻击"
```
`<right>`会被显示为绑定至鼠标右键的按键. 

## 注释
`.hjson`文件可以包含多种注释. tML用两种Hjson注释表达不同的含义. 

以`#`开头的注释可被用做提示. 它们需要被置于对应的键的上一行. 否则就有可能在本地化文件自动更新后消失或错位.

例: 
```
...
			ExampleCanStackItem: {
				DisplayName: Example CanStack Item: 礼物袋
				# 引用一个指向游戏语言对应的 "右键以打开" 文本的键
				Tooltip: "{$CommonItemTooltip.RightClickToOpen}"
			}
...
```

英语文件中的此类注释会被自动复制进其它语言的文件, 可以用于向译者说明情况. 

以`/* */`包裹或以`//`开头的注释被tML用于指示非英语文件中未翻译的键. 译者可以将标出的值翻译并移除注释. 模组作者**不应该**将它们作为常规注释, 因为它们可能在自动更新时丢失. 

## 本地化文件名
tML会尝试将模组里的所有`.hjson`文件加载为本地化文件. 如此一来, 本地化文件可以被放在任何路径下. 但我们习惯将它们放在模组的根目录下一个叫做 "Localization" 的文件夹里. 模组生成器遵循此习惯生成`Localization/en-US.hjson`. 

文件名的开头或包含此文件的文件夹名必须是一个有效的文化编码 (culture code) 以指明其语言. 

### 文化 (语言)
tML支持以下被称之为文化 (culture) 的语言: 英语 ("en-US"), 德语 ("de-DE"), 意大利语 ("it-IT"), 法语 ("fr-FR"), 西班牙语 ("es-ES"), 俄语 ("ru-RU"), 汉语 ("zh-Hans"), 葡萄牙语 ("pt-BR"), 或波兰语 ("pl-PL"). 这些编码被用于指明`.hjson`文件所属的语言. 要添加语言支持, 参见[添加新语言](#添加新语言). 

### 前缀
模组作者可以使用一种特殊文件名格式让文件中所有的本地化条目共享同一个前缀. 此特性最常见的用途是省去`Mod.模组名`条目. 这样该文件就少了几层缩进也更容易编辑了. 绝大部分模组不会用到它们本模组前缀以外的值. 

比如说, 一个名为`Localization/en-US_Mods.ExampleMod.hjson`的文件会继承`Mods.ExampleMod`, 意味着此文件里的所有条目可以省去`Mods`和`ExampleMod`, 直接从下一级开始. 

前缀的格式遵循以下规则: 首先以文件夹, 再是以下划线分隔. 语言确定以后, 剩下的部分将被作为前缀. 下面的这些例子都是用于汉语且以`Mods.ExampleMod`作为前缀的文件名. 

```
Localization/zh-Hans_Mods.ExampleMod.hjson
Localization/zh-Hans/Mods.ExampleMod.hjson
zh-Hans_Mods.ExampleMod.hjson
zh-Hans/Mods.ExampleMod.hjson
Localization/CoolBoss/zh-Hans_Mods.ExampleMod.hjson
```

## 多个同语言文件
模组作者可以用多个`.hjson`文件来管理翻译 (尤其是当本地化条目非常多时). 假设有一个内含`en-US_Mods.ExampleMod.Items.hjson`和`en-US_Mods.ExampleMod.hjson`的模组, 文件`en-US_Mods.ExampleMod.Items.hjson`可以存放所有物品的本地化条目, 其它文件放剩下的本地化条目. 新增内容的本地化条目会被自动添加进*含有与其最相似条目的*`.hjson`文件. 

如果你要分开本地化文件, 仅需编辑英语的文件再重新生成并加载模组. 其它语言的文件将会自动调整为相同的布局. 

# 添加翻译键
新内容的本地化条目会自动添加进`.hjson`文件里, 但也可以添加自定义的键. 

## 手动添加键
要添加自定义的键, 遵循HJSON语法即可直接向本地化文件中添加. 比如, ExampleMod有一个叫做 "Common" 的类别, 因为没有被tML的类型所使用, 其中的条目都是被手动添加的. 

例如, 这是原本的`.hjson`文件: 
```
Mods: {
	ExampleMod: {
    		Common: {
			PaperAirplane: 纸飞机
		}
        
		Currencies.ExampleCustomCurrency: 示例货币
   	 }
}
```

我们可以在`PaperAirplane`下新起一行, 添加另一个`Mods.ExampleMod.Common.名称`条目. 也可以用和类别`Common`一样的语法添加一个`Uncommon`类别. 还可以像`Currencies.ExampleCustomCurrency`一样, 不单独声明类别. 下面展示了这些方法: 
```
Mods: {
	ExampleMod: {
    		Common: {
			PaperAirplane: 纸飞机
        		HotDog: 热狗
		}
        
 		Uncommon: {
            		Helicopter: 示例直升机
       		}
        
		Currencies.ExampleCustomCurrency: 示例货币
        	Currencies.DirtCurrency: 土堆
    	}
}
```

请注意, 当本地化文件被自动更新时, tML会决定如何排版, 导致条目的位置发生变化, 但不会丢失. 

## 添加可本地化的属性
模组作者可以向他们的类中添加`LocalizedText`属性以达到多种目的. 正确地实现之后, 这些属性会被自动添加进`.hjson`文件里且可以被本地化. 

[示例治疗药水](https://github.com/tModLoader/tModLoader/blob/1.4.4-Localization-Overhaul/ExampleMod/Content/Items/Consumables/ExampleHealingPotion.cs) 展示了一种用法. `ExampleHealingPotion`把一个叫做`RestoreLifeText`的`LocalizedText`属性用于动态物品描述.

基本的套路是: 

1. 向你的类中添加一个静态`LocalizedText`属性
2. 在`SetStaticDefaults`中用`this.GetLocalization`为那个属性分配一个值
3. 在需要的地方用那个属性获取翻译

例: 
```cs
public class ExampleHealingPotion : ModItem
{
	// 第一步: 创建一个静态LocalizedText属性
	public static LocalizedText RestoreLifeText { get; private set; }
	public override void SetStaticDefaults() {
		// 第二步: 将RestoreLifeText的值设为GetLocalization的结果
		RestoreLifeText = this.GetLocalization(nameof(RestoreLifeText));
	}
	public override void ModifyTooltips(List<TooltipLine> tooltips) {
		TooltipLine line = tooltips.FirstOrDefault(x => x.Mod == "Terraria" && x.Name == "HealLife");
		if (line != null) {
			// 将文本改为 "回复生命上限一半 (快速治疗时为四分之一) 的生命
			// 第三步: 获取翻译. 因为要替换占位符, 这里用了方法Format, 但其属性Value也是可以用的
			line.Text = Language.GetTextValue("CommonItemTooltip.RestoresLife", RestoreLifeText.Format(Main.LocalPlayer.statLifeMax2 / 2, Main.LocalPlayer.statLifeMax2 / 4));
		}
	}
}
```

在上面的例子中, 除了`DisplayName`和`Tooltip`, `.hjson`文件里还会自动生成`RestoreLifeText`的条目. 然后模组作者就能更新这些条目: 

```
ExampleHealingPotion: {
	DisplayName: Example Healing Potion
	Tooltip: ""
	RestoreLifeText: "{0} ({1} when quick healing)"
}
```

**注意**

`LocalizedText`实例从设计上来说要以静态储存. 理想情况下你应该在加载时注册并获取它. `ExampleHealingPotion`的例子中, `LocalizedText`在`SetStaticDefaults`里注册并缓存进属性`RestoreLifeText`. 如果缓存不了, 也可以以一点性能作为代价, 每当需要时提取一次. 

为了自动在`.hjson`里生成与`LocalizedText`属性对应的条目, 至少要在加载期间访问一次对应的属性. 

### 从LocalizedText属性中提取文本

在类中, `LocalizedText`属性可被用于向玩家显示文本: 

```cs
Main.NewText(SomeLocalizedTextProperty.Value);
```

如果要显示的文本中有占位符, 可以用方法`Format`填入值, 该方法接受与文本中占位符数量相同的参数: 


```cs
Main.NewText(SomeLocalizedTextPropertyWithPlaceholders.Format(Main.LocalPlayer.statLifeMax2, Main.LocalPlayer.statManaMax2));
```

### 可继承的本地化属性

使用继承时, 可以在子类中用仅可`get`的属性或非静态属性达到与静态属性一样的效果 (子类不会继承父类的静态属性). 继承的性质允许你复用逻辑和翻译, 保持代码与`.hjson`文件整洁, 减少不必要的重复. 

以物品为例, 想象一下, 一个模组里的数个物品共用一个基类. 基类中可以有一个`LocalizedText`属性给每一个子类. 这个属性得在`SetStaticDefaults`时被访问以自动添加进`.hjson`文件里. 

**基类: MyBaseClass** 
```cs
public LocalizedText SpecialMessage => this.GetLocalization(nameof(SpecialMessage));
public override void SetStaticDefaults() {
	_ = SpecialMessage;
}
```

如果类`ClassA`和`ClassB`都继承自`MyBaseClass`, `.hjson`文件中会自动填入键`SpecialMessage`的条目: 

```
ClassA: {
	DisplayName: Class A 
	Tooltip: ""
	SpecialMessage: Mods.ExampleMod.Items.ClassA.SpecialMessage
}
ClassB: {
	DisplayName: Class B
	Tooltip: ""
	SpecialMessage: Mods.ExampleMod.Items.ClassB.SpecialMessage
}
```

如果子类重写了`SetStaticDefaults`, 记得保留`base.SetStaticDefaults()`来执行父类的代码 (以访问LocalizedText属性). 

`GetLocalization`生成的键的格式是: `Mods.{模组名}.{类别名}.{内容名}.{后缀}`的格式生成键. 如果需要一个不符合此格式的键, 应当改用`Language.GetOrRegister("完整的键");`. 注意, 由于C#的设计, `GetLocalization`必须以`this.`调用, 这不可被省略. 通过在被继承的属性中使用完整的键, 该属性的翻译可以用单个键保存 (而不是为每个子类创建一个键), 需要不同翻译的子类仍可以重写该属性, 通过`this.GetLocalization`使用它们自己的键. 

### 注册可本地化的属性
要使这些键被自动注册并添加进`.hjson`文件中, 模组作者需要在模组加载时访问`getter`. 比较简单的方法是在方法`SetDefaults`或`SetStaticDefaults`里访问: 

```cs
public override void SetDefaults() {
	_ = 随便什么信息;

    ... 其它代码
}
```

### 从可本地化的属性中获取文本
在代码的其它地方, 该属性可被用于向用户显示本地化的文本: 

```cs
Main.NewText(Language.GetTextValue(this.GetLocalizationKey("随便什么信息")));
```

### 另一个示例

[示例箱子](https://github.com/tModLoader/tModLoader/blob/1.4/ExampleMod/Content/Tiles/Furniture/ExampleChest.cs)是一个使用自定义键的例子. 默认情况下, tML会为每个`ModTile`注册一个`Mods.{模组名}.Tiles.{内容名}.MapEntry`格式的键. 这使得为物块添加地图条目变得简单. (地图条目控制大地图中鼠标停留在物块上时显示的文字.) 然而, `ExampleChest`需要两个地图条目. 新的键可以被轻易地添加进本地化文件中: 

```cs
AddMapEntry(new Color(200, 200, 200), this.GetLocalization("MapEntry0"), MapChestName);
AddMapEntry(new Color(0, 141, 63), this.GetLocalization("MapEntry1"), MapChestName);
```

这样做的结果就是本地化文件中多出了这些键, 可以被翻译成其它语言: 
```
ExampleChest: {
	MapEntry0: Example Chest
	MapEntry1: Locked Example Chest
}
```

在这个文件的另一个地方, 这些键以`GetLocalization`被动态获取: 
```cs
public override LocalizedText ContainerName(int frameX, int frameY) {
	int option = frameX / 36; // 用 frameY 判断箱子处于第几帧, 对应不同的状态
	return this.GetLocalization("MapEntry" + option);
}
```

这种方法对动态的键很有用. 

# `ModType`与`ILocalizedModType`
有实现自定义`ModType`的模组可以实现`ILocalizedModType`来轻松地推行本地化. 仅需在类继承后面加上`, ILocalizedModType`并添加`public string LocalizationCategory => "自定义类别名称";`以实现属性`LocalizationCategory`. 对于你自定义的`ModType`中的每一个`LocalizedText`, 你可以使用`public virtual LocalizedText 随便什么名字 => this.GetOrAddLocalization(nameof(随便什么名字), 默认显示的随便什么名字);`以允许它们像已有`ModType`里的`Localizedtext`属性一样成为`.hjson`文件中分好类的属性. 

**深入解析:** `GetLocalization`是一个用于简化代码和防止打错字的helper方法. 其等同于传入了完整键的方法`Language.GetOrRegister`. 相似的, `GetLocalizedValue`等同于对应写法的`Language.GetTextValue`. 若要获取生成的键, 可以使用`GetLocalizationKey`. 

`GetLocalization`和`Language.GetOrRegister`有一个可选的`makeDefaultValue`参数, 决定了键不存在时默认生成的值. 例如, 传入`() => ""`会导致默认值变为一个空字符串而不是键. 若不必进行本地化或内部名已经写得很清楚, 模组作者可以传入`PrettyPrintName`以达到显示加了空格的内部名的效果. 如果不必要做本地化或者内部名已经写得很清楚了, 你可以用这种方法. 

# 添加新语言
默认情况下, tML仅会为模组生成和更新已有的`.hjson`文件. 要添加新的语言, 仅需新建一个文本文档并将其重命名为与现有本地化文件相同的格式. 文件名或其路径中需要包含对应的语言代码: 英语 ("en-US"), 德语 ("de-DE"), 意大利语 ("it-IT"), 法语 ("fr-FR"), 西班牙语 ("es-ES"), 俄语 ("ru-RU"), 汉语 ("zh-Hans"), 葡萄牙语 ("pt-BR"), 或波兰语 ("pl-PL"). 创建好文件并确保其有正确的文件扩展名`.hjson`之后, 重新生成模组. 这样该文件就会更新出可供翻译的条目, 其它文件也会依据英语的`.hjson`文件的结构一并生成. 

非英语本地化文件的注释和结构都从英语的文件继承. 如果你要向译者致谢, 将他们写进英语文件顶部的一条注释中, 这样他们也会被传到其它语言文件里. 

模组作者可以按照喜好自由地规划英语文件, 其它语言文件将会更新以匹配英语文件. 如果你分割了本地化文件, 也仅需编辑英语文件. 相似地, 从英语文件中移除的键也会从其它文件中移除. 总的来说, 模组作者一般仅需对英语文件进行操作, 其它语言文件将会自动调整. 

---

```
Note to wiki editors: 
This page is NOT an exact translation of the original page. 
There are additions, deletions or modifications based on nature the target language. 
If you want to translate this page, you are suggested to refer to the original page. 
```

---

````
Translated by Lacewing (Lacewing#1697 on Discord). 
````