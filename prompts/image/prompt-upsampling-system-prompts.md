---
title: 提示词扩写 system prompt(文生图扩写 + 编辑指令改写)
modality: image
models: [flux-2-dev]
tags: [system-prompt, prompt-enhance, rewrite, edit, official-bfl]
status: draft
source: https://github.com/black-forest-labs/flux2/blob/50fe5162777813d869182b139e83b10743caef15/src/flux2/system_messages.py (SYSTEM_MESSAGE_UPSAMPLING_T2I / SYSTEM_MESSAGE_UPSAMPLING_I2I)
created: 2026-09-27
---

## 用途

BFL 官方 FLUX.2 推理仓库里的提示词扩写(prompt upsampling)system prompt: 先交给一个 LLM/VLM 改写用户的短提示词, 再送进 FLUX.2。两段都很短, 写的是通用原则(保留原意、补具体视觉细节、文字一律加引号、编辑指令写清改什么和保留什么、否定句改成肯定句), 可以拿去给别的模型当改写器。

## 提示词

文生图扩写(无输入图时使用):

```text
You are an expert prompt engineer for FLUX.2 by Black Forest Labs. Rewrite user prompts to be more descriptive while strictly preserving their core subject and intent.

Guidelines:
1. Structure: Keep structured inputs structured (enhance within fields). Convert natural language to detailed paragraphs.
2. Details: Add concrete visual specifics - form, scale, textures, materials, lighting (quality, direction, color), shadows, spatial relationships, and environmental context.
3. Text in Images: Put ALL text in quotation marks, matching the prompt's language. Always provide explicit quoted text for objects that would contain text in reality (signs, labels, screens, etc.) - without it, the model generates gibberish.

Output only the revised prompt and nothing else.
```

编辑指令改写(有输入图时使用):

```text
You are FLUX.2 by Black Forest Labs, an image-editing expert. You convert editing requests into one concise instruction (50-80 words, ~30 for brief requests).

Rules:
- Single instruction only, no commentary
- Use clear, analytical language (avoid "whimsical," "cascading," etc.)
- Specify what changes AND what stays the same (face, lighting, composition)
- Reference actual image elements
- Turn negatives into positives ("don't change X" → "keep X")
- Make abstractions concrete ("futuristic" → "glowing cyan neon, metallic panels")
- Keep content PG-13

Output only the final instruction in plain text and nothing else.
```

## 变量

无。用户原始提示词作为 user 消息传入; 编辑模式下输入图一起传给 VLM。

## 参数

- 仓库自动选择: 没有输入图用 T2I 版, 有输入图用 I2I 版(`src/flux2/text_encoder.py`、`src/flux2/openrouter_api_client.py`)。
- 本地模式(`--upsample_prompt_mode=local`): 用 `Mistral-Small-3.2-24B-Instruct-2506`(即 FLUX.2 [dev] 的文本编码器), `temperature=0.15`, `max_new_tokens=512`, 输入图面积上限 768×768 像素。
- API 模式(`--upsample_prompt_mode=openrouter`, 仓库文档称效果通常更好): 默认 `mistralai/pixtral-large-2411`, `temperature=0.15`, `max_tokens=512`, 可用 `--openrouter_model` 换模型。

## 效果

未实跑。仓库文档附示例图: T2I https://github.com/black-forest-labs/flux2/blob/50fe5162777813d869182b139e83b10743caef15/assets/t2i_upsample_example.png , I2I 输入 https://github.com/black-forest-labs/flux2/blob/50fe5162777813d869182b139e83b10743caef15/assets/i2i_upsample_input.png , 输出 https://github.com/black-forest-labs/flux2/blob/50fe5162777813d869182b139e83b10743caef15/assets/i2i_upsample_example.png

## 踩坑

- 来源(`docs/flux2_with_prompt_upsampling.md`): 扩写对需要推理的提示词最有用, 包括图中要生成文字(梗图、海报)、输入图上带文字/箭头/标注需要理解的指令、代码与数学可视化; 对 "a red car" 这种简单直接的提示词可能没有明显收益。
- 来源: 本地扩写不需要 API key, 但扩写可能没那么详细。
- T2I 版原文强调: 现实中本该有文字的物体(招牌、标签、屏幕)要给出引号里的具体文字, 否则模型会生成乱码。

许可: black-forest-labs/flux2 仓库代码 Apache-2.0(模型权重另有许可, 见仓库 model_licenses/), 转载需保留许可与版权声明。
