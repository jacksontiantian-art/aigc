---
title: 带指定文字的 Logo / 海报模板
modality: image
models: [nano-banana-pro, nano-banana-2]
tags: [text-rendering, logo, poster, typography, template, t2i, official-google]
status: draft
source: https://ai.google.dev/gemini-api/docs/image-generation (Prompting guide and strategies > Prompts for generating images > 3. Accurate text in images; 页面 Last updated 2026-09-17)
created: 2026-09-27
---

## 用途

需要图里出现准确文字的设计稿: logo、海报、标牌等。来源原话: "Be clear about the text, the font style (descriptively), and the overall design." 模板把图像类型、品牌/概念、要渲染的文字(放在双引号里)、字体风格、整体风格、配色分开写。

## 提示词

官方模板(换行照原文):

```text
Create a [image type] for [brand/concept] with the text "[text to render]"
in a [font style]. The design should be [style description], with a
[color scheme].
```

官方示例:

```text
Create a modern, minimalist logo for a coffee shop called 'The Daily Grind'. The text should be in a clean, bold, sans-serif font. The color scheme is black and white. Put the logo in a circle. Use a coffee bean in a clever way.
```

## 变量

原文用方括号占位, 原样保留。

| 变量 | 说明 | 示例(对照官方示例, 大致对应) |
|---|---|---|
| `[image type]` | 图像类型 | `modern, minimalist logo` |
| `[brand/concept]` | 品牌或主题 | `a coffee shop called 'The Daily Grind'` |
| `[text to render]` | 要出现在图里的文字, 模板里外面包着双引号 | `The Daily Grind` |
| `[font style]` | 字体风格(用描述, 不一定是字体名) | `clean, bold, sans-serif font` |
| `[style description]` | 整体风格 | `modern, minimalist` |
| `[color scheme]` | 配色 | `black and white` |

## 参数

来源未说明具体参数。来源建议专业素材用 Gemini 3 Pro Image(Nano Banana Pro)。

## 效果

未实跑。来源示例图: https://ai.google.dev/static/gemini-api/docs/images/logo_example.jpg

## 踩坑

- 模板里要渲染的文字用双引号, 但官方示例把店名写成单引号 `'The Daily Grind'`, 两者不一致; 来源没说哪种更好, 待实测。
- 来源只说 "Use Gemini 3 Pro Image for professional asset production", 没有给出文字出错时的处理办法。

许可: Google Developers 文档内容为 CC BY 4.0(代码示例 Apache 2.0), 见页脚; 转载需署名 Google、附原链接并注明改动。
