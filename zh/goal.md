# 速通目标（Speedrun Goals）

## 概述
本模组通过加载数据包中 `data/<namespace>/neospeedzero/goals/<path>.json` 路径下的 JSON 文件，定义游戏内的「速通目标」。每个目标可配置触发条件（如完成特定进阶、收集特定物品等），并支持自定义目标图标与显示文本。本文将详细说明数据包的格式规范与各字段含义。

---

## 基础定义（Definitions）

### IconState（图标状态模式）
用于控制目标图标与系统生成图标的冲突处理逻辑，取值如下：
- `icon`：完全使用用户指定的图标（忽略系统生成的图标）。
- `covers_gen`（默认值）：当用户图标与系统生成的图标的数据组件（Data Components）存在冲突时，优先使用用户指定的图标的组件。
- `covers_icon`：当用户图标与系统生成的图标的数据组件（Data Components）存在冲突时，优先使用系统生成的图标的组件。

### StatedIcon（带状态的图标对象）
一个可选配置的对象，用于为特定触发条件自定义图标。结构如下：
```json
{
  /* 可选 */ "replace": "<IconState>",  // 图标状态模式（icon/covers_gen/covers_icon）
  /* 其余字段 */ ...                    // 必须符合 ItemStack 格式（物品堆定义）
}
```
- `replace` 字段可选，用于声明当前图标的冲突处理策略（默认继承目标的 `icon` 字段策略）。
- 其余字段需遵循 Minecraft 的 `ItemStack` 格式（如 `{"item": "minecraft:diamond"}`）。

### HomogenousItemList（同构物品列表）
表示一组同类型物品的集合，支持以下三种形式：
- 单个物品 ID（如 `"minecraft:diamond"`）。
- 物品标签 ID（以 `#` 开头，如 `"#minecraft:logs_that_burn"`）。
- 上述两种形式的列表（如 `["minecraft:diamond", "#minecraft:logs_that_burn"]`）。

---

## 目标配置（Goal 结构）
速通目标的核心配置位于 `Goal` 对象中，包含以下字段：

| 字段名     | 类型       | 说明                                                                 |
|------------|------------|----------------------------------------------------------------------|
| `icon`     | `ItemStack`| 目标的默认图标（若未在触发条件中自定义图标，则使用此图标）。          |
| `display`  | `Component`| 目标的显示文本（可选，遵循 Minecraft 文本组件格式，如 JSON 文本）。   |
| `predicates`| 数组       | 触发目标达成的条件列表（**至少配置一个**）。每个元素为以下类型之一。|

---

### Predicates（触发条件）
`predicates` 是一个数组，每个元素表示一种触发目标达成的条件。以下是支持的触发条件类型：

#### 类型 1：基于进阶触发的条件
当玩家获得指定进度（Advancement）时触发目标达成。  
**结构示例**：
```json
{
  "advancement": "minecraft:end/kill_dragon",    // 进度 ID（必填）
  "icon": {                                      // 可选：自定义图标（StatedIcon 格式）
    "replace": "covers_icon",
    "item": "minecraft:emerald"
  }
}
```
- `advancement`：必填，指定触发目标达成的进度 ID（如 `minecraft:end/kill_dragon`）。
- `icon`：可选，自定义该触发条件的图标（遵循 `StatedIcon` 格式）。

---

#### 类型 2：基于物品列表的全选触发条件
要求玩家收集列表中**所有**物品（或标签包含的所有物品）。  
**结构示例**：
```json
[
  "#minecraft:logs_that_burn"  // 收集主世界所有类型的木头（标签）
]
```
或
```json
[
  "minecraft:diamond", 
  "#minecraft:logs_that_burn"  // 同时收集钻石和主世界所有类型的木头
]
```
- 直接填写 `HomogenousItemList`（单个物品 ID、标签 ID 或列表）。

---

#### 类型 3：基于物品列表及谓词的触发条件
要求玩家收集列表中**任意一个**物品（或标签包含的任意一个物品）。如果`select`指定为`all`，则收集列表中的**所有**物品（或标签中的所有物品）。  
**结构示例**：
```json
{
  "items": ["minecraft:diamond_sword", "minecraft:netherite_axe"],  // 任选其一
  "select": "any",             // "any" 表示任选，"all" 表示全选
  "item_predicate": {          // 可选：额外过滤物品的条件（如附魔、耐久等）
    "components": {
      "minecraft:damage": 3
    },
    "predicates": {
      "minecraft:enchantments": {
        "enchantments": ["minecraft:sharpness"],
        "levels": {
          "min": 4
        }
      }
    }
  },
  "icon": {                    // 可选：自定义图标
    "replace": "icon",
    "item": "minecraft:redstone"
  }
}
```
- `items`：必填，指定候选物品列表（`HomogenousItemList`）。
- `select`：固定值为 `"any"`（表示任选其一）。
- `item_predicate`：可选，用于对列表中的物品添加额外筛选条件（如附魔、耐久等，遵循 Minecraft「物品堆叠谓词」格式）。
- `icon`：可选，自定义该触发条件的图标（`StatedIcon` 格式）。

---

#### 类型 4：基于物品谓词的触发条件
要求玩家收集**任意一个满足指定谓词**的物品（不依赖预定义列表）。  
**结构示例**：
```json
{
  "item_predicate": {          // 必填：物品筛选条件
    "predicates": {
      "minecraft:enchantments": {
        "enchantments": ["minecraft:unbreaking"],
        "levels": {
          "min": 2,
          "max": 3
        }
      }
    }
  },
  "icon": {                    // 可选：自定义图标
    "replace": "covers_gen",
    "item": "minecraft:diamond_sword"
  }
}
```
- `item_predicate`：必填，定义物品的筛选条件（遵循 Minecraft `ItemPredicate` 格式）。
- `icon`：可选，自定义该触发条件的图标（`StatedIcon` 格式）。

---

## 注意事项
1. **触发条件互斥性**：`predicates` 数组中的多个条件**同时生效**，即玩家需满足所有条件才能达成目标（除非特别说明）。
2. **图标优先级**：若同一触发条件中同时配置了 `icon` 和系统默认图标，按 `IconState` 模式决定最终显示（默认 `covers_gen`）。
3. **物品标签支持**：使用 `#<tag_id>` 形式可匹配标签下的所有物品（如 `#minecraft:tools` 匹配所有工具）。
4. **文本组件**：`display` 字段支持 Minecraft 所有文本格式（如颜色代码 `\u00A7`、翻译键 `{translate:...}` 等）。

---

## 示例配置
以下是一个完整的速通目标配置示例，要求玩家完成「解放末地」进度，并收集任意一个带效率2~3附魔的钻石镐或下界合金镐：

```json
{
  "icon": {
    "item": "minecraft:diamond_pickaxe",
    "replace": "covers_icon"
  },
  "display": [
    {
      "text": "速通目标：",
      "color": "gold"
    },
    {
      "text": "钻石大师",
      "bold": true
    }
  ],
  "predicates": [
    {
      "advancement": "minecraft:end/kill_dragon",
      "icon": {
        "replace": "icon",
        "item": "minecraft:dragon_egg"
      }
    },
    {
      "items": ["minecraft:diamond_pickaxe", "minecraft:netherite_pickaxe"],
      "select": "any",
      "item_predicate": {
        "minecraft:enchantments": {
          "enchantments": ["minecraft:efficiency"],
          "levels": {"min": 2, "max": 3}
        }
      },
      "icon": {
        "replace": "covers_icon",
        "item": "minecraft:diamond_pickaxe"
      }
    }
  ]
}
```

此配置表示：玩家需完成「解放末地」（`end/kill_dragon`）进度（使用龙蛋图标），并拥有一个带效率2~3附魔的钻石镐或下界合金镐，才能达成该速通目标。目标图标默认使用钻石镐（若与系统生成的图标的数据组件冲突，则**覆盖**系统图标的组件），触发进度时的图标使用龙蛋，获得镐子时的图标使用钻石镐（若与若与系统生成的图标的数据组件冲突，则**采用**系统图标的组件）。最终显示文本为金色标题「速通目标：钻石大师」。
