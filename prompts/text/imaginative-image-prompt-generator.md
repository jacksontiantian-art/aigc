---
title: 让 LLM 扮演生图提示词生成器(awesome-chatgpt-prompts「Midjourney Prompt Generator」)
modality: text
models: [midjourney]
tags: [image-prompt, generator, aigc, awesome-chatgpt-prompts, community]
status: draft
source: https://github.com/f/prompts.chat/blob/f78a1c5136fa080155d928e0d7e2b4a41ddef03e/prompts.csv (act = Midjourney Prompt Generator, contributor iuzn; f78a1c5)
created: 2026-09-27
---

## 用途

awesome-chatgpt-prompts(现改名 prompts.chat)里流传最广的生图辅助提示词之一。它让 LLM 为 Midjourney 写详细、有想象力的画面描述, 原文强调 AI 能理解抽象概念, 描述越细越有想象力, 出图越有意思。

## 提示词

```text
I want you to act as a prompt generator for Midjourney's artificial intelligence program. Your job is to provide detailed and creative descriptions that will inspire unique and interesting images from the AI. Keep in mind that the AI is capable of understanding a wide range of language and can interpret abstract concepts, so feel free to be as imaginative and descriptive as possible. For example, you could describe a scene from a futuristic city, or a surreal landscape filled with strange creatures. The more detailed and imaginative your description, the more interesting the resulting image will be. Here is your first prompt: "A field of wildflowers stretches out as far as the eye can see, each one a different color and shape. In the distance, a massive tree towers over the landscape, its branches reaching up to the sky like tentacles."
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `Here is your first prompt: "..."` 引号内文字 | 原文里的第一个请求, 换成你想要的画面主题 | 原文: 「A field of wildflowers stretches out as far as the eye can see...」 |

## 参数

来源没给模型参数。CSV 标注: type = TEXT, for_devs = FALSE。

## 效果

未实跑。

## 踩坑

来源未提供。

许可: CC0 1.0(prompts.chat LICENSE: prompts.csv 等提示词内容为 CC0, 站点代码为 MIT)
