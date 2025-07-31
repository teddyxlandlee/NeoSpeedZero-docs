# NeoSpeed Zero

使用本模组，你可以通过[数据包](https://zh.minecraft.wiki/w/数据包)的形式自定义速通挑战！

支持的挑战目标包括收集物品、达成进度等。

<!-- bilibili-video https://www.bilibili.com/video/BV1jH8qzZEzs -->

## 命令

*   `/neospeed start <目标> [<难度>]`
    *   `<目标>`：目标ID，例如 `speedabc:c`
    *   `[<难度>]` (可选)：难度，例如 `neospeedzero:empty`。详情请见 [wiki](https://ns0.7c7.icu/#/zh/difficulty)。
    *   开始一个新的目标。
*   `/neospeed stop`
    *   停止当前目标。
*   `/neospeed view`
    *   查看进度。或者，你也可以按 `B` 键（可配置）。
*   `/neospeed view raw`
    *   以文本形式查看进度。

## 内置目标

模组内置了多个目标：

*   **SpeedABC**：收集所有可获取的、物品ID以给定字母开头的物品。例如，`speedabc:a` 包含 `acacia_log`（金合欢原木）、`apple`（苹果）等。
    *   目标ID： `speedabc:a`, `speedabc:b`, `speedabc:c`, ..., `speedabc:z`
*   **HanNumSpeed**：收集所有可获取的、简体中文 (`zh_cn`) 物品名称长度与给定数字匹配的物品。例如，`hannumspeed:1` 包含书、弩、糖等。
    *   目标ID： `hannumspeed:1`, `hannumspeed:2`, `hannumspeed:3`, ...
*   **所有原版物品**：收集所有可获取的原版物品，即包含鞘翅、龙蛋、深板岩绿宝石矿石等物品，但不包括基岩等物品。
    *   目标ID： `speedabc:vanilla_all`

## 数据包格式

请参阅 [wiki](https://ns0.7c7.icu/#/zh/goal)。
