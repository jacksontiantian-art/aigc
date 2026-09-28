---
title: 等距微缩地标模型(45° 俯视 + 国家名标题)
modality: image
models: [gpt-image-2]
tags: [text-to-image, isometric, diorama, miniature, landmark, typography, community, reprint]
status: draft
source: https://github.com/EvoLinkAI/awesome-gpt-image-2-API-and-Prompts/blob/e2a269ad1a055a0b4f6c1e170341c6c1aba30faa/README.md (Case 332, 第 7916 行); 原作者 X @iamaiistudio(PromptLab) https://x.com/iamaiistudio/status/2065737739589615987
created: 2026-09-27
---

## 用途

文生图, 不用参考图。生成某个国家标志性建筑的等距微缩 3D 模型: 45° 俯视, 底座上有街道、景观和小人, 背景纯色, 顶部居中写国家名和建筑名。

「等距 / 微缩 / diorama」是各大合集里出现最多的类型之一。同一合集里还有 Case 266「Landmark Miniature Diorama」(同一作者); PicoTrex 有 Case 33「Map to Isometric Buildings」(@demishassabis); ZeroLu 的 Nano Banana Pro 合集有 2.10「USA 3D Diorama with Landmarks」和 2.12「City's Tallest Buildings 3D Cartoon View」。

## 提示词

```text
Generate an isometric miniature 3D diorama of [COUNTRY NAME]'s iconic [FAMOUS STRUCTURE] landmark from a 45-degree top-down perspective.

Use clean soft textures and realistic PBR materials with balanced, natural lighting. The elevated base features surrounding streets, landscape elements, and cultural details unique to the structure. Include tiny stylized figures of locals and tourists with detailed facial features.

Set the background to solid [BACKGROUND COLOR]. Display [COUNTRY NAME] in bold text at the top center with [STRUCTURE NAME] on the next line, followed by a minimal architecture icon below. Adjust text color to ensure contrast.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `[COUNTRY NAME]` | 国家名, 出现两次(主体描述 + 顶部标题) | (来源未给示例) |
| `[FAMOUS STRUCTURE]` | 要做成模型的标志性建筑 | (来源未给示例) |
| `[BACKGROUND COLOR]` | 纯色背景的颜色 | (来源未给示例) |
| `[STRUCTURE NAME]` | 标题第二行显示的建筑名 | (来源未给示例) |

## 参数

原帖说是在 ChatGPT 里用 GPT Image 2 生成的("GPT Image 2 on ChatGPT")。来源没有给出尺寸、质量等参数。

## 效果

未实跑。合集示例图(4 张):
- https://github.com/EvoLinkAI/awesome-gpt-image-2-API-and-Prompts/blob/e2a269ad1a055a0b4f6c1e170341c6c1aba30faa/images/poster_case402/output.jpg
- https://github.com/EvoLinkAI/awesome-gpt-image-2-API-and-Prompts/blob/e2a269ad1a055a0b4f6c1e170341c6c1aba30faa/images/poster_case402/output2.jpg
- https://github.com/EvoLinkAI/awesome-gpt-image-2-API-and-Prompts/blob/e2a269ad1a055a0b4f6c1e170341c6c1aba30faa/images/poster_case402/output3.jpg
- https://github.com/EvoLinkAI/awesome-gpt-image-2-API-and-Prompts/blob/e2a269ad1a055a0b4f6c1e170341c6c1aba30faa/images/poster_case402/output4.jpg

## 踩坑

来源里没有写踩坑。已对照原帖, 文字一致; 原帖是一整段, EvoLink 把它分成了三段。

许可: EvoLink 仓库是 CC0-1.0。提示词本身的作者是 X 用户 @iamaiistudio, 原帖没有声明许可, CC0 不一定能覆盖原帖内容。
