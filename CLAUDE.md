This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# aigc

AI 生成(图像 / 视频 / 音频 / 文本)的个人知识库 + Claude Code 工具箱。目标是**慢慢积累**: 好用的 skill、验证过的提示词、模型笔记、工作流。当前没有应用代码。

## 目录

```
.claude/skills/   可直接调用的 skill(自写 + 精选第三方), 见下表
prompts/          提示词库, 按模态分 image/ video/ audio/ text/, 一条一个 .md, 模板 prompts/_template.md
                  来源索引 prompts/SOURCES.md(官方 / 高 star 社区库, 含只索引不转载的); 非 OSI 许可附件在 prompts/_licenses/
models/           模型笔记, 一个模型一个 <slug>.md, 模板 models/_template.md
outputs/          生成产物, 已 gitignore, 不入库
```

## Skills

| skill | 来源 | 用途 |
|---|---|---|
| `save-prompt` | 自写 | 把提示词按模板收录进 `prompts/` |
| `model-notes` | 自写 | 用 Tavily 调研模型, 写/更新 `models/<slug>.md` |
| `mmx-cli`(目录 `minimax-multimodal-toolkit`) | [MiniMax-AI/skills](https://github.com/MiniMax-AI/skills) @60aaae5, MIT, 官方 | 通过 `mmx` CLI 调 MiniMax API 生成文本/图/视频/语音/音乐; 需 `npm i -g mmx-cli` + `mmx auth login` |
| `h3-prompt-writing` | [MiniMax-AI/MiniMax-H3](https://github.com/MiniMax-AI/MiniMax-H3/tree/main/skills) @d21241f, H3 Community License, **官方** | 把需求改写成 H3 结构化提示词, 覆盖 T2VA / I2VA / FL2VA / L2VA / Ref2VA 五种模式; 纯文档, 不调 API |
| `qwen-image-prompt-writing` | 自写; `references/` 是官方 PE 改写模型的 system prompt 原文([QwenLM/Qwen-Image-2.1](https://github.com/QwenLM/Qwen-Image-2.1) `prompt_rewrite/prompts/` @fb7ae1d, Qwen Research License, 附 LICENSE/NOTICE) | 让 Claude 充当官方改写器, 写 Qwen-Image-2.1 文生图/编辑提示词, 并给出画幅与 ComfyUI 设置; 纯文档。官方没有提供 skill, 社区版本见 `models/qwen-image-2-1.md`, 均未引入 |
| `krea-2-prompt-writing` | 自写; `references/` 是官方 `docs/expansion.txt`(扩写 system prompt)和 `docs/prompting.md` 原文([krea-ai/krea-2](https://github.com/krea-ai/krea-2) @db3984f, Apache-2.0) | 按 Krea 官方方式写 Krea 2 文生图提示词(探索 / 定稿 / 润色三种), 给画幅、官方风格 LoRA 触发词和 ComfyUI Turbo 设置; 纯文档。官方 skill [krea-ai/skills](https://github.com/krea-ai/skills) 只走 Krea 托管 MCP(付费), 未引入 |
| `z-image-prompt-writing` | 自写; `references/pe.py` 是官方 PE 模板原文(Tongyi-MAI 官方 HF Space `Z-Image-Turbo` @9baf60e), 另附 Space 示例提示词 | 按官方 PE 四步法写 Z-Image-Turbo / Base 提示词, 给分辨率、负面提示词(仅 Base)和 ComfyUI 设置; 纯文档。官方没有 skill |

- 第三方 skill 是**拷贝快照**, 不会自动更新; 升级时重新从上游拉取并更新上表的 commit。改动过的第三方 skill 在表里注明。
- MiniMax-AI/skills 仓库还有 `minimax-music-gen` / `gif-sticker-maker` / `vision-analysis` 等, 需要时再按上面方式引入。
- H3 仓库另有 8 个风格 skill(极简产品广告、3D 动画短片、纸艺定格等), 依赖 MiniMax Hub 专有工具(`hub_generate_video` 等), **在 Claude Code 里跑不了**, 只能当提示词方法论参考。

## 本地环境

- **ComfyUI 本地运行**, H3 用自己的工作流(开源 H3-Base, 出 768p), 不走 MiniMax API。
- Qwen-Image-2.1 也在本地 ComfyUI 跑(int8 权重, 用 Comfy 官方模板)。**许可仅限非商用**, 商用出图前要另拿许可。
- Krea 2 本地跑开源 Turbo(int8, `models/unet/krea2/`), 用 Comfy 官方模板。开源权重许可: 公司年营收 < 100 万美元才可商用。
- Z-Image 本地有 Turbo(bf16)和 Base(GGUF Q4_K_M), 用 Comfy 官方模板, Apache-2.0。
- **工作流 JSON 不入库**: Comfy 官方模板随 ComfyUI 安装(`comfyui-workflow-templates` 包, 版本由 ComfyUI 锁定), 自用工作流留在 ComfyUI 的 `user/default/workflows/`。仓库只在 `models/<slug>.md` 记模板名、上游 commit、默认参数和本机要改的地方。
- 各模型的「本机现状」(已下载的文件、自用工作流、要改的路径)写在对应的 `models/<slug>.md`。
- **只要提示词时不碰本机目录**: 写提示词、给参数建议只依据仓库里的笔记和 skill 文档; 笔记没写清的参数标「待确认」。要去本机 ComfyUI 目录查文件(源码、模板、模型), 先问用户, 同意后再查。

## 约定

- **收提示词的门槛**: 厂商官方, 或社区 GitHub 仓库 star ≥ 1000 且 6 个月内有 push(≥ 10k 可例外, 注明是否停更)。许可不允许转载的只进 `SOURCES.md`; 转载自 X 等平台的要写原作者。
- **提示词**: 原文一字不改地存; 自己写的可复用部分用 `{{变量}}`, 原文自带的占位符(`[subject]`、`{prompt}` 等)保持原样; `status` 只有 draft / tested / favorite 三档, 没实跑过就是 draft。
- **模型事实带日期和来源**: AIGC 变化快, `models/` 里每条事实都要能追到来源; 查不到或冲突的写「待核实」, 不猜。
- **区分官方与社区**: skill、工作流、第三方托管版本(如 fal 的 H3 Max)都标明出处。
- **联网调研用 Tavily MCP**(`.mcp.json`, HTTP 远程)。key 只在本地 `.mcp.json`, 已 gitignore; 新环境从 `.mcp.json.example` 复制。
- 密钥(MiniMax / Tavily 等 API key)一律不写进受版本控制的文件。
- 文件名 kebab-case, 正文中文为主; 需要英文提示词的模型(如 H3)提示词本体保持英文。

## 以后可以做

- skill 攒多了之后, 加 `.claude-plugin/marketplace.json` 把本仓库做成 plugin 市场(结构参考 MiniMax-AI/skills), 其他项目一条命令安装。
