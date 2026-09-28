---
title: 写实照片场景模板(镜头类型 + 主体 + 场景 + 光线 + 机位与镜头)
modality: image
models: [nano-banana-2, nano-banana-pro]
tags: [photorealistic, template, t2i, official-google]
status: draft
source: https://ai.google.dev/gemini-api/docs/image-generation (Prompting guide and strategies > Prompts for generating images > 1. Photorealistic scenes; 页面 Last updated 2026-09-17)
created: 2026-09-27
---

## 用途

Google Gemini API 官方文档给 Nano Banana 系列的文生图模板之一, 用来写写实照片。来源原话: 场景描述越具体, 对结果的控制越多。句式是「镜头类型 + 主体 + 场景 + 光线 + 机位 + 镜头」, 没有用到模型专有语法, 可以当跨模型的写实照片骨架。

## 提示词

官方模板(换行照原文):

```text
A photorealistic [type of shot] of a [subject description] in a [setting
description]. [Description of the light]. Shot from a [camera angle]
with a [lens type].
```

官方示例:

```text
A photorealistic wide-angle shot of a vibrant coral reef teeming with tropical fish. Crystal-clear turquoise water with sunbeams filtering down from the surface, illuminating a sea turtle gliding gracefully over the coral. Shot from a low perspective with a wide-angle lens. Aspect ratio 16:9.
```

## 变量

原文用方括号占位, 原样保留。

| 变量 | 说明 | 示例(对照官方示例, 大致对应) |
|---|---|---|
| `[type of shot]` | 镜头类型 | `wide-angle shot` |
| `[subject description]` | 主体 | `vibrant coral reef teeming with tropical fish` |
| `[setting description]` | 场景/环境(原文此占位符跨行) | 官方示例没有单独对应, 环境写进了下一句 |
| `[Description of the light]` | 光线 | `Crystal-clear turquoise water with sunbeams filtering down from the surface, ...` |
| `[camera angle]` | 机位 | `a low perspective` |
| `[lens type]` | 镜头 | `a wide-angle lens` |

## 参数

- 官方示例把画幅直接写在提示词末尾: `Aspect ratio 16:9.`
- 同页「Optional configurations」另有输出格式、画幅(aspect ratio)、分辨率(image size)的 API 配置, 本模板没有给具体取值。
- 同页列出的型号: Nano Banana 2(`gemini-3.1-flash-image`)、Nano Banana Pro(`gemini-3-pro-image`)、Nano Banana 2 Lite(`gemini-3.1-flash-lite-image`)、Nano Banana(`gemini-2.5-flash-image`, 旧版); 模板本身没有限定型号。

## 效果

未实跑。来源该节未抓到示例图。

## 踩坑

来源没有给出这个模板的失败案例。同页 Best practices 中相关的几条:
- 越具体越可控, 例如不写 "fantasy armor", 而写出盔甲的材质、纹样和形制。
- 用 "semantic negative prompts": 不写 "no cars", 改成正向描述 "an empty, deserted street with no signs of traffic"。
- 用摄影和电影术语控制构图, 如 `wide-angle shot`、`macro shot`、`low-angle perspective`。

许可: Google Developers 文档内容为 CC BY 4.0(代码示例 Apache 2.0), 见页脚; 转载需署名 Google、附原链接并注明改动。
