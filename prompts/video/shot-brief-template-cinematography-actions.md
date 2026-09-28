---
title: 视频提示词模板: 散文场景描述 + Cinematography(镜头/情绪)+ Actions 节拍 + Dialogue
modality: video
models: [sora-2, sora-2-pro]
tags: [template, t2v, dialogue, official]
status: draft
source: https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/examples/sora/sora2_prompting_guide.ipynb ("Prompt Templates and Examples" → "Descriptive Prompt Template", 指南标注 Updated - March 2026)
created: 2026-09-27
---

## 用途

OpenAI 官方 Sora 2 提示词指南给出的描述型模板: 把"场景散文""镜头与情绪""动作节拍""对白"分块写, 便于逐项迭代。指南强调这不是万能公式, 不重要的项可以省略, 留白越多模型越自由。

## 提示词

```text
[Prose scene description in plain language. Describe characters, costumes, scenery, weather and other details. Be as descriptive to generate a video that matches your vision.]

Cinematography:
Camera shot: [framing and angle, e.g. wide establishing shot, eye level]
Mood: [overall tone, e.g. cinematic and tense, playful and suspenseful, luxurious anticipation]

Actions:
- [Action 1: a clear, specific beat or gesture]
- [Action 2: another distinct beat within the clip]
- [Action 3: another action or dialogue line]

Dialogue:
[If the shot has dialogue, add short natural lines here or as part of the actions list. Keep them brief so they match the clip length.]
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `[Prose scene description in plain language. ...]` | 人物、服装、场景、天气等细节的散文描述 | Inside a cluttered workshop, shelves overflow with gears, bolts, and yellowing blueprints. ...(指南 Example 1) |
| `[framing and angle, e.g. wide establishing shot, eye level]` | 景别与角度 | medium close-up, slow push-in with gentle parallax from hanging tools |
| `[overall tone, e.g. cinematic and tense, ...]` | 整体情绪 | gentle, whimsical, a touch of suspense |
| `[Action 1: a clear, specific beat or gesture]` 等 | 一条一个动作节拍, 可混入台词 | The robot taps the bulb; sparks crackle. |
| `[If the shot has dialogue, ...]` | 简短自然的台词, 长度要能塞进片长 | Robot says quietly: "Almost lost it… but I got it!" |

## 参数

- 只能靠 API 参数、不能在正文里要求的项: `model`(`sora-2` / `sora-2-pro`)、`size`(sora-2: 720x1280、1280x720; sora-2-pro 另有 1024x1792、1792x1024、1080x1920、1920x1080)、`seconds`(4/8/12/16/20, 默认 4)、`characters`(最多引用 2 个已上传角色)。
- 图像输入 `input_reference`: 须与目标 `size` 一致, 格式 jpeg/png/webp; 图像锚定首帧, 文字描述之后发生什么。
- 视频扩展: 以整段原片为上下文, 单次最长 20 秒, 最多扩展 6 次, 合计 120 秒。

## 效果

未实跑

## 踩坑

- 片子越短越守指令; 指南建议必要时拼接两段 4 秒, 而不是直接生成 8 秒。
- 每个镜头只给一个明确运镜和一个明确主体动作; 动作写成节拍或计数(例: "takes four steps to the window, pauses, and pulls the curtain in the final second")。
- 用可见的名词动词代替空泛形容("wet asphalt, zebra crosswalk, neon signs reflecting in puddles" 优于 "a beautiful street at night")。
- 调色写光源组合并给 3–5 个色彩锚点, 多镜头剪辑时保持光线逻辑一致。
- 对白放在散文描述下方的单独块里; 4 秒约容纳一两句短对白, 长台词难以对口型。
- 同一提示词多次生成结果不同, 指南称这是特性; 编辑时一次只改一处并说明改了什么。
- 许可: MIT(openai/openai-cookbook 仓库 LICENSE, Copyright (c) 2025 OpenAI), 可转载, 需保留版权与许可声明。
