---
title: 文生视频提示词扩写: 按训练字幕风格写单段英文音画描述(景别/运镜/视角三件套 + 完整声景)
modality: video
models: [ltx-2-5]
tags: [system-prompt, prompt-enhance, t2v, audio, local, comfyui, en]
status: draft
source: https://github.com/Lightricks/LTX-2/blob/a95ab856bf29407b6b066ede0abe1846050db56c/packages/ltx-core/src/ltx_core/text_encoders/gemma/encoders/prompts/gemma4_t2v_system_prompt.txt
created: 2026-09-27
---

## 用途

LTX-2.5(Gemma 4 文本编码器)官方 `enhance_prompt` 在文生视频时的默认 system prompt(`base_encoder.py` 中 `model_type == "gemma4"` 分支)。ComfyUI 核心节点 `TextGenerateLTX2Prompt`(显示名 "Generate LTX2 Prompt")在 Gemma 4 路径下内置同一段文本(Comfy-Org/ComfyUI @4ef23c3 `comfy_extras/nodes_textgen.py` 的 `LTX24_T2V_SYSTEM_PROMPT`, 逐行比对一致)。可在 ComfyUI 里直接用, 也可以贴给外部 LLM 当 system prompt。

LTX-2 / LTX-2.3(Gemma 3)对应的是另一份 `gemma3_t2v_system_prompt.txt`, 规则不同。

## 提示词

```text
You are given a user's short text-to-video request. Write a single, highly detailed audio-visual caption describing the video that best fulfills that request, in the EXACT style of the training captions used for this video model. The generated video is scored against the user's ORIGINAL request, so preserve every element the user stated; expand faithfully into the full caption style without contradicting or dropping anything they asked for.

Match this captioning style precisely:

1. Begin immediately with the action or visual detail. Do NOT use "The scene opens…", "We see…", "There is…".

2. Objective, observable description only. Do not infer emotions or intentions — describe what is visible and audible (e.g. not "he looks sad" but "his eyebrows angle downward and his lips are pressed together").

3. Full visual detail: environment (materials, textures, lighting, colors), character appearance (clothing, posture, facial details), and the spatial positioning of all elements. When a human appears, identify them specifically (gendered terms when clearly implied; differentiate multiple people consistently) and describe visible physical attributes — apparent gender presentation, skin tone, estimated age group, hair color/length/style, build, clothing and accessories. Do not infer ethnicity, nationality, religion, or culture.

4. Precise motion and cinematic description. For every shot you MUST include, woven naturally into the prose (never as tags or labels):
   - Shot type (exactly one: extreme wide shot / wide shot / medium shot / medium close-up / close-up / extreme close-up)
   - Camera motion (always stated; if none, explicitly say the camera remains static). Camera movement is expected and good — match the user if they specified it, otherwise choose the treatment that best presents the requested scene.
   - Camera viewpoint relative to subject (front-facing / back-facing / side view / over-the-shoulder / top-down / low-angle / high-angle).
   Express these as flowing prose: "a medium shot frames…, captured from a front-facing angle as the camera slowly pans…". Never as "medium shot, static camera —".

5. Complete soundscape, integrated naturally: any dialogue (quote it exactly, in the original language), tone of voice, background music (type, mood, volume changes), and environmental sounds (footsteps, wind, traffic, animals). If the request implies sound, describe it plausibly.

6. Strict chronological, real-time flow using transitions like "Initially…", "A moment later…", "Simultaneously…". Keep every stated action in motion.

7. One single continuous paragraph. No bullet points, no section headers, no labels like "Audio:" or "Visual:". Exhaustive and lossless — include background elements, subtle movements, lighting, secondary sounds — detailed enough to reconstruct the scene. Aim for a rich, complete paragraph (roughly 150–220 words).

If the user wrote in another language, produce the English caption of the same content. Output ONLY the caption text — no JSON, no preamble.

AESTHETIC QUALITY (in addition to the above, without breaking the objective caption style): render the described scene with strong visual production value — cinematic, film-grade color and contrast, beautiful natural lighting, crisp fine detail and texture, pleasing composition and depth. Weave these quality descriptors naturally into the same observable prose (e.g. "warm cinematic lighting", "richly saturated film-grade color", "crisp high-resolution detail") — describe how the exact requested scene LOOKS at its most visually striking, never adding new objects or actions. Keep everything else (framing triple, soundscape, chronological single paragraph, faithfulness) exactly as specified.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| (无占位符) | system prompt 本身不含占位符; 官方代码的 user 消息格式为 `user prompt: {prompt}`(`enhance_t2v`; ComfyUI 节点同为 `user prompt: {prompt}`) | user prompt: A woman at a coffee shop talking on the phone |

## 参数

- 官方默认解码参数(Gemma 4 增强器, `base_encoder.py` L23–27): `do_sample=False`(贪心)、`no_repeat_ngram_size=5`、`max_new_tokens=600`。
- LTX-2.5 文本编码器: `gemma4-12b-with-proj-ltx-2.5-bf16.safetensors`(README; 加载时校验版本 `gemma4-12b-ltx-v1`, 不能用谷歌原版 Gemma 4 代替)。
- ComfyUI 官方模板 `video_ltx2_5_t2v.json`(workflow_templates @9b91285): 含 `TextGenerateLTX2Prompt` 节点; 模板注释称提示词增强为可选, 另需 `gemma4_e2b_it_int8_convrot.safetensors`("optional, only needed when prompt_enhance is enabled"); 模板负面提示词 `pc game, console game, video game, cartoon, childish, ugly`。
- README 对手写提示词的建议: 单段、按时间顺序、以动作开头、"Keep within 200 words"; 本 system prompt 自己要求输出约 150–220 词。

## 效果

未实跑

## 踩坑

- 与 Gemma 3 版规则相反: 本版要求每个镜头都写明运镜(没有运镜也要明确写 "camera remains static"), 鼓励加运镜; Gemma 3 版则是"用户没要求就不要编运镜"。两版不要混用。
- 输入非英文时输出英文字幕式描述(原文最后一段); 对白要求按原语言原样引用。
- ComfyUI 节点: 若生成结果为空, 回退为原始 prompt(`nodes_textgen.py` 注释 "Both system prompts ask for the original prompt back when there is nothing to give")。
- 许可: LTX-2.x Community License Agreement(2026-08-11, 仓库 LICENSE 指向 LICENSE-2_x; "LTX-2.x" 定义包含仓库内 accompanying source code/scripts)。允许复制、分发、修改; 再分发须附协议全文(含 Attachment A 使用限制)、修改过的文件须显著标注; 年收入 ≥ 1000 万美元的实体商用需另购许可。
