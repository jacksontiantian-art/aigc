---
title: 风格模板「sai-enhance」(通用画质增强, SDXL Prompt Styler 的示例风格)
modality: image
models: [sdxl]
tags: [style-template, prompt-placeholder, enhance, negative-prompt, fooocus, comfyui]
status: draft
source: https://github.com/lllyasviel/Fooocus/blob/ae05379cc97bc4361ec8b4ec90193dab21be763f/sdxl_styles/sdxl_styles_sai.json ("sai-enhance", 第 38 行); twri/sdxl_prompt_styler@51068179 的 sdxl_styles_sai.json 内容相同; "sai" 指 Stability AI, 据 stable-diffusion-art.com 是 DreamStudio 的预设风格(二手来源, 待核实)
created: 2026-09-27
---

## 用途

最通用的「画质增强」风格, 不改变画风, 只在主体前后加质量词, 并附一组通用负面词。twri/sdxl_prompt_styler 的 README 就用它做示例, 输入 "a futuristic pop up tent in a forest" 会得到 "breathtaking a futuristic pop up tent in a forest . award-winning, professional, highly detailed"。

SAI 系列一共 17 个风格(3d-model、analog film、anime、cinematic、comic book、craft clay、digital art、enhance 等)。Fooocus 和 twri 两个仓库里的 17 条逐条一致(本地已比对)。

## 提示词

正向模板:

```text
breathtaking {prompt} . award-winning, professional, highly detailed
```

负面提示词:

```text
ugly, deformed, noisy, blurry, distorted, grainy
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `{prompt}` | 用户的正向提示词, 替换进模板 | a futuristic pop up tent in a forest |

## 参数

- twri README 的规则: 节点把模板里的 `{prompt}` 换成用户正向文本; 如果用户给了负面文本, 就和模板的 `negative_prompt` 合并, 例如负面文本 "dark" 合并后是 "ugly, deformed, noisy, blurry, distorted, grainy, dark"。
- Fooocus 里的显示名是 "SAI Enhance"(`normalize_key` 会把 `sai-enhance` 规范成这个名字)。
- 针对 SDXL, 不适用于自然语言指令式模型。

## 效果

未实跑。

## 踩坑

- 「SAI 风格来自 Stability AI 官方预设」这个说法只有二手来源(stable-diffusion-art.com: "These styles are the presets in DreamStudio, the official Stable Diffusion generator"), 没找到 Stability AI 的一手出处, 待核实。
- twri/sdxl_prompt_styler 仓库只有 920 star, 最近 push 是 2024-05, 不满足收录门槛, 这里只当旁证; 主来源是 Fooocus。

许可: Fooocus 仓库是 GPL-3.0; twri/sdxl_prompt_styler 是 MIT。SAI 风格原始文本的许可待核实。
