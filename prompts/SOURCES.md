# 提示词来源索引

2026-09-27 调研后筛出来的来源。门槛有两条: 要么是厂商官方; 要么是社区 GitHub 仓库, star ≥ 1000 且 6 个月内有 push(≥ 10k star 的例外, 但要注明是否停更)。star 和 push 日期来自当天的 GitHub API。

「建议」一栏有三种:

- **收录**: 已经挑出条目放进 `prompts/`
- **仅索引**: 有价值, 但许可不允许转载, 或者内容是二次加工的, 需要时回原站查
- **不推荐**: 列出来是为了避免以后重复调研

## 图像

### 官方

| 来源 | 归属 / 许可 | 内容 | 建议 |
|---|---|---|---|
| [Gemini API: Nano Banana 图像生成](https://ai.google.dev/gemini-api/docs/image-generation) | Google, 文字 CC BY 4.0 | 7 个生成模板 + 7 个编辑模板, 带占位符; 在售型号有 Nano Banana 2 Lite / 2 / Pro(页面 2026-09-17) | 收录 5 条 |
| [openai-cookbook: GPT Image prompting guide](https://github.com/openai/openai-cookbook/blob/main/examples/multimodal/image-gen-models-prompting-guide.ipynb) | OpenAI, MIT; 已归档, 由 API 文档接替 | 20 多条示例 | 收录 2 条 |
| [OpenAI API 文档: Image prompting](https://developers.openai.com/api/docs/guides/image-prompting) | OpenAI, 无开放许可 | 当前型号 `gpt-image-2.5-flare` / `-sunburst`, 8 条原则 | 仅索引 |
| [black-forest-labs/skills](https://github.com/black-forest-labs/skills) `flux-image-best-practices` | BFL 官方, MIT, 121★(官方的, 所以不看 star) | JSON、hex 颜色、文字排版、编辑规则 | 收录 1 条 |
| [black-forest-labs/flux2](https://github.com/black-forest-labs/flux2) `src/flux2/system_messages.py` | BFL 官方, Apache-2.0, 2.7k★ | FLUX.2 官方提示词扩写 system prompt | 收录 1 条 |
| [docs.bfl.ai prompting guide](https://docs.bfl.ai/guides/prompting_summary) | BFL, 无开放许可 | 18 页: 结构公式、JSON、否定句改写表 | 仅索引 |
| [Tencent-Hunyuan/HunyuanImage-3.0](https://github.com/Tencent-Hunyuan/HunyuanImage-3.0) `PE/system_prompt.py` | 腾讯官方, 混元社区许可, 3.3k★ | 中文改写 system prompt(通用版 / 文字渲染版) | 收录 1 条(通用版) |
| 火山方舟 Seedream 4.0–5.0 提示词指南 | 字节官方, 无开放许可 | 5.0 pro 编辑可用坐标标签 `<point>` / `<bbox>` | 仅索引 |
| [Midjourney 文档](https://docs.midjourney.com) | 官方, 受服务条款约束 | 当前默认 V8.2; 短提示词、`--sref` / `--sw`、文字只认双引号 | 仅索引 |
| Google Cloud: Gemini image best practices / Nano Banana prompting guide 博客 | Google | 与 ai.google.dev 高度重合 | 仅索引 |
| Imagen prompt guide | Google | Imagen 已于 2026-08-17 停服 | 不推荐 |

### 社区

| 来源 | ★ / 最近 push | 许可 | 性质 | 建议 |
|---|---|---|---|---|
| [PicoTrex/Awesome-Nano-Banana-images](https://github.com/PicoTrex/Awesome-Nano-Banana-images) | 23.8k / 2026-09-03 | LICENSE 写 Apache-2.0, README 徽章写 CC BY 4.0, 两者冲突 | 转载, 标注了 X 原作者; 约 141 例 | 收录 6 条 |
| [EvoLinkAI/awesome-gpt-image-2-API-and-Prompts](https://github.com/EvoLinkAI/awesome-gpt-image-2-API-and-Prompts) | 17.3k / 2026-07-18 | CC0 | 转载, 标注了作者; 462 例, 顺带给自家 API 引流 | 收录 1 条 |
| [ZeroLu/awesome-nanobanana-pro](https://github.com/ZeroLu/awesome-nanobanana-pro) | 10.3k / 2026-09-27 | MIT | 转载 | 候选, 本轮未收 |
| [ZeroLu/awesome-gpt-image](https://github.com/ZeroLu/awesome-gpt-image) | 2.2k / 2026-09-27 | MIT | 转载, 覆盖 GPT Image 2 / 2.5 | 候选 |
| [lllyasviel/Fooocus](https://github.com/lllyasviel/Fooocus) `sdxl_styles/` | 53.2k / **已停更**(只修 bug) | GPL-3.0 | 277 个 `{prompt}` 风格模板 | 收录 2 条(SDXL 系) |
| [freestylefly/awesome-gpt-image-2](https://github.com/freestylefly/awesome-gpt-image-2) | 33.6k | MIT | 用 AI 改写过, 不是原文 | 仅索引 |
| [YouMind-OpenLab](https://github.com/YouMind-OpenLab) awesome-nano-banana-pro-prompts / awesome-gpt-image-2 | 13.5k / 10.0k | CC BY 4.0 | 转载后改写成 `{argument}` 语法 | 仅索引 |
| [wuyoscar/GPT-Image2-Skill](https://github.com/wuyoscar/GPT-Image2-Skill) | 5.6k | MIT | 画廊 + skill | 仅索引 |
| [willwulfken/MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference) | 12.3k / 已停更 | **无许可** | MJ 风格关键词对比 | 仅索引 |
| Civitai / PromptHero | 平台, 月访问千万级 / 百万级 | — | 用户上传, 图片带生成参数 | 仅索引(查 SD / Flux 系参数用) |

未达门槛: JimmyLv/awesome-nano-banana、jamez-bondos/awesome-gpt4o-images、twri/sdxl_prompt_styler(920★)等。FLUX、Seedream、Midjourney 近期的合集都不到 1000★。

## 视频 / 音频

| 来源 | 归属 / 许可 | 内容 | 建议 |
|---|---|---|---|
| [Wan-Video/Wan2.2](https://github.com/Wan-Video/Wan2.2) `wan/utils/system_prompt.py` | 阿里官方, Apache-2.0, 17.6k★ | 文生 / 图生 / 空提示图生, 中英各一套扩写 system prompt; 附官方负面提示词 | 收录 2 条(中文版) |
| [Lightricks/LTX-2](https://github.com/Lightricks/LTX-2) `…/gemma/encoders/prompts/` | 官方, LTX-2.x 社区许可, 9.5k★ | Gemma 4 版(对应 LTX-2.5)和 Gemma 3 版(对应 LTX-2 / 2.3), 各有文生、图生 | 收录 2 条(Gemma 4 版) |
| [Tencent-Hunyuan/HunyuanVideo-1.5](https://github.com/Tencent-Hunyuan/HunyuanVideo-1.5) `hyvideo/utils/rewrite/` | 腾讯官方, 混元社区许可, 4.6k★ | 文生改写(输出英文)、图生改写(输出中文)、提示词手册 | 收录 2 条 |
| [openai-cookbook: Sora 2 prompting guide](https://github.com/openai/openai-cookbook/blob/main/examples/sora/sora2_prompting_guide.ipynb) | OpenAI, MIT | 描述式模板、超细制作简报 | 收录 2 条 |
| [Gemini API: Veo 3.1](https://ai.google.dev/gemini-api/docs/veo) / [Omni Flash](https://ai.google.dev/gemini-api/docs/omni) / [Vertex 视频提示词指南](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/video/video-gen-prompt-guide) | Google, CC BY 4.0 | 各元素词表、时间码、素材角色标签 | 收录 2 条 |
| Google Cloud 博客《Veo 3.1 终极提示词指南》、LTX-2.5 官方博客 | 官方, 保留权利 | 五段公式、多镜头规则 | 仅索引 |
| Runway Gen-4.5 帮助中心、可灵 3.0 用户指南 | 官方, 条款禁止复制 | 结构公式、多镜头、原生音频 | 仅索引 |
| ElevenLabs v3 / 音效 / 音乐文档 | 官方, 保留权利 | audio tags、音乐五要素 | 仅索引 |
| Suno 帮助 | 官方 | 只有 `[Verse]` 这类段落标记 | 不推荐, 内容太薄 |
| ZeroLu/awesome-seedance 等 Seedance 合集 | 社区, 1.4k–2.6k★ | Seedance 提示词 | 仅索引, 与本地模型无关 |

Veo / Sora / Kling / Wan 的社区视频提示词合集没有一个达到门槛。

本地 ComfyUI 的对应关系:

- **LTX**: 核心节点 `TextGenerateLTX2Prompt` 内置了同一套 system prompt, 需要另外下载 `gemma4_e2b_it_int8`。
- **Wan / Hunyuan**: 官方模板都有, 但没有扩写节点。先用 LLM 按收录的 system prompt 扩写, 再粘进模板。

## 文本(LLM)

| 来源 | 归属 / 许可 | 内容 | 建议 |
|---|---|---|---|
| [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks) `misc/metaprompt.ipynb` | Anthropic, MIT, 53k★ | Metaprompt: 由任务描述生成带变量的提示词模板 | 收录 |
| [Claude prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) | Anthropic | 官方总参考页 | 仅索引 |
| [openai-cookbook](https://github.com/openai/openai-cookbook) ChatGPT prompt guide | OpenAI, MIT | 「先提问再写」meta-prompt | 收录 |
| [OpenAI 最新模型指南](https://developers.openai.com/api/docs/guides/latest-model)、[Prompt generation](https://developers.openai.com/api/docs/guides/prompt-generation) | OpenAI | 当前旗舰 gpt-6-astra; Playground「Generate」用的 meta-prompt | 仅索引 |
| [Gemini prompting strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies) | Google, CC BY 4.0 | Gemini 3 的 XML 分节模板 | 收录 |
| DeepSeek 提示库 / 百炼 / 智谱 / Kimi 官方提示词文档 | 官方, 无开放许可 | 通用写法、样例 | 仅索引 |
| [f/prompts.chat](https://github.com/f/prompts.chat)(原 awesome-chatgpt-prompts) | 171k★, 提示词 CC0 | 2169 条 | 收录 2 条 |
| [danielmiessler/Fabric](https://github.com/danielmiessler/Fabric) | 44k★, MIT | 260 个 pattern | 收录 2 条; `improve_prompt` 里嵌了 OpenAI 原文, 不收 |
| [linshenkx/prompt-optimizer](https://github.com/linshenkx/prompt-optimizer) | 35.8k★, AGPL-3.0 | 提示词优化器, 内置文生图优化模板 | 收录 2 条 |
| [langgptai/LangGPT](https://github.com/langgptai/LangGPT) | 12.6k★, LICENSE 与徽章不一致 | 中文结构化提示词范式 | 收录 1 条 |
| [dair-ai/Prompt-Engineering-Guide](https://github.com/dair-ai/Prompt-Engineering-Guide) | 78.7k★, MIT, 更新放缓 | 指南、论文 | 仅索引 |
| [PlexPt/awesome-chatgpt-prompts-zh](https://github.com/PlexPt/awesome-chatgpt-prompts-zh) | 62.7k★ | 基本是 prompts.chat 的中文化 | 仅索引 |
| [zenstory-ai/drama-skills](https://github.com/zenstory-ai/drama-skills) | 2.3k★, MIT | 短剧 / 漫剧的剧本、分镜、图像和视频提示词 skill | 仅索引, **值得细看** |
| [yawiii/ComfyUI-Prompt-Assistant](https://github.com/yawiii/ComfyUI-Prompt-Assistant) | 2.4k★, GPL-3.0 | ComfyUI 节点: 调 LLM 翻译、扩写、反推提示词 | 仅索引, 与本地 ComfyUI 相关 |
| x1xhlol/system-prompts-and-models-of-ai-tools、asgeirtj/system_prompts_leaks 等 | 50k–144k★ | **泄露**的系统提示词 | 仅索引, 不拷贝; 想看官方版, Anthropic 在 release-notes/system-prompts 页公开了自己的 |
