## 前言

Fabric 是 Minecraft 一款非官方的模组 API,与 Forge mod 不同。它以轻量级和高性能为设计目标,专注于支持新版本的 Minecraft。

Fabric 和 Forge 在各自的加载编译流程上差别很大，所以你很难看见有同时支持二者的 mod，除非做了兼容性处理

Fabric 还支持 kotlin 编程

<br>

## 环境配置

> 以下及后续的所有教程均基于 fabric 官方 wiki 总结精华与踩坑得来，如有觉得下方内容不够详细的，可以查看原网站：https://fabricmc.net/wiki/zh_cn:tutorial:setup

<br>

### 安装必要前置

`JDK17` 及以上版本（硬性要求，低于此版本的 JDK 无法编译 Gradle）

`Intellij Idea` 任意版本

此外，我们还需要使用 fabric 提供的 `fabric-example-mod` 作为第一个 mod 的开发模板  
前往官网的模板生成器，生成你想要的对应 MC 版本模板，我这里使用的是 1.20 的  
https://fabricmc.net/develop/template/

<br>

### 配置 gradle

> 众所周知，这是最最最最恶心的环节，有可能卡的你生无可恋并出现无法预知的弱智错误，在此处我将详细介绍我所踩到的坑以及目前遇到错误的解决方案

#### 解压 template 文件

把上一步下载好的 ZIP 文件解压到任意一个文件夹内，并确保全路径绝对不能包含中文和其他特殊符号（下划线可以）

删除多余的 `RAEDME.md` `.github` `LICENSE`

然后使用 IDEA 打开该项目文件夹  
紧接着此时 IDEA 会自动开始配置 gradle，立马点击停止！！！等我们配置代理和镜像源后再重新构建，否则巨慢！！！而且可能直接下载到一半就报错

<br>

#### 修改镜像源

> 配置镜像源以及代理可以参考这个网站：[Fabric 镜像与代理配置](https://fabricmc.cn/2021/06/28/%E5%A6%82%E4%BD%95%E5%8A%A0%E9%80%9FFabric%E6%A8%A1%E7%BB%84%E7%9A%84%E6%9E%84%E5%BB%BA/)

将 `settings.gradle` 替换为以下内容

```groovy
pluginManagement {
    repositories {
        maven {
            name = 'Fabric'
            url = 'https://repository.hanbings.io/proxy'
        }
        gradlePluginPortal()
    }
}
```

向 `build.gradle` 添加如下内容（如果已存在，则直接替换掉）

```groovy
repositories {
    maven {
        url 'https://maven.aliyun.com/nexus/content/groups/public'
    }
    maven {
        url 'https://repository.hanbings.io/proxy'
    }
}
```

<br>

#### 配置外部代理

是的，即使你配置了镜像源，可能依然会非常卡，如果你掌握了科学上网的方法，那么可以尝试添加一个代理

打开项目目录下的 `gradle.properties` 文件

添加如下代码

- proxyHost 即为代理地址（我这边默认就是 127.0.0.1）
- proxyPort 为你开的代理软件对应的端口

```conf
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=10809
```

实际上，如果你参考其他教程，可能会在上方顺便注册 https 代理，但是我一旦注册了必定报错而无法下载对应库，所以我就索性删掉了，只留下比较核心的内容

<br>

### 构建 gradle 与反编译

#### 构建

构建的方式很简单，打开 IDEA，右键点击项目目录，选择“重新构建”即可

构建成功的标志是你在构建输出窗口看见 `BUILD SUCCESSFUL`

<br>

#### 反编译

该步骤必须要在 gradle 构建成功后执行，否则会一直卡着动不了

使用管理员权限打开命令提示符，进入项目所在的目录  
执行该代码：`gradlew genSources`

等待时长浮动较大，反正最后构建成功会给你一个大大的绿色提示滴~

<br>

## 第一个物品

> 此系列参考油管教程：https://www.youtube.com/watch?v=fQYNhfAwLf8&list=PLKGarocXCE1EeLZggaXPJaARxnAbUD8Y_&index=2

制作物品所需的图像资源包：https://url.kaupenjoe.net/yt331/assets

由于油管上最新的教程只有 1.19 的，而目前代码风格有所变动，我会针对 1.20 新改动做出对应解释

<br>

### 物品注册

首先请各位按照下图所示文件结构，在对应位置新建空的 Java 类文件，如果文件已存在就不管  
新建的文件将在后续逐步填充，不要在意

![](./img/fb-env/fe1.png)

#### TutorialMod.java

该文件原始的名称应该是 `ExampleMod.java`

由于 Java 特性，类名必须和文件名一致，故我们可以使用快捷键 `shift+f6` 快速执行主类名称的更改，同时任何引用此类的位置的名称都会做出对应更改，十分方便！

```java
package com.example;

import com.example.item.ModItems;
import net.fabricmc.api.ModInitializer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

// 主要类，用于初始化模组
public class TutorialMod implements ModInitializer {

    // 定义模组的MOD_ID
    public static final String MOD_ID = "tutorialmod";
    public static final Logger LOGGER = LoggerFactory.getLogger(MOD_ID);

    @Override
    public void onInitialize() {
        // 在模组初始化时注册自定义物品
        ModItems.registerModItems();
    }
}
```

<br>

#### TutorialModClient.java

定义客户端，目前暂且用不到，先写入以下代码

```java
package com.example;

import net.fabricmc.api.ClientModInitializer;

public class TutorialModClient implements ClientModInitializer {
    @Override
    public void onInitializeClient() {

    }
}
```

<br>

#### ModItems.java

在该文件内执行物品的注册操作

当我们注册物品时，务必牢记注册物品所用到的物品名！！！后续为物品添加纹理以及模型时都会用到的

```java
package com.example.item;

import com.example.TutorialMod;
import net.fabricmc.fabric.api.item.v1.FabricItemSettings;
import net.minecraft.item.Item;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.util.Identifier;

public class ModItems {

    // 自定义物品示例：ZER_DIAMOND
    public static final Item ZER_DIAMOND = regItem("zer_diamond",
            new Item(new FabricItemSettings()));

    // 自定义物品示例：ZER_INGOT
    public static final Item ZER_INGOT = regItem("zer_ingot",
            new Item(new FabricItemSettings()));

    // 定义注册物品到游戏的物品注册表的方法
    // 物品注册需要接收两个参数：1.MODID 2.物品名称
    private static Item regItem(String name, Item item) {
        return Registry.register(Registries.ITEM, new Identifier(TutorialMod.MOD_ID, name), item);
    }

    // 在模组初始化时调用，用于注册自定义物品
    public static void registerModItems() {
        TutorialMod.LOGGER.debug("TutorialMod正在注册Items，MOD_ID:" + TutorialMod.MOD_ID);
        // 在这里添加更多的自定义物品注册逻辑
    }
}
```

<br>

### 资源文件

为 resources 文件夹新增如下图所示结构
![](./img/fb-env/fe2.png)

下面将介绍对应结构的作用

1. `lang/en_us.json` 定义物品或者方块在游戏内部显示的名称
2. `models/item/zer_diamond.json` 定义模型
3. `textures/item/zer_diamond.png` 定义模型对应的贴图
4. `fabric.mod.json` 模组属性设置
5. `tutorialmod.mixins.json` 模组混合属性设置

<br>

#### en_us.json

首先当然是配置我们的语言文件啦

对于我们开发者来说，推荐首先使用英文，后续可以逐步补全中文翻译

所以`en_us.json`表示当你的 MC 客户端使用英文时显示的翻译，对于的中文翻译文件就是`zh_cn.json`

填入代码

```json
{
	// 注意格式 item.MOD_ID.物品名称
	// 所以知道为什么我要叫你牢记物品注册时用到的名称了吧！
	"item.tutorialmod.zer_diamond": "Zhiller's Diamond",
	"item.tutorialmod.zer_ingot": "Zhiller's Ingot"
}
```

<br>

#### zer_diamond.json

在这里配置物品的模型文件

因为当前物品只是一个手拿物品，所以模型可以说就是一个简单的平面，不需要做过多修饰  
写入以下代码

```json
{
	"parent": "item/generated",
	"textures": {
		"layer0": "tutorialmod:item/zer_diamond"
	}
}
```

> 另外一个方块文件 `zer_ingot.json` 大家直接如法炮制即可

<br>

#### 纹理

在 textures 文件夹下对应的 block 以及 item 文件夹添加方块和物品的纹理文件

注意，纹理文件名必须和方块或物体注册名完全一致！使用 png 格式！

<br>

#### fabric.mod.json

目前仅需修改我打了注释的几个地方的内容，其他的不管

```json
{
	"schemaVersion": 1,
	"id": "tutorialmod", // 在这修改MOD_ID
	"version": "${version}",
	"name": "Example mod", // 你的MOD名字
	"description": "This is an example description! Tell everyone what your mod is about!",
	"authors": ["zhiller"], // MOD作者
	"contact": {
		"homepage": "https://fabricmc.net/",
		"sources": "https://github.com/FabricMC/fabric-example-mod"
	},
	"license": "CC0-1.0",
	"environment": "*",
	"entrypoints": {
		// main入口点文件所在位置
		"main": ["com.example.TutorialMod"],
		// client客户端入口点文件所在位置
		"client": ["com.example.TutorialModClient"]
	},
	// 混合文件所在位置
	"mixins": ["tutorialmod.mixins.json"],
	"depends": {
		"fabricloader": ">=0.14.21",
		"minecraft": "~1.20.1",
		"java": ">=17",
		"fabric-api": "*"
	},
	"suggests": {
		"another-mod": "*"
	}
}
```

<br>

#### tutorialmod.mixins.json

由于我们在 src 根目录下的 mixin 包内文件没有做任何修改，故下方代码也不需要做任何修改

但请注意要修改该文件的文件名，开头必须是你的 `MOD_ID` 哦！

```json
{
	"required": true,
	"package": "com.example.mixin",
	"compatibilityLevel": "JAVA_17",
	"mixins": ["ExampleMixin"],
	"injectors": {
		"defaultRequire": 1
	}
}
```

<br>

### 运行！

没错！你目前已经创建了第一个属于你的个人物品，现在进入客户端来看看成果把~

依次点击：`gradle->fabric->runClient` 执行客户端

![](./img/fb-env/fe3.png)

<br>

新建一个超平坦世界，输入该指令获取你注册的物品：`give @p tutorialmod:zer_diamond 1`

![](./img/fb-env/fe4.png)

## 创造模式物品栏

<br>

### 添加到当前已有物品栏

> 再添加自定义的创造模式物品栏之前，请确保你的确有这个需求！否则建议直接添加到当前已有的物品栏内部

创建新文件：`com/example/item/ModItemGroup.java`

```java
package com.example.item;


import net.fabricmc.fabric.api.itemgroup.v1.FabricItemGroup;
import net.fabricmc.fabric.api.itemgroup.v1.FabricItemGroupEntries;
import net.fabricmc.fabric.api.itemgroup.v1.ItemGroupEvents;
import net.minecraft.item.ItemGroup;
import net.minecraft.item.ItemGroups;
import net.minecraft.item.ItemStack;
import net.minecraft.text.Text;


public class ModItemGroup {

    // 定义我们要把什么物品添加到物品栏里面
    private static void addItemsToGroup(FabricItemGroupEntries entries){
        entries.add(ModItems.ZER_DIAMOND);
        entries.add(ModItems.ZER_INGOT);
    }

    // 注册物品栏方法
    public static void registerItemGroup(){
        // 添加到已有的物品栏INGREDIENTS里面
        ItemGroupEvents.modifyEntriesEvent(ItemGroups.INGREDIENTS).register(ModItemGroup::addItemsToGroup);
    }
}
```

<br>

最后别忘了在 `TutorialMod.java` 里面执行初始化

```java
package com.example;

import com.example.item.ModItemGroup;
import com.example.item.ModItems;
import net.fabricmc.api.ModInitializer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class TutorialMod implements ModInitializer {
	...

	@Override
	public void onInitialize() {
		ModItems.registerModItems();

        // 在这里注册物品栏哦！
        // 由于我们需要使用到自定义物品，所以注册物品栏顺序必须晚于物品注册
		ModItemGroup.registerItemGroup();
	}
}
```

最终结果：  
![](./img/fb-inv/fi1.png)

<br>

### 注册自定义物品栏

其他内容保持不变，我们仅需修改 `ModItemGroup.java`

```java
package com.example.item;


import com.example.TutorialMod;
import net.fabricmc.fabric.api.itemgroup.v1.FabricItemGroup;
import net.fabricmc.fabric.api.itemgroup.v1.FabricItemGroupEntries;
import net.fabricmc.fabric.api.itemgroup.v1.ItemGroupEvents;
import net.minecraft.item.ItemGroup;
import net.minecraft.item.ItemGroups;
import net.minecraft.item.ItemStack;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.text.Text;
import net.minecraft.util.Identifier;


public class ModItemGroup {
    // 注册的物品栏
    private static final ItemGroup ZER_GROUP = FabricItemGroup.builder()
            // 物品栏显示的名称
            // Text.translatable表示根据lang文件下的不同语言来翻译名称
            .displayName(Text.translatable("itemgroup.tutorialmod"))

            // 选择一个物品作为物品栏图标
            .icon(() -> new ItemStack(ModItems.ZER_INGOT))

            // 选择你要加入物品栏的物品
            .entries((displayContext, entries) -> {
                entries.add(ModItems.ZER_DIAMOND);
                entries.add(ModItems.ZER_INGOT);
            }).build();

    // 注册物品栏
    public static void registerItemGroup() {
        TutorialMod.LOGGER.debug(TutorialMod.MOD_ID + "已经创建好物品栏了");

        // 在这里注册自定义物品栏
        Registry.register(
                Registries.ITEM_GROUP,
                new Identifier(TutorialMod.MOD_ID, "zer_group"),
                ZER_GROUP
        );
    }
}
```

<br>

别忘了在 en_us.json 或者 zh_cn.json 添加对应名称翻译哦！

```json
{
	"item.tutorialmod.zer_diamond": "Zhiller's Diamond",
	"item.tutorialmod.zer_ingot": "Zhiller's Ingot",

	"itemgroup.tutorialmod": "Zhiller's ItemGroup"
}
```

<br>

打开游戏，发现创造模式下物品栏多出来了一个，点进去就可以看见我们注册好的物品栏了

物品栏内部有两个我们上一期制作的自定义物品

![](./img/fb-inv/fi2.png)

<br>

## 创建方块

> 资源文件下载：https://kaupenjoe.net/files/Minecraft/Videos/MBKJ2023-51%20-%20Fabric%201.20.X%20Blocks/

<br>

### 方块注册流程

方块注册和物品注册基本上没啥区别

但要注意，对于方块，需要注册两次：一次是注册手上拿着的物品，一次是注册放在地上的方块

创建管理方块注册的类：`com/example/block/ModBlocks.json`

```java
package com.example.block;

import com.example.TutorialMod;
import net.fabricmc.fabric.api.item.v1.FabricItemSettings;
import net.fabricmc.fabric.api.object.builder.v1.block.FabricBlockSettings;
import net.minecraft.block.Block;
import net.minecraft.block.Blocks;
import net.minecraft.item.BlockItem;
import net.minecraft.item.Item;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.sound.BlockSoundGroup;
import net.minecraft.util.Identifier;

public class ModBlocks {

    // copyOf表示直接复制现有方块的属性（比如硬度、可开采性以及亮度可堆叠啥的）
    // sounds表示直接复制现有方框的声音（如行走表面声音、破坏声音）
    public static final Block ZER_BLOCK = regBlock("zer_block",
            new Block(FabricBlockSettings.copyOf(Blocks.IRON_BLOCK).sounds(BlockSoundGroup.AMETHYST_BLOCK)));

    public static final Block RAW_ZER_BLOCK = regBlock("raw_zer_block",
            new Block(FabricBlockSettings.copyOf(Blocks.IRON_BLOCK).sounds(BlockSoundGroup.AMETHYST_BLOCK)));

    // 再注册可放置的实体方块
    private static Block regBlock(String name, Block block){
        regBlockItem(name, block);
        return Registry.register(
                Registries.BLOCK,
                new Identifier(TutorialMod.MOD_ID,name),
                block
        );
    }

    // 先注册方块物品
    private static Item regBlockItem(String name, Block block){
        return Registry.register(
                Registries.ITEM,
                new Identifier(TutorialMod.MOD_ID,name),
                new BlockItem(block, new FabricItemSettings())
        );
    }

    public static void registerModBlocks(){
        TutorialMod.LOGGER.debug(TutorialMod.MOD_ID+"的方块已加载完毕");
    }
}
```

<br>

最后别忘了在主入口中执行初始化

```java
package com.example;

import com.example.block.ModBlocks;
import com.example.item.ModItemGroup;
import com.example.item.ModItems;
import net.fabricmc.api.ModInitializer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class TutorialMod implements ModInitializer {
    ...

	@Override
	public void onInitialize() {
		ModItems.registerModItems();
		ModBlocks.registerModBlocks(); // 初始化方块管理类
		ModItemGroup.registerItemGroup();
	}
}
```

<br>

### 材质纹理处理

#### blockstates

方块状态文件夹下创建 `blockstates/zer_block.json`

```json
{
	"variants": {
		"": {
			"model": "tutorialmod:block/zer_block"
		}
	}
}
```

<br>

#### models/block

在这里设置方块的渲染模型，创建文件 `models/block/zer_block.json`

```json
{
	"parent": "block/cube_all",
	"textures": {
		"all": "tutorialmod:block/zer_block"
	}
}
```

<br>

#### models/item

创建手持方块的模型，创建文件 `models/item/zer_block.json`

这里不需要写啥了，直接继承前面定义的方块模型即可

```json
{
	"parent": "tutorialmod:block/zer_block"
}
```

<br>

然后就是翻译文件的内容添加啦，这里就不多做赘述了，大家按照这个模板来做就可以了

<br>

### 成果展示

![](./img/fb-inv/fi3.png)

<br>

## 新配方

<br>

### 工作台配方

为便于你快速创建配方，可以直接去这个网站上通过拖拽的方式创建属于你的配方表，它会自动生成 json 文件，非常方便！  
之后你仅需修改对应的物品方块为我们自定义的即可  
https://crafting.thedestruc7i0n.ca/

<br>

#### 无序合成配方

此配方用于将宝石块分解为单个锭

注意，`minecraft:crafting_shapeless` 表示无序合成

```json
{
	"type": "minecraft:crafting_shapeless",
	"ingredients": [
		{
			"item": "tutorialmod:zer_block"
		}
	],
	"result": {
		"item": "tutorialmod:zer_ingot",
		"count": 9
	}
}
```

<br>

#### 有序合成配方

下面展示使用 9 个 ingot 合成一个 block 的配方

```json
{
	"type": "minecraft:crafting_shaped",
	"pattern": ["###", "###", "###"],
	"key": {
		"#": {
			"item": "tutorialmod:zer_ingot"
		}
	},
	"result": {
		"item": "tutorialmod:zer_block",
		"count": 1
	}
}
```

<br>

### 熔炉配方

熔炉配方很简单就三个，一个是普通熔炉配方、一个是高炉配方，当然还有一个烟熏炉配方（只不过我们自定义的物品都是矿石，没法用烟熏炉）

下面是普通熔炉配方（smelting）

```json
{
	"type": "minecraft:smelting",
	"category": "misc",
	"cookingtime": 200,
	"experience": 1,
	"group": "zer",
	"ingredient": {
		"item": "tutorialmod:zer_diamond"
	},
	"result": "tutorialmod:zer_ingot"
}
```

这个是高炉配方（blasting）

```json
{
	"type": "minecraft:blasting",
	"category": "misc",
	"cookingtime": 100,
	"experience": 0.7,
	"group": "copper_ingot",
	"ingredient": {
		"item": "minecraft:copper_ore"
	},
	"result": "minecraft:copper_ingot"
}
```

<br>

## 挖掘等级与掉落物
