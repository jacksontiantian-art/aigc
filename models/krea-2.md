---
name: Krea 2 (K2)
slug: krea-2
vendor: Krea AI(Krea.ai, Inc.)
modality: image
access: both            # 托管 Medium / Large / Turbo 走 API; 开源 Raw / Turbo 权重可本地跑
updated: 2026-09-27
---

## 一句话

Krea 自己**从零训练**的第一个图像基础模型, 主打「审美多样性 + 风格可控」(风格参考、moodboard)。2026-05-12 以托管形式上线, 2026-06-23 开源 Raw + Turbo 两个 12B checkpoint; 开源许可规定**公司年营收 100 万美元以下才可商用**。

## 能力与限制

- **版本线**:
  - Krea 1: 闭源, 官方称「与 Black Forest Labs 合作训练」(Krea 博客, 2025-07-31)
  - FLUX.1 Krea [dev]: 2025-07-31 发布的 Krea 1 开放版, 基于 BFL 提供的 flux-dev-raw(12B), 与 FLUX.1-dev 兼容(同一篇博客)
  - **Krea 2**: 2026-05-12 发布, 官方称「第一个完全从零构建的基础图像模型」, 不再基于 FLUX(Krea 博客《What Is Krea 2》)
  - Krea 2 开源版: 2026-06-23(第三方 VentureBeat / Kompozy 报道日期); 许可 v.1 落款 2026-06-22, 官方仓库建于 2026-06-22(GitHub API)
- **托管三档**(Krea API 博客、API 文档):
  - Medium: 体量较小、较快、较便宜, 后训练较重, 出图稳定; 擅长插画、动漫、绘画等表现性风格
  - Large: 体量是 Medium 的 2 倍以上, 后训练较轻, 出图更「生」、更有质感; 擅长写实, 以及运动模糊、颗粒、低动态范围这类 raw 质感
  - Turbo(API id `medium-turbo`): 最快, 约 4 秒一张, 网页端 2 credits 一张, 一次最多出 4 张, 只出 1K(Krea 用户指南《Krea 2 Turbo》)。官方说要追求风格贴合或成片质量时仍应换 Medium / Large(Krea 博客《Krea 2 Turbo》)
- **开源两档**(官方 README、HF 模型卡):
  - Raw: 预训练底座, 没有蒸馏也没有后训练, 多样性高、便于微调。官方说「不建议直接用来推理」, 定位是训练 LoRA 或做后训练; 训练分辨率最高 1K
  - Turbo: 8 步蒸馏, 可出 1K–2K。Comfy 官方博客称它是从完整后训练的 RL checkpoint 蒸馏来的, 质量高于 Raw, 但输出之间的差异变小
  - 两者搭配用: **在 Raw 上训 LoRA, 在 Turbo 上跑**(README 原话 "TRAIN on Raw and RUN on Turbo")
- **架构**:
  - 12B 稠密 DiT(MMDiT), VAE 用 Qwen Image VAE, 文本编码器是 Qwen3-VL(多层特征聚合)(Krea 开源页、Comfy 官方博客)
  - 代码里写死 `Qwen/Qwen3-VL-4B-Instruct`, MMDiT 为 28 层, 采用 GQA(48 头 / 12 个 KV 头), 采样器是 flow matching Euler(官方仓库 `inference.py` / `sampling.py`)
  - 技术报告称: 早期模型用 Qwen Image VAE 扩规模, 更大的模型改用 FLUX 2 VAE。托管 Medium / Large 各用哪个 VAE, 报告没有明说
- **开源版只能文生图**(HF 模型卡)。编辑、ControlNet、局部重绘、风格参考都靠社区扩展(见社区表)
- **分辨率与画幅**:
  - API 目前只出 1K, 支持 8 种比例: 1:1 / 4:3 / 3:2 / 16:9 / 2.35:1 / 4:5 / 2:3 / 9:16(API 文档)
  - Turbo 各比例的输出尺寸(用户指南):

    | 1:1 | 4:3 | 3:2 | 16:9 | 2.35:1 | 4:5 | 2:3 | 9:16 |
    |---|---|---|---|---|---|---|---|
    | 1024×1024 | 1184×896 | 1248×832 | 1376×768 | 1568×672 | 928×1152 | 832×1248 | 768×1376 |

  - 本地运行时, 宽高不是 16 的倍数会被自动补齐(README)
- **托管版独有的控制项**(API 文档):
  - 风格参考 `image_style_references`: 每张图单独设 `strength`, 取值 -2 到 2
  - moodboard: 在网页端建好并「分析」, 得到 taste profile / keywords / avoids 三项; API 每次请求最多带 1 个
  - `creativity`: 提示词扩写强度, 可选 raw / low / medium(默认)/ high
  - 生成滑杆 intensity / complexity / movement: 取值 -100 到 100, 背后是训练进模型的 LoRA
  - 训练好的风格 `styles`
- **文字渲染一般**: Krea 官方 skill 的评价是「中等, 只能写几个词」(官方 skill @ea45cca)
- **官方自评的短板**(官方 skill 原话大意):
  - 所有 Krea 2 模型都带偏软的 AI 感, 对比度高、饱和、纹理偏软
  - 对细节、锐度、长文字、多物体精确布局要求高时不要用
  - Turbo 评为 C 档
- **排行榜**: 技术报告称进了 Artificial Analysis 文生图榜前 10, 在独立实验室中排第 2; README 却写「独立实验室第 1」(冲突见待核实)

## 怎么调用

| 方式 | 说明 |
|---|---|
| 本地 ComfyUI(**本人在用**) | 需 ComfyUI ≥ 0.26.0(Comfy 官方博客)。权重用 [Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2) 的重打包版, 官方推荐 **Turbo fp8**(`krea2_turbo_fp8_scaled`), 也有 int8 convrot / bf16 / mxfp8 / nvfp4 可选; 文本编码器 `qwen3vl_4b_fp8_scaled`, VAE `qwen_image_vae`, CLIPLoader 的 type 选 `krea2`。官方模板见下方列表(Comfy 官方文档、workflow_templates @25ff90a) |
| 官方推理代码 | [krea-ai/krea-2](https://github.com/krea-ai/krea-2) 的 `inference.py`, 用 uv 运行。Turbo: `--steps 8 --cfg 0.0 --mu 1.15 --width 2048 --height 2048`; Raw: `--steps 52 --cfg 3.5`(README) |
| Diffusers | `Krea2Pipeline`, 需装源码版 diffusers; 权重 `krea/Krea-2-Turbo` / `krea/Krea-2-Raw`, 需在 HF 上申请授权(HF 模型卡) |
| SGLang | `sglang generate --model-path krea/Krea-2-Turbo --num-inference-steps 8 ...`(HF 模型卡) |
| InvokeAI | 第三方支持: Turbo 8 步、CFG 1; Raw 约 28 步、CFG 约 5.5(InvokeAI 文档, 与官方参数不一致, 见待核实) |
| LoRA 训练 | 官方推荐在 Raw 上训, 工具可选 diffusers、Ostris ai-toolkit、fal、Kohya musubi-tuner(README)。社区低显存方案见社区表 |
| Krea API | `POST https://api.krea.ai/generate/image/krea/krea-2/{medium,large,medium-turbo}`, 带 Bearer token; 异步返回 `job_id`, 用 `GET /jobs/{id}` 轮询, 或加 `X-Webhook-URL` 请求头接收回调。有 Node SDK `@krea-ai/sdk`(API 文档) |
| Krea 托管 MCP | `https://api.krea.ai/mcp`(Streamable HTTP, OAuth 登录)。Claude Code 添加方式: `claude mcp add --transport http krea-ai https://api.krea.ai/mcp`。提供 `list_models` / `get_model_schema` / `generate` / `get_job` 等工具。用 OAuth 登录时扣所选工作区的算力点, 用 API token 时扣 API 余额(MCP 文档) |
| ComfyUI Partner Nodes(托管) | `Krea2ImageNode` + `Krea2StyleReferenceNode`, 模板 `api_krea2_t2i` / `api_krea2_style_reference`。可选 medium turbo / medium / large, 最多 10 张风格参考, 支持 `moodboard_id`(Comfy 内置节点文档)。按次付费, 价格待核实 |
| fal | 开源 Turbo 托管版 `fal-ai/krea-2/turbo`($0.008 / 百万像素, fal 模型页); LoRA 推理 `fal-ai/krea-2/turbo/lora`, 训练 `fal-ai/krea-2-trainer`(README 链接) |
| Replicate | Krea 官方账号上架了 `krea/krea-2-medium`、`krea/krea-2-large`(Replicate official 合集), Replicate 博客还提到 `krea-2-medium-turbo`; 价格待核实 |

**ComfyUI 官方模板**:
- `image_krea2_turbo_t2i`「Krea-2: Text to Image」: fp8, 2026-06-24
- `image_krea2_turbo_t2i_int8`: 2026-07-08
- `image_krea2_turbo_int8_image_style_reference`: 1–2 张参考图, 用社区的 ostris 风格参考 LoRA, 2026-07-20
- 三个都是 Comfy 官方做的(不是 Krea 官方), MIT, 随 ComfyUI 安装。本机装的 `comfyui-workflow-templates-json` 0.1.95 与上游 @25ff90a 的关键节点参数一致(2026-09-27 对比)
- `prompt_enhance` 默认值: 两个文生图模板开, 风格参考模板关。风格参考模板的子图留了 `image1`–`image3` 三个输入, 教程说接 1–2 张
- 风格 LoRA 开关 `enable_lora` 默认关, 默认选 `krea2_darkbrush`, 触发词由 `CustomCombo` 自动填
- 托管 API 模板 `api_krea2_t2i` / `api_krea2_style_reference` 走 Krea 付费 API, 本地用不上

### 本机现状(2026-09-27 查看 `E:\ComfyUI-aki-v27\ComfyUI-aki-v2\ComfyUI`)

- ComfyUI 0.37.4, 内置 blueprint 已有「Text to Image (Krea-2 Turbo)」和「Image Style Reference (Krea-2 Turbo)」
- **已下载**:
  - `models/unet/krea2/`: `krea2_turbo_int8_convrot`、`krea2_raw_int8_convrot`、`krea2Anime_v15_3196103`(社区微调, 来源待核实), 各 13.49 GB
  - `text_encoders/qwen3vl_4b_fp8_scaled`(5.24 GB)
  - `vae/qwen_image_vae`(0.25 GB)
  - `loras/krea2/` 下两个社区 LoRA: `Krea2_四视图Character_Design_4-View_V1`、`krea2-Cc-MJ-风格滤镜`(来源待核实)
- **没下载**: 官方 9 个风格 LoRA, 以及风格参考模板要用的 `krea2_style_reference`
- **自用工作流**(`user/default/workflows/Krea2/`), 两个都接 SeedVR2 int8 放大:
  - 「单采」: Turbo 20 步
  - 「双采」: 先 8 步, 再以 denoise 0.5 补 5 步
  - 两个工作流都引用了 `Krea2-功能\` 目录下的 LoRA(`Krea2-亚洲hina_krea2Turbo_lora_tqd_v4.0`、`krea2_turbo_lora_rank_64_bf16`), 但**本机 `loras/` 下没有这个目录**(见踩坑)

### 官方 skill: krea-ai/skills(暂不引入)

- [krea-ai/skills](https://github.com/krea-ai/skills) 在 Krea 官方 GitHub 组织下, MIT 许可, v0.7.6 @ea45cca(2026-09-03), 22 star
- 含 3 个 skill: `krea-generate`(通用生成路由)、`krea-marketing`、`krea-motion`; 自带 `.claude-plugin/marketplace.json`, 安装方式 `npx skills add krea-ai/skills`
- **全部依赖 Krea 托管 MCP**(`https://api.krea.ai/mcp`), 按工作区算力点或 API 余额计费; skill 里明确写了「不要用非 MCP 的替代方式」。**本地 ComfyUI 用不上, 暂不引入**
- 值得参考的是 `krea-generate/references/models/krea-2.md`: Krea 对自家模型的坦白评价、moodboard 强度档位、「滑杆不要主动设」等规则。未入库, 需要时按 @ea45cca 从上游取

### 社区资源(均为社区, star 与更新日期取于 2026-09-27)

| 仓库 | star | 最近更新 | 用途 | 建议 |
|---|---|---|---|---|
| [lbouaraba/comfyui-krea2edit](https://github.com/lbouaraba/comfyui-krea2edit) | 662 | 2026-07-29 | 指令式、保留身份的编辑节点和工作流 | 需要编辑时再看 |
| [ostris/ComfyUI-Krea2-Ostris-Edit](https://github.com/ostris/ComfyUI-Krea2-Ostris-Edit) | 340 | 2026-07-17 | 配合 ai-toolkit 训练的实验性编辑 LoRA 的节点 | 同上 |
| [capitan01R/ComfyUI-Krea2T-Enhancer](https://github.com/capitan01R/ComfyUI-Krea2T-Enhancer) | 240 | 2026-09-26 | Turbo 增强节点 | 可试 |
| [facok/comfyui-krea2-controlnet](https://github.com/facok/comfyui-krea2-controlnet) | 232 | 2026-07-04 | ControlNet | 需要控图时再看 |
| [nkxx188/ComfyUI-Krea2-StyleTransfer](https://github.com/nkxx188/ComfyUI-Krea2-StyleTransfer) | 231 | 2026-07-21 | 免训练的风格参考节点 | 可与官方模板的 ostris LoRA 对比 |
| [AcademiaSD/AcademiaSD_LoRAlab-Krea2](https://github.com/AcademiaSD/AcademiaSD_LoRAlab-Krea2) | 138 | 2026-08-03 | 低显存 LoRA 训练 | 训 LoRA 时再看 |
| [Gothdir/ComfyUI-Krea2-PromptStyler](https://github.com/Gothdir/ComfyUI-Krea2-PromptStyler) | 31 | 2026-08-03 | 提示词构建节点, 300+ 风格 | star 少, 不引入 |
| [sjh9714/krea2-wildcards](https://github.com/sjh9714/krea2-wildcards) | 26 | 2026-09-06 | 514 条 Turbo 提示词 + 示例图 + wildcards | star 少, 只作参考 |
| [ThetaCursed/Krea2-Style-Explorer](https://github.com/ThetaCursed/Krea2-Style-Explorer) | 25 | 2026-08-04 | 1500+ 条风格参考库 | star 少, 只作参考 |
| [AI-KSK/krea2-prompt-suite](https://github.com/AI-KSK/krea2-prompt-suite) | 11 | 2026-07-20 | Codex 提示词 skill | star 少, 不引入 |
| HF [vantagewithai/Krea-2-Turbo-GGUF](https://huggingface.co/vantagewithai/Krea-2-Turbo-GGUF) / [molbal/krea2-gguf](https://huggingface.co/molbal/krea2-gguf) | 157 / 20 like | — | 社区 GGUF 量化 | 本机已有 int8, 不需要 |

提示词或 skill 类的社区仓库 star 都在 31 以下, 没有值得引入的。

## 提示词要点

写提示词时调用 skill **`krea-2-prompt-writing`**, 规则原文放在它的 `references/` 里。要点如下:

- **用自然语言写, 长而详细的效果最好**, 但不做提示词工程也能出好图(官方 `docs/prompting.md`)。训练数据以长 caption 为主, 同时保证见过短、中长度的提示词(技术报告; Comfy 官方博客)
- **要画出来的文字加引号**(`prompting.md`; `expansion.txt` 第 4 条)。文字能力只够几个词(官方 skill)
- **先写模糊的, 再逐步收窄**: 先用 "a cat riding a bicycle" 这种粗提示词出几轮, 看模型给出哪些风格方向(摄影、插画、3D……), 再加风格词。细节写得越多, 输出范围越窄(Krea 博客《Prompting Guide》《Start Vague》)
- **风格语言写在前面, 媒介写明确**: 例如 "risograph print, limited palette, bold silhouette"(用户指南《Krea 2 Turbo》)。官方 20 条示例的写法大体是「媒介 / 风格 + 主体及其属性 + 场景 + 光线 + 构图 / 镜头 + 质感」, 这是本笔记从示例中归纳的, 不是官方规定的模板(`prompting.md`)
- **文本编码器自带的描述维度**: 代码里固定的系统提示是 "Describe the image by detailing the color, shape, size, texture, quantity, text, spatial relationships of the objects and background"(`encoder.py`)。按颜色、形状、大小、质感、数量、文字、空间关系去写, 和编码器的预期一致
- **本地扩写用官方 `expansion.txt`**: ComfyUI 官方文生图模板的 `prompt_enhance` 默认开(风格参考模板默认关), 系统提示就是这份文件, LLM 最多生成 512 token。它的规则包括: 忠实于原意、不添加新物体、写成一段不用列表、不过度具体化、已经写得很详细就只润色、保留用户指定的媒介。要让提示词原样生效就关掉扩写。托管 API 里对应的开关是 `creativity`, `raw` 表示完全不扩写(API 文档)
- **没有负面提示词这一项**: Turbo 的 CFG 是 0(官方代码)或 1(ComfyUI), 负面提示不起作用, 模板里负面条件用 `ConditioningZeroOut` 置空; 托管 API 也没有 negative 参数。只有 Raw 开着 CFG 时, 代码层面支持负面提示(`sampling.py`)。Comfy 官方的 API 模板示例直接把 "no extra objects, no hands visible" 写进了正向提示词
- **分工**: 提示词负责画面里有什么(主体、构图、镜头、光线、约束), 审美(配色、质感、美术方向)交给风格参考、moodboard 或 LoRA(官方 skill)
- **风格参考强度**:
  - 网页端默认 50%; 20% 只留一点色调; 80% 以上风格占主导, 可能反过来改掉主体(Krea 博客)
  - API 建议从 0.6 左右起步, 多张叠加时强度之和取 1 左右(API 文档)
  - 官方 skill 更保守: 只有要精确复刻某种风格时才用, 每次只用 1 张、强度 0.5, 并让提示词本身也写成参考图的风格
- **本地用官方风格 LoRA 时加触发词, 强度 1.0**: 例如 `krea2_darkbrush` 对应 "monochrome ink wash style", `krea2_retroanime` 对应 "purple retro anime style"(完整 9 条见 Comfy 官方文档)。模板里的 CustomCombo 节点会自动追加触发词
- **选型**: 插画、动漫选 Medium; 胶片感写实、颗粒、运动模糊选 Large; Turbo 用来快速试方向, 定稿再换大模型(API 博客、Turbo 用户指南)。**生成滑杆除非用户明确要求, 否则不要设**, 写实类尤其不要动(官方 skill)

## 参数

| 参数 | 官方(README / 代码) | ComfyUI 官方模板 | 说明 |
|---|---|---|---|
| 步数 | Turbo 8; Raw 52(README 推荐), `inference.py` 默认 28 | Turbo 8 | Raw 的推荐值来源之间有冲突, 见待核实 |
| CFG | Turbo 0.0(关闭); Raw 3.5(README), 代码默认 4.5 | cfg 1 | ComfyUI 里 cfg 1 就是不开 CFG |
| 采样器 | flow matching Euler | euler / simple | |
| 时间步偏移 mu | Turbo 建议固定 1.15; 默认随分辨率从 0.5 变到 1.15 | 风格参考模板用 `ModelSamplingFlux`(1.15 / 0.5) | |
| 分辨率 | Turbo 1K–2K; Raw 最高 1K | `ResolutionSelector`, megapixels 设 2.0 即 2K | |
| 提示词长度 | 文本编码 `max_length=512`, 且 `truncation=True`(`encoder.py`) | 扩写 LLM 最多 512 token | 超长提示词在官方代码中会被截断 |
| LoRA | 官方风格 LoRA 推荐强度 1.0 | `LoraLoaderModelOnly` 默认 0.8 | |

## 价格与许可

- **本地运行**: 不花钱
- **Krea API**(每张价格, 官方 API 文档):

  | 档位 | 纯文生图 | 带风格参考 | 带 moodboard |
  |---|---|---|---|
  | Medium | $0.030 | $0.035 | $0.040 |
  | Large | $0.060 | $0.065 | $0.070 |
  | Turbo | $0.015 | $0.0175 | $0.020 |

  moodboard 和风格参考同时用时按 moodboard 价计, 不叠加。**API 自带商用权**, 用 API 不需要接受开源许可(API 文档; Krea 开源定价页)
- **fal**: Turbo $0.008 / 百万像素; **Replicate** 与 **Comfy Partner Nodes** 的价格待核实
- **开源权重**: [Krea 2 Community License](https://www.krea.ai/krea-2-licensing) v.1(2026-06-22):
  - **授权范围**(第 2.1 条): 有限、非独占、可撤销、免版税; 可以使用、复制、分发、做衍生品、生成输出
  - **商用的营收门槛**(第 2.3 条): 被许可方及其关联实体过去 12 个月的全公司总营收(所有来源合计)**低于 100 万美元**, 才能按本协议商用; 达到或超过门槛, 商用前必须另取企业许可; 使用途中营收超过门槛, 须立即停止商用并联系 Krea(opensource@krea.ai)
  - **企业许可**(Krea 开源定价页):
    - Maker 档: 每个模型 $1000/月, 每月最多 1 万张, 限 1 个域名、10 个用户, 含微调和 LoRA 权利; 不能用于客户或下游
    - Enterprise 档: 定制
  - **必须做内容过滤**(第 4.2 条): 部署时要有合理、适当的过滤措施, 协议举的例子有 Falconsai/nsfw_image_detection、NudeNet、CompVis safety checker、Hive、Azure AI Content Safety 和人工审核。HF 模型卡写明, 部署方不做这些就属于违约
  - **标注 AI 生成**(第 4.3 条): 法律或平台有要求时, 须声明内容由 AI 生成
  - **分发与衍生模型命名**(第 3.1 / 3.2 条):
    - 分发模型或含模型的产品时, 要附上协议副本并让接收方受其约束
    - **衍生模型名必须以 "Krea" 开头**, 例如 "Krea 2 [Your Model Name]"
    - Notice 文件里要保留规定的署名句
    - 衍生品须注明已修改, 不得自称官方产品
  - **输出版权**: Krea 不主张用户生成内容的版权(HF 模型卡)
  - 另须遵守 [Acceptable Use Policy](https://www.krea.ai/krea-2-use-policy)
- **推理代码许可**: krea-ai/krea-2 仓库是 Apache-2.0(GitHub), 但权重仍按上面的社区许可

## 踩坑

- **不要直接拿 Raw 出图**: HF 模型卡写的是 "not recommended for inference use"; Comfy 官方博客也说 Raw 不是为成片质量调的
- **ComfyUI 太旧会报红**: 版本低于 0.26.0 时, CLIPLoader 里没有 `krea2` 这个 type(Comfy 博客评论区的用户反馈)。本机 0.37.4, 没有这个问题
- **官方模板的模型文件名不带子目录**: 本机模型放在 `unet/krea2/` 下, 打开模板后要在 UNETLoader 里重选 `krea2\krea2_turbo_int8_convrot.safetensors`。fp8 模板默认找的是 `krea2_turbo_fp8_scaled`, 本机没有这个文件, 要改用 int8 版模板或手动换成 int8
- **本机两个自用工作流缺 LoRA**: 都引用了 `Krea2-功能\Krea2-亚洲hina_krea2Turbo_lora_tqd_v4.0`, 「单采」还引用了 `Krea2-功能\krea2_turbo_lora_rank_64_bf16`; 本机没有 `loras/Krea2-功能/` 目录, 打开会报缺文件。`krea2_turbo_lora_rank_64_bf16` 出自 Comfy-Org/Krea-2, 用途待核实
- **本地复现不了托管版的风格参考和 moodboard**: Comfy 本地风格参考模板用的是社区的 `ostris/krea2_turbo_style_reference` LoRA, 不是 Krea 托管的风格迁移系统; moodboard、`creativity`、生成滑杆也都只在托管版里有
- **风格迁移可能让图变脏、变糊**: 官方 skill 原话。所以它建议只在需要精确匹配时才用, 每次只用 1 张参考图
- **生成滑杆会带来强对比、伪影和 glitch**(官方 skill), 叠加多个或把 complexity 拉到极端时更明显
- **超过 512 token 的提示词会被截断**(官方 `encoder.py`)。ComfyUI 的实现是否同样截断, 待核实
- **GGUF 属于社区量化**: 2026-06-24 有人向 city96/ComfyUI-GGUF 提 issue #464 请求支持 Krea 2, 目前的支持情况待核实。本机已有 int8 权重, 用不到 GGUF

## 待核实

- **许可口径冲突**: VentureBeat 和 Kompozy 说「超过 50 席位须购买企业许可」; 官方许可 v.1 写的是 100 万美元营收门槛。以官方条文为准, 50 席位的说法来源不明
- 开源的 Turbo 和托管的 `medium-turbo` 是不是同一个模型: 目前只有 VentureBeat 说开源 Turbo 派生自 Medium, 官方没有确认。托管 Medium / Large 的参数量未公开, 各用哪个 VAE 也未公开
- 风格参考的数量上限: 网页端和博客写最多 4 张; API schema 写 `maxItems: 10`; Comfy 的托管节点也写最多 10 张
- Artificial Analysis 排名: README 说「独立实验室第 1」, 技术报告说「独立实验室第 2、总榜前 10」
- Raw 的推荐参数: README 是 52 步 / CFG 3.5; `inference.py` 默认 28 步 / 4.5; InvokeAI 文档写约 28 步 / 5.5
- Replicate 的价格; Krea 网页订阅价(第三方 Kompozy 称入门档 $9/月); Comfy Partner Nodes 的价格
- 正式发布的 diffusers 是否已收录 `Krea2Pipeline`(目前官方写的是装源码版)
- Krea 1 的具体发布日期
- HF 模型卡里的 Model Overview / Release Checkpoints 表格没有抓到(HF 直连不通, Tavily 抓取时缺表), 本笔记以 GitHub README 为准
- 中文提示词的效果: 官方没有说明, 模型卡只提到提示词的语言会影响遵循度。本机自用工作流用的是中文提示词
- 本机 `krea2Anime_v15_3196103` 和两个 `loras/krea2/` 下 LoRA 的出处与许可

## 来源

- [krea-ai/krea-2](https://github.com/krea-ai/krea-2): 官方推理代码, 含 README、`docs/prompting.md`、`docs/expansion.txt`、`docs/KREA-2-COMMUNITY-LICENSE`、`encoder.py`、`sampling.py`、`inference.py`; @db3984f(2026-07-24), 取于 2026-09-27
- [krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)、[krea/Krea-2-Raw](https://huggingface.co/krea/Krea-2-Raw): 官方 HF 模型卡, 取于 2026-09-27
- Krea 官方博客, 均取于 2026-09-27:
  - [What Is Krea 2?](https://www.krea.ai/blog/krea-2-image-model)(2026-05-12)
  - [Krea 2 Prompting Guide](https://www.krea.ai/blog/krea-2-deep-dive-walkthrough)(2026-05)
  - [Start Vague](https://www.krea.ai/blog/explorative-prompting-krea-2)
  - [Style references in Krea 2](https://www.krea.ai/blog/style-references-krea-2)
  - [Krea 2 API](https://www.krea.ai/blog/krea-2-api-launch)
  - [Krea 2 Turbo](https://www.krea.ai/blog/krea-2-turbo)
  - [Technical Report](https://www.krea.ai/blog/krea-2-technical-report)
  - [Releasing Open Weights for FLUX.1 Krea](https://www.krea.ai/blog/flux-krea-open-source-release)(2025-07-31)
- Krea 官方文档, 均取于 2026-09-27:
  - [Krea 2 API overview](https://www.krea.ai/docs/developers/krea-2/overview), 以及同目录下的 style-transfer / moodboards / generative-sliders
  - [API reference: Medium / Large / Turbo](https://www.krea.ai/docs/api-reference/krea/krea-2-turbo)
  - [MCP server](https://www.krea.ai/docs/developers/mcp)
  - [用户指南 Krea 2](https://www.krea.ai/docs/user-guide/features/krea)、[用户指南 Krea 2 Turbo](https://www.krea.ai/docs/user-guide/features/krea-2-turbo)
- Krea 官方页面, 均取于 2026-09-27: [开源页](https://www.krea.ai/krea-2-open-source)、[Community License v.1](https://www.krea.ai/krea-2-licensing)、[Acceptable Use Policy](https://www.krea.ai/krea-2-use-policy)、[开源商业许可定价](https://www.krea.ai/open-source-pricing)
- [krea-ai/skills](https://github.com/krea-ai/skills): 官方 skill, MIT, v0.7.6 @ea45cca(2026-09-03), 取于 2026-09-27
- [ComfyUI 文档: Krea-2](https://docs.comfy.org/tutorials/image/krea/krea-2)、[Krea 2 Partner Nodes](https://docs.comfy.org/tutorials/partner-nodes/krea2/krea2-t2i)、[Krea2ImageNode](https://docs.comfy.org/built-in-nodes/Krea2ImageNode): Comfy 官方, 取于 2026-09-27
- [Comfy-Org/workflow_templates](https://github.com/Comfy-Org/workflow_templates): `templates/*krea2*.json` 和 `index.json`, @25ff90a(2026-09-26), 取于 2026-09-27
- [ComfyUI Blog: Krea 2 Open-Source Models are now available in ComfyUI](https://blog.comfy.org/p/krea-2-open-source-models-are-now)、[Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2): Comfy 官方, 取于 2026-09-27
- [fal-ai/krea-2/turbo](https://fal.ai/models/fal-ai/krea-2/turbo)、[Replicate 博客](https://replicate.com/blog/krea-2)、[Replicate official 合集](https://replicate.com/collections/official): 第三方托管, 取于 2026-09-27(Tavily 搜索摘要)
- [InvokeAI: Krea-2](https://invoke.ai/features/krea-2): 第三方, 取于 2026-09-27
- [VentureBeat 报道](https://venturebeat.com/technology/enterprise-grade-ai-image-generation-in-2-seconds-is-here-krea-2-raw-and-turbo-available-as-open-weights-under-custom-license)、[Kompozy 评测](https://kompozy.io/reviews/krea-2): 第三方, 取于 2026-09-27
- 社区仓库的 star 数来自 GitHub search API, 取于 2026-09-27
