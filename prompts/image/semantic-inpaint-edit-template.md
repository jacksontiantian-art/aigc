---
title: 语义局部编辑模板(只改指定元素, 其余不变)
modality: image
models: [nano-banana-2, nano-banana-pro]
tags: [edit, inpainting, local-edit, i2i, template, official-google]
status: draft
source: https://ai.google.dev/gemini-api/docs/image-generation (Prompting guide and strategies > Prompts for editing images > 2. Inpainting (semantic masking); 页面 Last updated 2026-09-17)
created: 2026-09-27
---

## 用途

不画蒙版, 用一句话指定要改的元素, 其余保持原样。来源原话: "Conversationally define a \"mask\" to edit a specific part of an image while leaving the rest untouched." 「change only X ... keep everything else exactly the same」这个结构在 OpenAI、BFL、Seedream 的官方指南里也都出现(写法各有差异), 是跨模型通用的编辑指令骨架。

## 提示词

官方模板(换行照原文):

```text
Using the provided image, change only the [specific element] to [new
element/description]. Keep everything else in the image exactly the same,
preserving the original style, lighting, and composition.
```

官方示例(原文带引号, 照录):

```text
"Using the provided image of a living room, change only the blue sofa to be
a vintage, brown leather chesterfield sofa. Keep the rest of the room,
including the pillows on the sofa and the lighting, unchanged."
```

## 变量

原文用方括号占位, 原样保留; `[new element/description]` 在原文里跨行。

| 变量 | 说明 | 示例(取自官方示例) |
|---|---|---|
| `[specific element]` | 要改的元素 | `the blue sofa` |
| `[new element/description]` | 改成什么 | `a vintage, brown leather chesterfield sofa` |

## 参数

需要随提示词传入一张输入图。来源未说明其他参数。

## 效果

未实跑。来源示例: 输入 https://ai.google.dev/static/gemini-api/docs/images/living_room.png , 输出 https://ai.google.dev/static/gemini-api/docs/images/living_room_edited.png

## 踩坑

- 官方示例在模板之外又点名了要保留的细节("including the pillows on the sofa and the lighting"), 比模板里泛泛的 "Keep everything else" 更具体。
- 同页 Best practices 建议小步迭代, 例: "Keep everything the same, but change the character's expression to be more serious."

许可: Google Developers 文档内容为 CC BY 4.0(代码示例 Apache 2.0), 见页脚; 转载需署名 Google、附原链接并注明改动。
