---
title: 风格迁移编辑模板(保留构图, 换艺术风格)
modality: image
models: [nano-banana-2, nano-banana-pro]
tags: [style-transfer, edit, i2i, template, official-google]
status: draft
source: https://ai.google.dev/gemini-api/docs/image-generation (Prompting guide and strategies > Prompts for editing images > 3. Style transfer; 页面 Last updated 2026-09-17)
created: 2026-09-27
---

## 用途

输入一张照片, 保留原构图, 把内容重绘成另一种艺术风格。来源原话: "Provide an image and ask the model to recreate its content in a different artistic style." 句式里「保留什么」和「换成什么」分成两句写, 支持图像编辑的模型都可以套用。

## 提示词

官方模板:

```text
Transform the provided photograph of [subject] into the artistic style of [artist/art style]. Preserve the original composition but render it with [description of stylistic elements].
```

官方示例(原文带引号, 照录):

```text
"Transform the provided photograph of a modern city street at night into the artistic style of Vincent van Gogh's 'Starry Night'. Preserve the original composition of buildings and cars, but render all elements with swirling, impasto brushstrokes and a dramatic palette of deep blues and bright yellows."
```

## 变量

原文用方括号占位, 原样保留。

| 变量 | 说明 | 示例(取自官方示例) |
|---|---|---|
| `[subject]` | 输入照片的内容 | `a modern city street at night` |
| `[artist/art style]` | 目标风格 | `Vincent van Gogh's 'Starry Night'` |
| `[description of stylistic elements]` | 具体风格元素 | `swirling, impasto brushstrokes and a dramatic palette of deep blues and bright yellows` |

## 参数

需要随提示词传入一张输入图。来源未说明其他参数。

## 效果

未实跑。来源示例: 输入 https://ai.google.dev/static/gemini-api/docs/images/city.png , 输出 https://ai.google.dev/static/gemini-api/docs/images/city_style_transfer.png

## 踩坑

来源未说明。

许可: Google Developers 文档内容为 CC BY 4.0(代码示例 Apache 2.0), 见页脚; 转载需署名 Google、附原链接并注明改动。
