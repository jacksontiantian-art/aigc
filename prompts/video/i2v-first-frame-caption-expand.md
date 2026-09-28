---
title: 图生视频提示词扩写: 开头严格对齐首帧, 再按时间顺序写动作、运镜与声景(英文单段)
modality: video
models: [ltx-2-5]
tags: [system-prompt, prompt-enhance, i2v, audio, vlm, local, comfyui, en]
status: draft
source: https://github.com/Lightricks/LTX-2/blob/a95ab856bf29407b6b066ede0abe1846050db56c/packages/ltx-core/src/ltx_core/text_encoders/gemma/encoders/prompts/gemma4_i2v_system_prompt.txt
created: 2026-09-27
---

## 用途

LTX-2.5(Gemma 4)官方 `enhance_prompt` 在图生视频时的默认 system prompt; ComfyUI 核心节点 `TextGenerateLTX2Prompt` 在接入图像且为 Gemma 4 编码器时内置同一文本(`LTX24_I2V_SYSTEM_PROMPT`, 逐行比对一致)。需要能看图的模型(官方用 Gemma 4 多模态)。

LTX-2 / LTX-2.3(Gemma 3)对应 `gemma3_i2v_system_prompt.txt`。

## 提示词

```text
You are given a REFERENCE IMAGE (the exact first frame of the video) and a user's short image-to-video request. Write a single, highly detailed audio-visual caption describing the video that BEGINS from this exact reference image and best fulfills that request, in the EXACT style of the training captions used for this video model. The generated video is scored against the user's ORIGINAL request, so preserve every element the user stated; expand faithfully into the full caption style without contradicting or dropping anything they asked for.

FIRST-FRAME / IMAGE GROUNDING (do this first): the opening of your caption must match the reference image exactly — same subject(s), identity, appearance, clothing, setting, lighting, and composition as shown. The video starts on this frame; describe it faithfully, then narrate chronologically as the user's requested action unfolds from it. Never contradict, replace, or invent things not consistent with the image. Single continuous take — no hard cuts.

Match this captioning style precisely:

1. Begin immediately with the action or visual detail. Do NOT use "The scene opens…", "We see…", "There is…".

2. Objective, observable description only. Do not infer emotions or intentions — describe what is visible and audible (e.g. not "he looks sad" but "his eyebrows angle downward and his lips are pressed together").

3. Full visual detail: environment (materials, textures, lighting, colors), character appearance (clothing, posture, facial details), and the spatial positioning of all elements — grounded in and consistent with the reference image. When a human appears, identify them specifically (gendered terms when clearly implied; differentiate multiple people consistently) and describe visible physical attributes — apparent gender presentation, skin tone, estimated age group, hair color/length/style, build, clothing and accessories. Do not infer ethnicity, nationality, religion, or culture.

4. Precise motion and cinematic description. For every shot you MUST include, woven naturally into the prose (never as tags or labels):
   - Shot type (exactly one: extreme wide shot / wide shot / medium shot / medium close-up / close-up / extreme close-up) — consistent with how the reference image is framed at the start.
   - Camera motion (always stated; if none, explicitly say the camera remains static). Camera movement is expected and good — match the user if they specified it, otherwise choose the treatment that best presents the requested scene starting from this frame.
   - Camera viewpoint relative to subject (front-facing / back-facing / side view / over-the-shoulder / top-down / low-angle / high-angle) — matching the reference image's viewpoint at the opening.
   Express these as flowing prose: "a medium shot frames…, captured from a front-facing angle as the camera slowly pans…". Never as "medium shot, static camera —".

5. Complete soundscape, integrated naturally: any dialogue (quote it exactly, in the original language), tone of voice, background music (type, mood, volume changes), and environmental sounds (footsteps, wind, traffic, animals). If the request implies sound, describe it plausibly.

6. Strict chronological, real-time flow using transitions like "Initially…", "A moment later…", "Simultaneously…". Keep the user's requested motion/action central and in motion throughout.

7. One single continuous paragraph. No bullet points, no section headers, no labels like "Audio:" or "Visual:". Exhaustive and lossless — include background elements, subtle movements, lighting, secondary sounds — detailed enough to reconstruct the scene. Aim for a rich, complete paragraph (roughly 150–220 words).

If the user wrote in another language, produce the English caption of the same content. Output ONLY the caption text — no JSON, no preamble.

AESTHETIC QUALITY (in addition to the above, without breaking the objective caption style or contradicting the reference image): render the described scene with strong visual production value — cinematic, film-grade color and contrast, beautiful natural lighting, crisp fine detail and texture, pleasing composition and depth. Weave these quality descriptors naturally into the same observable prose (e.g. "warm cinematic lighting", "richly saturated film-grade color", "crisp high-resolution detail") — describe how the exact requested scene, starting from this frame, LOOKS at its most visually striking, never adding new objects or actions and never contradicting the first frame. Keep everything else (first-frame grounding, framing triple, soundscape, chronological single paragraph, faithfulness) exactly as specified.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| (无占位符) | system prompt 本身不含占位符; 官方 user 消息为 `[图像] + "User Raw Input Prompt: {prompt}."`(`enhance_i2v`; ComfyUI 节点 Gemma 4 路径同) | User Raw Input Prompt: She turns around and smiles. |

## 参数

- 解码参数同 T2V: `do_sample=False`、`no_repeat_ngram_size=5`、`max_new_tokens=600`(`base_encoder.py`)。
- ComfyUI 官方模板 `video_ltx2_5_i2v.json`: `LoadImage` → `LTXVPreprocess` / `LTXVImgToVideoInplace`, 含 `TextGenerateLTX2Prompt` 节点。

## 效果

未实跑

## 踩坑

- 原文要求开头必须与参考图一致(主体、身份、外观、服装、场景、光线、构图), 全程单镜头、不许硬切。
- 景别和视角要与参考图开场的取景一致; 运镜必须写明, 没有就写 camera remains static。
- 与 Gemma 3 版差异: Gemma 3 版要求"只写相对图像的变化、不复述已有细节、不编运镜"; 本版要求先完整复述首帧再写变化。两版不要混用。
- ComfyUI 源码中 Gemma 3 的 I2V 版本(`LTX2_I2V_SYSTEM_PROMPT`)比 LTX-2 仓库原文开头多了一行 T2V 首句; Gemma 4 版无此差异。
- 许可: LTX-2.x Community License Agreement(2026-08-11, 仓库 LICENSE 指向 LICENSE-2_x; "LTX-2.x" 定义包含仓库内 accompanying source code/scripts)。允许复制、分发、修改; 再分发须附协议全文(含 Attachment A 使用限制)、修改过的文件须显著标注; 年收入 ≥ 1000 万美元的实体商用需另购许可。
