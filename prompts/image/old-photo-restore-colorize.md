---
title: 老照片修复并上色(一句话版)
modality: image
models: [nano-banana]
tags: [edit, restoration, colorize, old-photo, community, reprint]
status: draft
source: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/README_en.md (Case 20, 第 1182 行); 合集标注原作者 X @GeminiApp(Google Gemini 官方账号) https://x.com/GeminiApp/status/1960347483021959197
created: 2026-09-27
---

## 用途

上传一张老旧、破损或黑白的照片, 让模型修复并上色。这是合集里最短、最通用的一版。

「老照片修复 / 上色 / 高清化」在别的合集里也有更长的版本, 比如 PicoTrex Example 74(@op7418, 高清化后用现代动漫手法重绘)、YouMind 的 Nano Banana Pro 合集 No.15「Cinematic Portrait Restoration」(@QAiStudio)、EvoLink Case 456「Old Footage Restoration」(@CuriousRefuge)。

## 提示词

```text
restore and colorize this photo.
```

## 变量

无。

| 变量 | 说明 | 示例 |
|---|---|---|
| (输入图) | 合集说明: "Need to upload an old photo that needs restoration" | 一张黑白老照片 |

## 参数

需要上传 1 张照片。来源没有给出其他参数。

## 效果

未实跑。合集示例图:
- 输入: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case20/input.jpg
- 输出: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case20/output.jpg

## 踩坑

- 出处待核实: 合集标注的 @GeminiApp 帖子(2025-08-26)正文只有 "You can even bring new life to old photos. Check out how easy it is to restore your favorite photos from the past: goo.gle/3UMaEMD", 不含这句提示词。短链接没能抓取, 所以没法确认这句是不是 Google 官方给的原话。
- 合集把它归给 Google 官方账号, 但这条是从社区合集里转来的, 不属于厂商的官方提示词指南。

许可: PicoTrex 仓库的 LICENSE 文件是 Apache-2.0, README 徽章却写 CC BY 4.0, 两者不一致, 待核实。
