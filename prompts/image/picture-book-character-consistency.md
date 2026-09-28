---
title: 绘本角色一致性两步法(先立角色锚点, 再续写场景)
modality: image
models: [gpt-image-2, gpt-image-2-5-flare, gpt-image-2-5-sunburst]
tags: [character-consistency, illustration, multi-turn, edit, official-openai]
status: draft
source: https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/examples/multimodal/image-gen-models-prompting-guide.ipynb (6.4 Children’s Book Art with Character Consistency); 同文见 https://developers.openai.com/api/docs/guides/image-prompting (Keep a character consistent)
created: 2026-09-27
---

## 用途

多页插图里保持同一个角色不走样。来源的做法是两步: 第一步生成一张「角色锚点」图, 锁定外貌、比例、服装和气质; 第二步把第一步的输出当输入图, 换场景和动作, 并在提示词里重复角色的关键特征。Character / Theme / Style / Constraints 分段的写法本身不依赖模型。

## 提示词

第 1 步, 建立角色(文生图):

```text
Create a children’s book illustration introducing a main character.

Character:
A young, storybook-style hero inspired by a little forest outlaw,
wearing a simple green hooded tunic, soft brown boots, and a small belt pouch.
The character has a kind expression, gentle eyes, and a brave but warm demeanor.
Carries a small wooden bow used only for helping, never harming.

Theme:
The character protects and rescues small forest animals like squirrels, birds, and rabbits.

Style:
Children’s book illustration, hand-painted watercolor look,
soft outlines, warm earthy colors, whimsical and friendly.
Proportions suitable for picture books (slightly oversized head, expressive face).

Constraints:
- Original character (no copyrighted characters)
- No text
- No watermarks
- Plain forest background to clearly showcase the character
```

第 2 步, 续写场景(把第 1 步的图作为输入图):

```text
Continue the children’s book story using the same character.

Scene:
The same young forest hero is gently helping a frightened squirrel
out of a fallen tree after a winter storm.
The character kneels beside the squirrel, offering reassurance.

Character Consistency:
- Same green hooded tunic
- Same facial features, proportions, and color palette
- Same gentle, heroic personality

Style:
Children’s book watercolor illustration,
soft lighting, snowy forest environment,
warm and comforting mood.

Constraints:
- Do not redesign the character
- No text
- No watermarks
```

## 变量

原文是完整示例, 没有占位符。复用时通常要换的是 Character 段的外貌描述和 Scene 段的情节, 第 2 步 Character Consistency 段要和第 1 步的外貌逐项对应(整理者建议, 非来源原文)。

## 参数

- 第 1 步: `client.images.generate`, `model="gpt-image-2"`, `size="1024x1536"`, `quality="medium"`。
- 第 2 步: `client.images.edit`, `image=[第 1 步的输出图]`, 其余同上。
- API 文档 GPT Image 2.5 指南用同样两段提示词和同样的 size/quality。

## 效果

未实跑。cookbook 示例图: 第 1 步 https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/images/childrens_book_illustration_1_gpt-image-2.png , 第 2 步 https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/images/childrens_book_illustration_2_gpt-image-2.png

## 踩坑

- 来源(API 文档): 续写时要重复外貌约束, 角色才能保持一致。
- 来源(API 文档 Prompting fundamentals): "same style as before" 之类的指代可以延续上下文, 但一旦出现漂移, 要把关键约束重新写一遍。
- 两段都用 Constraints 明确要求原创角色、无文字、无水印。

许可: openai-cookbook 仓库 MIT(Copyright (c) 2025 OpenAI), 转载需保留版权与许可声明。同一提示词也出现在 OpenAI API 文档 GPT Image 2.5 prompting guide, 该页面未见开放许可, 以 cookbook 版本为准。
