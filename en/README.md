# NeoSpeed Zero

This is a mod that allows you to customize speedrun goals with [data pack](https://minecraft.wiki/Data_pack).

In the goal, you can decide what items to collect, or even what complicated goals to achieve.

If you achieve everything in your goal, you'll win.

## Commands

- `/neospeed start <goal> [<difficulty>]`
  - `<goal>`: Goal ID, e.g. `speedabc:c`
  - `[<difficulty>]` (Optional): Difficulty, e.g. `neospeedzero:empty`. See [wiki](https://ns0.7c7.icu/#/en/difficulty) for details.
  - Starts a new goal.
- `/neospeed stop`
  - Stop current goal.
- `/neospeed view`
  - View progress. Alternatively, you can press `B` (configurable).
- `/neospeed view raw`
  - View progress in text.

## Built-in goals

There are several built-in goals:

- **SpeedABC**: collect all obtainable items whose ID start with the given letter. For instance, `speedabc:a` contains `acacia_log`, `apple`, etc.
  - Goal IDs: `speedabc:a`, `speedabc:b`, `speedabc:c`, ..., `speedabc:z`
- **HanNumSpeed**: collect all obtainable items whose Simplified Chinese (`zh_cn`) name match the given length. For instance, `hannumspeed:1` contains Book (书), Crossbow (弩), Sugar (糖), etc.
  - Goal IDs: `hannumspeed:1`, `hannumspeed:2`, `hannumspeed:3`, ...
- **All Vanilla Items**: collect all obtainable vanilla items. For example, Elytra, Dragon Egg, Deepslate Emerald Ore, but not Bedrock.
  - Goal ID: `speedabc:vanilla_all`

## Data pack format

See [wiki](https://ns0.7c7.icu/#/en/goal).
