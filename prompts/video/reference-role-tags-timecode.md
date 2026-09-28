---
title: 多参考视频提示词: 用 <IMAGE_REF_N> 等标签绑定素材角色 + [0-3s] 时间码分段
modality: video
models: [gemini-omni-flash]
tags: [template, reference, timecode, multi-shot, official]
status: draft
source: https://ai.google.dev/gemini-api/docs/omni ("Gemini Omni Flash prompt guide" → "Using tags in prompts to set image and video roles", 页面 Last updated 2026-09-08 UTC)
created: 2026-09-27
---

## 用途

Google Gemini Omni Flash(`gemini-omni-1.1-flash`)官方提示词指南里的 6 张参考图示例: 用标签把上传的图/视频绑定为首帧、尾帧或参考, 再用时间码切分段落。Gemini API 文档称 Omni Flash 是默认视频模型, Veo 3.1 用于扩展、尾帧控制等特定能力。

## 提示词

```text
[0-3s] A studio fashion sequence. Starting with woman <IMAGE_REF_0>, she is holding <IMAGE_REF_1>
[3-6s] Then we see the man <IMAGE_REF_2> holding <IMAGE_REF_3>
[6-10s] And finally another woman <IMAGE_REF_4> who is holding <IMAGE_REF_5> while walking.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `<IMAGE_REF_0>` … `<IMAGE_REF_5>` | 第 N 张参考图(`<IMAGE_REF_N>`), 写在它所指的人/物之后 | a woman <IMAGE_REF_1> is walking |
| `[0-3s]` `[3-6s]` `[6-10s]` | 时间码分段, 文档称"no precise syntax needed", 自然语言也可 | [0-3s] A person is walking |
| `<FIRST_FRAME>` / `<LAST_FRAME>` | (本例未用)把图绑定为首帧 / 尾帧 | <FIRST_FRAME> <LAST_FRAME> a woman is walking |
| `<VIDEO_REF_N>` | (本例未用)参考视频 | the person in <VIDEO_REF_0> is playing the violin |
| `[# Sources ...]` / `[# References ...]` | (本例未用)复杂情况在提示词开头声明来源与参考, 如 `[# Sources <FIRST_FRAME>@Image1] [# References <IMAGE_REF_0>@Image2]`; 还有 `<VIDEO_0>@Video1`、`<PREVIOUS_VIDEO>@Video1` | 见上游原文件 |

## 参数

- 模型 `gemini-omni-1.1-flash`, 通过 Interactions API 调用。
- 扩展: 每次延长 10 秒, 总长上限 40 秒, 以原视频最后 10 秒为上下文。
- 声明来源/参考后, 文档建议在提示词末尾补一句用途说明, 如 "Use this image as the starting frame." 或 "Use the given image(s) as references for video generation. The images should not be used as literal initial frames."。

## 效果

未实跑

## 踩坑

- 默认会生成含多个镜头的叙事; 需要单镜头时必须明说(文档示例以 "Continuous, unbroken handheld shot of ..." 开头)。
- 编辑时提示词越简单越好, 过度描述会带来意外改动; 局部修改加 "Keep everything else the same"。
- 扩展时输入视频的末尾若干帧会被改动以便衔接。
- 许可: CC BY 4.0(页面脚注: "Except as otherwise noted, the content of this page is licensed under the Creative Commons Attribution 4.0 License"), 可转载与改编, 须署名 Google、给出来源链接并注明改动。
