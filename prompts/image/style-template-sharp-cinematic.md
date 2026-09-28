---
title: 风格模板「Fooocus Sharp」(电影剧照感 + 去模糊锐化, Fooocus 默认风格)
modality: image
models: [sdxl]
tags: [style-template, prompt-placeholder, cinematic, negative-prompt, fooocus]
status: draft
source: https://github.com/lllyasviel/Fooocus/blob/ae05379cc97bc4361ec8b4ec90193dab21be763f/sdxl_styles/sdxl_styles_fooocus.json ("Fooocus Sharp", 第 11 行); 作者 Fooocus 项目(lllyasviel); 按 update_log, 这个风格综合了 CivitAI 上的多条提示词
created: 2026-09-27
---

## 用途

套在任意正向提示词外面的风格预设, 让 SDXL 出图更锐、少糊, 并带一点电影剧照感。它是 Fooocus 的**默认风格之一**: `modules/config.py` 里 `default_styles` 的默认值是 `["Fooocus V2", "Fooocus Enhance", "Fooocus Sharp"]`。其中 Fooocus V2 是 GPT-2 提示词扩写, 不是模板; Fooocus Enhance 只有负面提示词。

Fooocus 的 update_log 原话: "Added "Fooocus Sharp". This style combines many CivitAI prompts that reduces SDXL blurry and improves sharpness in a relatively natural way."

## 提示词

正向模板:

```text
cinematic still {prompt} . emotional, harmonious, vignette, 4k epic detailed, shot on kodak, 35mm photo, sharp focus, high budget, cinemascope, moody, epic, gorgeous, film grain, grainy
```

负面提示词:

```text
anime, cartoon, graphic, (blur, blurry, bokeh), text, painting, crayon, graphite, abstract, glitch, deformed, mutated, ugly, disfigured
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `{prompt}` | 用户的正向提示词, 由 Fooocus 替换进模板 | a futuristic pop up tent in a forest |

## 参数

- 套用方式(`modules/sdxl_styles.py` 的 `apply_style`): `p.replace('{prompt}', positive)`; 风格的负面提示词会和用户的负面提示词合并。
- 在 ComfyUI 里可以用 twri/sdxl_prompt_styler 节点(同样的 JSON 格式、同样的 `{prompt}` 替换规则), 或 ComfyUI-Easy-Use 的 Styles Selector(下拉菜单默认就是 `fooocus_styles`, 据 comfyui-wiki.com)。
- 针对 SDXL 调的, 不适用于 Nano Banana、GPT Image 这类自然语言模型。

## 效果

未实跑。

## 踩坑

- Fooocus 目前处于「Limited Long-Term Support (LTS) with Bug Fixes Only」, 不打算适配新模型架构; readme 在技巧列表处注明 "Those are based on SDXL and are not very up-to-date with latest models."
- update_log 写了 "Fooocus Sharp" 和 "Fooocus Enhance" "may undergo minor revision in future updates", 不同版本的文本可能不一样, 以上面的 commit 为准。
- 旧的默认风格 "Default (Slightly Cinematic)" 已改名为 "Fooocus Cinematic", 不再默认选中。想复现旧结果用 Fooocus Cinematic。

许可: Fooocus 仓库是 GPL-3.0。
