---
title: 超细制作简报式提示词: 画面规格/镜头滤镜/调色/布光/场景/服道/声音/分秒镜头表/机位说明/后期
modality: video
models: [sora-2, sora-2-pro]
tags: [template, cinematography, shot-list, sound, official]
status: draft
source: https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/examples/sora/sora2_prompting_guide.ipynb ("Going Ultra-Detailed" → Example, notebook 第 10 个 cell)
created: 2026-09-27
---

## 用途

OpenAI Sora 2 指南 "Going Ultra-Detailed" 一节的示例: 像导演给摄影/VFX 组写简报那样, 用专业制作术语写镜头、布光、调色、声音乃至"为什么这样拍"。指南说这种写法适合复刻特定摄影风格(IMAX 航拍、35mm 手持、复古 16mm 纪录片等)或在多镜头间保持严格连续性。

## 提示词

```text
Format & Look
Duration 4s; 180° shutter; digital capture emulating 65 mm photochemical contrast; fine grain; subtle halation on speculars; no gate weave.

Lenses & Filtration
32 mm / 50 mm spherical primes; Black Pro-Mist 1/4; slight CPL rotation to manage glass reflections on train windows.

Grade / Palette
Highlights: clean morning sunlight with amber lift.
Mids: balanced neutrals with slight teal cast in shadows.
Blacks: soft, neutral with mild lift for haze retention.

Lighting & Atmosphere
Natural sunlight from camera left, low angle (07:30 AM).
Bounce: 4×4 ultrabounce silver from trackside.
Negative fill from opposite wall.
Practical: sodium platform lights on dim fade.
Atmos: gentle mist; train exhaust drift through light beam.

Location & Framing
Urban commuter platform, dawn.
Foreground: yellow safety line, coffee cup on bench.
Midground: waiting passengers silhouetted in haze.
Background: arriving train braking to a stop.
Avoid signage or corporate branding.

Wardrobe / Props / Extras
Main subject: mid-30s traveler, navy coat, backpack slung on one shoulder, holding phone loosely at side.
Extras: commuters in muted tones; one cyclist pushing bike.
Props: paper coffee cup, rolling luggage, LED departure board (generic destinations).

Sound
Diegetic only: faint rail screech, train brakes hiss, distant announcement muffled (-20 LUFS), low ambient hum.
Footsteps and paper rustle; no score or added foley.

Optimized Shot List (2 shots / 4 s total)

0.00–2.40 — “Arrival Drift” (32 mm, shoulder-mounted slow dolly left)
Camera slides past platform signage edge; shallow focus reveals traveler mid-frame looking down tracks. Morning light blooms across lens; train headlights flare softly through mist. Purpose: establish setting and tone, hint anticipation.

2.40–4.00 — “Turn and Pause” (50 mm, slow arc in)
Cut to tighter over-shoulder arc as train halts; traveler turns slightly toward camera, catching sunlight rim across cheek and phone screen reflection. Eyes flick up toward something unseen. Purpose: create human focal moment with minimal motion.

Camera Notes (Why It Reads)
Keep eyeline low and close to lens axis for intimacy.
Allow micro flares from train glass as aesthetic texture.
Preserve subtle handheld imperfection for realism.
Do not break silhouette clarity with overexposed flare; retain skin highlight roll-off.

Finishing
Fine-grain overlay with mild chroma noise for realism; restrained halation on practicals; warm-cool LUT for morning split tone.
Mix: prioritize train and ambient detail over footstep transients.
Poster frame: traveler mid-turn, golden rim light, arriving train soft-focus in background haze.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| (无占位符) | 这是完整示例而非模板; 可替换的是各段标题下的内容 | 段落: Format & Look / Lenses & Filtration / Grade / Palette / Lighting & Atmosphere / Location & Framing / Wardrobe / Props / Extras / Sound / Optimized Shot List / Camera Notes (Why It Reads) / Finishing |

## 参数

- 与描述型模板相同: 时长、分辨率、角色只能用 API 参数 `seconds`、`size`、`characters` 设定(本例 "Duration 4s" 对应 `seconds: "4"`)。

## 效果

未实跑

## 踩坑

- 指南: 越长越细的提示词越限制模型创造力, 模型会尽量遵循但不一定总能做到; 短提示词给模型更多自由、结果更出乎意料。
- 指南建议描述"观众先注意到什么"、机位与镜头、光线方向、调色、质感、同期声、镜头时长。
- 许可: MIT(openai/openai-cookbook 仓库 LICENSE, Copyright (c) 2025 OpenAI), 可转载, 需保留版权与许可声明。
