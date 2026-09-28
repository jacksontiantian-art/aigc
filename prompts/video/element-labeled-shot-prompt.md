---
title: 按要素拆解的一句话视频提示词: 构图/主体/场景/氛围/运镜/动作逐项落位
modality: video
models: [veo-3-1]
tags: [template, t2v, official, structure]
status: draft
source: https://ai.google.dev/gemini-api/docs/veo#prompt-guide ("Example prompts and output" → "Icicles", 页面 Last updated 2026-09-05 UTC)
created: 2026-09-27
---

## 用途

Gemini API 的 Veo 3.1 官方提示词指南用这条例子演示 "prompt writing basics" 的各要素如何放进一句话, 括号里标出了每段对应的要素。适合当作检查清单: 写完一条提示词后, 看看构图、主体、场景、氛围、运镜、动作是否都落位。

Vertex AI(现 Gemini Enterprise Agent Platform)的同主题指南把要素展开为 Subject / Action / Scene or context / Camera angles / Camera movements / Lens and optical effects / Visual style & aesthetics(Lighting, Tone or mood, Artistic style, Ambiance)/ Temporal elements / Audio / Cinematic terms / Negative prompts。

## 提示词

```text
Close up shot (composition) of melting icicles (subject) on a frozen rock wall (context) with cool blue tones (ambiance), zoomed in (camera motion) maintaining close-up detail of water drips (action).
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `(composition)` | 构图/景别, 原文 "Close up shot" | wide shot, low angle |
| `(subject)` | 主体, 原文 "melting icicles" | a playful Golden Retriever puppy |
| `(context)` | 场景, 原文 "on a frozen rock wall" | a misty ancient forest |
| `(ambiance)` | 氛围/色调, 原文 "with cool blue tones" | muted orange warm tones |
| `(camera motion)` | 运镜, 原文 "zoomed in" | slow pan left |
| `(action)` | 动作, 原文 "maintaining close-up detail of water drips" | walking along the beach |

括号标签是指南为演示加的注解, 实际使用时是否保留, 来源未说明。

## 参数

- Veo 3.1(Gemini API): 生成 8 秒视频, 720p / 1080p / 4k, 原生音频; 画幅 16:9 或 9:16(页面开头)。
- Vertex AI 指南的音频建议: 明确说明是否要音频, 并用单独的句子描述声音(音效、环境声、对白)。

## 效果

未实跑

## 踩坑

- Vertex AI 指南: 负面提示词不要用 "no" / "don't" 这类指令式措辞, 而是直接列出不想要的东西(例: 用 "wall, frame" 而不是 "no walls")。
- Vertex AI 指南: 部分高级机位与镜头效果 "not officially supported", 效果视提示词与场景而定。
- 抓取时 Gemini API 页 "Prompt writing basics" 下的要素列表丢失, 要素全表以 Vertex AI 版为准。
- 许可: CC BY 4.0(页面脚注: "Except as otherwise noted, the content of this page is licensed under the Creative Commons Attribution 4.0 License"), 可转载与改编, 须署名 Google、给出来源链接并注明改动。
