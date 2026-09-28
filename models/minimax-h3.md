---
name: MiniMax H3 (海螺 3.0 / Hailuo 3.0)
slug: minimax-h3
vendor: MiniMax
modality: omni          # 文/图/视频/音频混合输入 → 带原生立体声的视频
access: both            # 官方 API 为完整版; 开源权重只有 H3-Base
updated: 2026-09-26
---

## 一句话

MiniMax 第三代海螺视频模型: 多模态上下文统一理解, 一次生成带原生立体声的视频; 强项是参考控制和视频编辑。

## 能力与限制(官方 README)

- 输出: 4–15 秒, 24 FPS, 默认短边 768px; 2K 需经 H3-Regenerate-2K(未开源, 走 API)
- 系统三段: **H3-Context-IR**(把多模态上下文压缩/增强成提示词, 未开源) → **H3-Base**(开源, 出 768p) → **H3-Regenerate-2K**(未开源, 带原始上下文重生成 2K, 不是传统超分)
- 开源权重两个变体, 对应五种模式:

| 变体 | 模式 | 输入 |
|---|---|---|
| H3-Base-FL2VA | T2VA 文生 / I2VA 首帧 / L2VA 尾帧 / FL2VA 首尾帧 | 0 张图=T2VA, 1 张=首帧或尾帧, 2 张=首尾帧 |
| H3-Base-Ref2VA | Ref2VA 全参考 | 图 ≤9; 视频 ≤3 段, 每段 2–15 秒, 总 ≤15 秒; 音频 ≤3 段, 不能单独作为输入, 每段 2–15 秒, 总 ≤15 秒; 文件总数 ≤12 |

- 官方 API 与开源 Base 都会做内容审核
- 时间线: 2026-07-17 WAIC 首发 → 07-31 上线 API → 08-03 放出权重(第三方报道)
- 榜单(Artificial Analysis, 第三方单次测评): 视频编辑 #1、文生视频 #2、图生视频 #3

## 怎么调用

| 方式 | 说明 |
|---|---|
| 本地 ComfyUI(**本人在用**) | 自有工作流; 官方模板参考 `video_minimax_h3_t2v.json` / `video_minimax_h3_r2v.json`, 见 Comfy 教程。只有 Base, 输出 768p |
| 官方 API | `/video-generation-v2-create`, 国内走 `api.minimaxi.com` / platform.minimaxi.com; 完整三段式, 可出 2K |
| 混合 | 官方 README「完整 2K 工作流」: 本地 SGLang 跑 Base + 官方 API 跑 Context-IR 和 Regenerate-2K |
| `mmx` CLI | skill `mmx-cli`; 其文档默认视频模型仍是 `MiniMax-Hailuo-2.3`, 用 H3 前先 `--help` 确认 model id |
| 第三方 | fal 的 **H3 Max**: 基于开源权重再训练, 768p、5–15 秒、出片快 |

## 提示词要点(官方 skill `h3-prompt-writing`)

写提示词直接调用 `.claude/skills/h3-prompt-writing`, 规则全文见其 `references/base-en.txt`(T2VA/I2VA/FL2VA/L2VA) 和 `references/ref-en.txt`(Ref2VA)。要点:

- **全英文**; 只有对白、歌词、画面里可见的文字保留原语言
- **基础模式** = 对齐指令(T2VA 没有) + 三字段 `integrated_multimodal_description` → `overall_soundscape` → `non_diegetic_music`
  - I2VA 首行固定: `For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.`
  - FL2VA / L2VA 首行是 `How the reference pictures align with the target video — ...`, 秒数精确到两位小数
  - FL2VA 优先单镜头, 让模型从首帧连续插到尾帧
- **Ref2VA** = 六段 `subject_definitions` → `summary` → `retention_analysis` → `detailed_description` → `overall_soundscape` → `non_diegetic_music`
  - 标签 `<Subject N>` / `<Picture N>` / `<Video N>` / `<Audio N>` 全文一致
  - `summary` 以任务类型开头: `[reference generation]` / `[keyframe completion]` / `[video editing]`, 可用 ` + ` 组合
- **分镜**: `[Shot 1]` 不写时间; 后续 `[Shot 2] At 00:03.500, the camera cuts to...`
- **运镜** = 类型 + 幅度 + 速度, 写成自然句: `The camera pushes in with small amplitude at slow speed toward...`
- **对白**: `(S1)` 稳定 ID; `<d>[English] 原话.</d>`; 旁白用 `says in an off-screen voiceover` 并注明嘴唇闭合; 跨镜头用 `<scenetrans>`, 被截断用 `<cutoff>`
- `overall_soundscape` 写环境音/动作音, 1–4 句; `non_diegetic_music` 写配乐乐器/节奏/动态, 不写情绪词, 没有就 `N/A`
- 少用 "cinematic" "beautiful" 这类空词, 写具体可见可听的细节; 描述总时长必须等于视频时长

## 价格与许可

- 许可: [MiniMax H3 Community License Agreement](https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/LICENSE)(仓库与自带 skills 同此许可)
- ⚠ **待核实**: 第三方报道称该许可禁止在美国、欧盟、英国、韩国使用(含输出物), 商用条款与发布前博客说法不同。商用前读原文。
- API 价格: 待补

## 待核实

- 帧数是否必须满足 `17k+5`(社区 skill minimax-h3-reel-maker 的说法, 官方文档未见)
- ComfyUI 路径下是否支持负面提示词(社区说 BasicGuider 没有入口, 官方未提)

## 来源

- [MiniMax-AI/MiniMax-H3](https://github.com/MiniMax-AI/MiniMax-H3) — 官方仓库 README.zh-CN 与 `skills/`, d21241f (2026-08-15), 取于 2026-09-26
- [MiniMax H3 官方博客](https://www.minimax.io/blog/minimax-h3) — 取于 2026-09-26
- [Comfy 教程: MiniMax H3](https://docs.comfy.org/tutorials/video/minimax/minimax-h3) — 官方 README 引用, 未打开
- [What Is MiniMax H3 (Hailuo 3.0)?](https://huggingface.co/blog/ResterChed/minimax-h3-hailuo-3-0) — 第三方, 取于 2026-09-26
- [Runpod: MiniMax H3](https://www.runpod.io/blog/minimax-h3-the-open-weight-omni-modal-video-model-and-what-it-takes-to-run-it) — 第三方, 取于 2026-09-26
- [MiniMax H3 Max (fal)](https://morphic.com/resources/models/minimax-h3-max) — 第三方, 取于 2026-09-26
