---
title: 短片剧本, 附镜头运动、机位与灯光调度
modality: text
models: []
tags: [screenplay, cinematography, storyboard, video, aigc, awesome-chatgpt-prompts, community]
status: draft
source: https://github.com/f/prompts.chat/blob/f78a1c5136fa080155d928e0d7e2b4a41ddef03e/prompts.csv (act = Screenplay Script with Cinematography Details, contributor yangmee; f78a1c5)
created: 2026-09-27
---

## 用途

让 LLM 同时扮演编剧和摄影指导, 按你给的梗概写一部 5 分钟短片剧本, 并写清镜头运动(pan、tilt)、机位角度和灯光设置。可作为视频生成前「剧本 → 分镜 → 逐镜提示词」的第一步。

## 提示词

```text
Act as a screenwriter and cinematographer. You will create a screenplay for a 5-minute short film based on the following summary:

↓-↓-↓-↓-↓-↓-↓-Edit Your Summary Here-↓-↓-↓-↓-↓-↓-↓-



↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑-↑

Your script should include detailed cinematography instructions that enhance the mood and storytelling, such as camera pans, angles, and lighting setups.

Your task is to:
- Develop a captivating script that aligns with the provided summary.
- Include specific cinematography elements like camera movements (e.g., pans, tilts), lighting, and angles that match the mood.
- Ensure the script is engaging and visually compelling.

Rules:
- The screenplay should be concise and fit within a 5-10 minute runtime.
- Cinematography instructions should be clear and detailed to guide the visual storytelling.
- Maintain a consistent tone that complements the film’s theme and mood.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| 「Edit Your Summary Here」两条分隔线之间 | 填故事梗概(原文是一行分隔线标记, 不是 `{{}}` 变量) | |

## 参数

来源没给模型参数。CSV 标注: type = TEXT, for_devs = FALSE。

## 效果

未实跑。

## 踩坑

- 原文前后时长不一致: 任务里写「5-minute short film」, Rules 里写「fit within a 5-10 minute runtime」。用的时候自己定一个。
- 同库更早的经典版本「Screenwriter」(只写剧情和人物, 不含镜头调度)见上游原文件。

许可: CC0 1.0(prompts.chat LICENSE: prompts.csv 等提示词内容为 CC0, 站点代码为 MIT)
