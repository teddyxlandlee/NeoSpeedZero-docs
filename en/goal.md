# Speedrun Goals

## Overview
This mod defines in-game "Speedrun Goals" by loading JSON files located at the path `data/<namespace>/neospeedzero/goals/<path>.json` within data packs. Each goal can be configured with trigger conditions (e.g., completing specific advancements, collecting specific items), and supports custom goal icons and display text. This document details the data pack format specifications and the meanings of each field.

---

## Definitions

### IconState (Icon State Mode)
Used to control the conflict resolution logic between the goal icon and system-generated icons. The possible values are:
- `icon`: Use the user-specified icon exclusively (ignores system-generated icons).
- `covers_gen` (default): When conflicts occur between the user-specified icon and system-generated icon's data components, prioritize the user-specified icon's components.
- `covers_icon`: When conflicts occur between the user-specified icon and system-generated icon's data components, prioritize the system-generated icon's components.

### StatedIcon (Stateful Icon Object)
An optional configuration object for customizing icons under specific trigger conditions. Its structure is as follows:
```json
{
  /* Optional */ "replace": "<IconState>",  // Icon state mode (icon/covers_gen/covers_icon)
  /* Remaining fields */ ...                // Must adhere to the ItemStack format (item stack definition)
}
```
- The `replace` field is optional and declares the conflict resolution strategy for the current icon (defaults to inheriting the goal's `icon` field strategy).
- All other fields must follow Minecraft's `ItemStack` format (e.g., `{"item": "minecraft:diamond"}`).

### HomogenousItemList (Homogeneous Item List)
Represents a collection of items of the same type, supporting three formats:
- A single item ID (e.g., `"minecraft:diamond"`).
- An item tag ID (prefixed with `#`, e.g., `"#minecraft:logs_that_burn"`).
- A list of the above two formats (e.g., `["minecraft:diamond", "#minecraft:logs_that_burn"]`).

---

## Goal Configuration (Goal Structure)
The core configuration of a speedrun goal resides in the `Goal` object, which includes the following fields:

| Field Name | Type         | Description                                                                 |
|------------|--------------|-----------------------------------------------------------------------------|
| `icon`     | `ItemStack`  | The default icon of the goal (used if no custom icon is defined in triggers). |
| `display`  | `Component`  | The display text of the goal (optional, follows Minecraft's text component format, e.g., JSON text). |
| `predicates`| Array        | A list of conditions for triggering goal completion (**at least one must be configured**). Each element is one of the following types. |

---

### Predicates (Trigger Conditions)
`predicates` is an array where each element represents a condition for triggering goal completion. The following trigger condition types are supported:

#### Type 1: Advancement-Based Trigger Condition
Triggers goal completion when the player obtains a specified advancement.  
**Structural Example**:
```json
{
  "advancement": "minecraft:end/kill_dragon",    // Advancement ID (required)
  "icon": {                                      // Optional: Custom icon (StatedIcon format)
    "replace": "covers_icon",
    "item": "minecraft:emerald"
  }
}
```
- `advancement`: Required. Specifies the advancement ID that triggers goal completion (e.g., `minecraft:end/kill_dragon`).
- `icon`: Optional. Customizes the icon for this trigger condition (follows `StatedIcon` format).

---

#### Type 2: All-Items-in-List Trigger Condition
Requires the player to collect **all** items (or all items in tags) listed.  
**Structural Examples**:
```json
[
  "#minecraft:logs_that_burn"  // Collect all types of wood in the Overworld (tag)
]
```
or
```json
[
  "minecraft:diamond", 
  "#minecraft:logs_that_burn"  // Collect both diamonds and all types of wood in the Overworld
]
```
- Directly specify a `HomogenousItemList` (single item ID, tag ID, or list).

---

#### Type 3: Any/All-Items-in-List with Predicates Trigger Condition
Requires the player to collect **any one** item (or any item in tags) from the list. If `select` is set to `all`, it requires collecting **all** items (or all items in tags) from the list.  
**Structural Example**:
```json
{
  "items": ["minecraft:diamond_sword", "minecraft:netherite_axe"],  // Choose any one
  "select": "any",             // "any" means choose one; "all" means collect all
  "item_predicate": {          // Optional: Additional filters for items (e.g., enchantments, durability)
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
  "icon": {                    // Optional: Custom icon
    "replace": "icon",
    "item": "minecraft:redstone"
  }
}
```
- `items`: Required. Specifies the candidate item list (`HomogenousItemList`).
- `select`: Fixed value `"any"` (indicates choosing one).
- `item_predicate`: Optional. Adds additional filtering conditions for items in the list (e.g., enchantments, durability; follows Minecraft's "item stack predicate" format).
- `icon`: Optional. Customizes the icon for this trigger condition (`StatedIcon` format).

---

#### Type 4: Item Predicate-Based Trigger Condition
Requires the player to collect **any one item that meets specified predicates** (no dependency on predefined lists).  
**Structural Example**:
```json
{
  "item_predicate": {          // Required: Item filtering conditions
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
  "icon": {                    // Optional: Custom icon
    "replace": "covers_gen",
    "item": "minecraft:diamond_sword"
  }
}
```
- `item_predicate`: Required. Defines item filtering conditions (follows Minecraft's `ItemPredicate` format).
- `icon`: Optional. Customizes the icon for this trigger condition (`StatedIcon` format).

---

## Notes
1. **Mutual Exclusivity of Predicates**: Multiple conditions in the `predicates` array **take effect simultaneously**, meaning the player must satisfy all conditions to complete the goal (unless otherwise specified).
2. **Icon Priority**: If both a custom `icon` and the system default icon are configured for the same trigger condition, the final display is determined by the `IconState` mode (defaults to `covers_gen`).
3. **Item Tag Support**: Using `#<tag_id>` matches all items under the tag (e.g., `#minecraft:tools` matches all tools).
4. **Text Components**: The `display` field supports all Minecraft text formats (e.g., color codes `\u00A7`, translation keys `{translate:...}`, etc.).

---

## Example Configuration
Below is a complete speedrun goal configuration example, requiring the player to complete the "Free the End" advancement and collect either a diamond pickaxe or netherite pickaxe with Efficiency II-III enchantments:

```json
{
  "icon": {
    "item": "minecraft:diamond_pickaxe",
    "replace": "covers_icon"
  },
  "display": [
    {
      "text": "Speedrun Goal: ",
      "color": "gold"
    },
    {
      "text": "Diamond Master",
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

This configuration means: The player must complete the "Free the End" advancement (`end/kill_dragon`) using the dragon egg icon, and possess a diamond pickaxe or netherite pickaxe with Efficiency II-III enchantments to complete the speedrun goal. The default goal icon uses a diamond pickaxe (if conflicts occur between its data components and the system-generated icon, it **overrides** the system icon's components). The icon for the advancement trigger uses the dragon egg, while the icon for obtaining the pickaxe uses a diamond pickaxe (if conflicts occur between its data components and the system-generated icon, it **adopts** the system icon's components). The final display text is a gold-colored title: "Speedrun Goal: Diamond Master".
