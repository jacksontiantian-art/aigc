---
title: 棚拍产品图 / 商业摄影模板
modality: image
models: [nano-banana-2, nano-banana-pro]
tags: [product, ecommerce, studio, template, t2i, official-google]
status: draft
source: https://ai.google.dev/gemini-api/docs/image-generation (Prompting guide and strategies > Prompts for generating images > 4. Product mockups & commercial photography; 页面 Last updated 2026-09-17)
created: 2026-09-27
---

## 用途

电商、广告、品牌用的干净棚拍产品图。来源原话: "Perfect for creating clean, professional product shots for ecommerce, advertising, or branding." 模板把产品、台面、布光方案与目的、机位与卖点、对焦重点、画幅拆成独立槽位, 适合批量替换产品。

## 提示词

官方模板(换行照原文):

```text
A high-resolution, studio-lit product photograph of a [product description]
on a [background surface/description]. The lighting is a [lighting setup,
e.g., three-point softbox setup] to [lighting purpose]. The camera angle is
a [angle type] to showcase [specific feature]. Ultra-realistic, with sharp
focus on [key detail]. [Aspect ratio].
```

官方示例:

```text
A high-resolution, studio-lit product photograph of a minimalist ceramic
coffee mug in matte black, presented on a polished concrete surface. The
lighting is a three-point softbox setup designed to create soft, diffused
highlights and eliminate harsh shadows. The camera angle is a slightly
elevated 45-degree shot to showcase its clean lines. Ultra-realistic, with
sharp focus on the steam rising from the coffee. Square image.
```

## 变量

原文用方括号占位, 原样保留; 部分占位符在原文里跨行。

| 变量 | 说明 | 示例(取自官方示例) |
|---|---|---|
| `[product description]` | 产品 | `minimalist ceramic coffee mug in matte black` |
| `[background surface/description]` | 台面/背景 | `polished concrete surface` |
| `[lighting setup, e.g., three-point softbox setup]` | 布光方案 | `three-point softbox setup` |
| `[lighting purpose]` | 布光目的 | `create soft, diffused highlights and eliminate harsh shadows` |
| `[angle type]` | 机位 | `slightly elevated 45-degree shot` |
| `[specific feature]` | 要展示的卖点 | `its clean lines` |
| `[key detail]` | 对焦重点 | `the steam rising from the coffee` |
| `[Aspect ratio]` | 画幅 | `Square image` |

## 参数

来源未说明具体参数; 画幅以文字写在提示词末尾(示例为 `Square image.`)。同页「Optional configurations」有画幅与分辨率的 API 配置。

## 效果

未实跑。来源示例图: https://ai.google.dev/static/gemini-api/docs/images/product_mockup.png

## 踩坑

来源未说明。

许可: Google Developers 文档内容为 CC BY 4.0(代码示例 Apache 2.0), 见页脚; 转载需署名 Google、附原链接并注明改动。
