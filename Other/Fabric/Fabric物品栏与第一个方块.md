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
