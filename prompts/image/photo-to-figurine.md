---
title: 照片 / 插画变角色手办(带包装盒与建模屏幕)
modality: image
models: [nano-banana]
tags: [edit, figurine, 3d, toy, community, reprint]
status: draft
source: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/README_en.md (Case 1, 第 840 行); 原作者 X @ZHO_ZHO_ZHO https://x.com/ZHO_ZHO_ZHO/status/1958539464994959715
created: 2026-09-27
---

## 用途

上传一张人物或角色图, 生成一张「手办实物照」: 手办站在圆形塑料底座上, 后面放着印有角色图的包装盒, 旁边的电脑屏幕显示 Blender 建模过程。

原作者在原帖里称这是 nano-banana「目前最出圈/火的用法」(原帖 2025-08-21, 抓取时 23.8 万浏览)。PicoTrex 合集把它排在 Nano Banana 部分的第 1 个案例。「手办 / 公仔 / 玩具化」这一类在 EvoLink、ZeroLu 等合集里也反复出现(盲盒头像、LEGO 人仔、高达模型盒等)。

## 提示词

```text
turn this photo into a character figure. Behind it, place a box with the character's image printed on it, and a computer showing the Blender modeling process on its screen. In front of the box, add a round plastic base with the character figure standing on it. set the scene indoors if possible
```

## 变量

原文没有占位符。输入图就是要做成手办的对象。

| 变量 | 说明 | 示例 |
|---|---|---|
| (输入图) | 合集说明: "Need to upload a reference image as the object for generating the figure" | 一张角色插画或人物照片 |

## 参数

来源没有给出分辨率、比例等参数。需要上传 1 张参考图。

## 效果

未实跑。合集示例图:
- 输入: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case1/input0.jpg
- 输出: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case1/output0.jpg

## 踩坑

- 原作者 2025-08-24 回复: 如果生成出透明亚克力片, 问题出在旧版提示词里的 "Make the PVC material look clear" 这一句, 去掉就好。PicoTrex 收录的是去掉这一句后的新版。新版结尾 "set the scene indoors if possible" 在 X 抓取结果里被截断, 没能逐字核对。
- PicoTrex 中文 README(例 1)是英文原文的译文, 不是原文。而且译文把 "a computer showing the Blender modeling process on its screen" 译成了「盒子上有一台电脑」, 和英文意思不一样。以英文为准。

许可: PicoTrex 仓库的 LICENSE 文件是 Apache-2.0, README 徽章却写 CC BY 4.0, 两者不一致, 待核实。提示词本身的作者是 X 用户 @ZHO_ZHO_ZHO, 合集的许可不一定能覆盖原帖内容。
