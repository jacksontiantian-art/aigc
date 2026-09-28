---
title: 人像照片生成 2 寸证件照(蓝底正装)
modality: image
models: [nano-banana]
tags: [edit, portrait, id-photo, headshot, community, reprint]
status: draft
source: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/README.md (例 63, 第 2550 行); 原作者 X @songguoxiansen(松果先森) https://x.com/songguoxiansen/status/1963602241610551609
created: 2026-09-27
---

## 用途

上传一张人物照片, 截取头部, 生成 2 寸证件照: 蓝底、职业正装、正脸、微笑。原帖(2025-09-04)抓取时有 58 万浏览, 称「一句话就可以实现的证件照生成」。

「证件照 / 职业头像」这一类在其他合集里也有, 比如 ZeroLu 的 Nano Banana Pro 合集 1.5「One-Click Business Photo」和 1.7「Professional Headshot Creator」, 都比这条长得多。

## 提示词

```text
截取图片人像头部，帮我做成2寸证件照，要求:
  1、蓝底
  2、职业正装
  3、正脸
  4、微笑
```

## 变量

原文没有占位符。原帖说有「白底和蓝底」两个版本, 这里收的是合集里的蓝底版。

| 变量 | 说明 | 示例 |
|---|---|---|
| (输入图) | 合集说明: 需上传一张人物参考图像 | 一张生活照 |

## 参数

需要上传 1 张人物照片。来源没有给出其他参数。

## 效果

未实跑。合集示例图:
- 输入: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case63/input.jpg
- 输出: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case63/output.jpg

## 踩坑

- 原文语言待核实: 原帖正文只写了「以下是提示词（白底和蓝底）」, 提示词本体(可能在配图或后续回复里)没有抓到。作者是用中文发帖的, 所以按 PicoTrex 中文版收录; 英文 README 里的 "Crop the head and create a 2-inch ID photo..." 应该是译文。
- 来源里没有写其他踩坑。

许可: PicoTrex 仓库的 LICENSE 文件是 Apache-2.0, README 徽章却写 CC BY 4.0, 两者不一致, 待核实。提示词本身的作者是 X 用户 @songguoxiansen, 合集的许可不一定能覆盖原帖内容。
