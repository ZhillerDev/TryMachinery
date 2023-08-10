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
