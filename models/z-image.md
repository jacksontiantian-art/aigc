---
name: Z-Image(造相)
slug: z-image
vendor: 阿里巴巴通义实验室 Tongyi-MAI(HF / GitHub 组织名 Tongyi-MAI)
modality: image
access: both            # 权重 Apache 2.0 开源; 阿里云百炼另有 z-image-turbo 托管 API
updated: 2026-09-27
---

## 一句话

Tongyi-MAI 的 6B 单流 DiT(S3-DiT)文生图家族, 文本编码器用 Qwen3-4B。已开源两个版本: **Turbo**(2025-11-26, 8 步蒸馏, 不用 CFG, 擅长写实和中英文字渲染)和 **Z-Image**(即 Base, 2026-01-27, 未蒸馏, 支持 CFG 和负面提示词, 适合微调)。**Omni-Base 和 Edit 至今没发权重**。许可 Apache 2.0。

## 变体与版本线

| 变体 | 官方仓库 | 权重 | 发布 | 训练阶段 | 步数 / CFG | 负面提示词 | 定位 |
|---|---|---|---|---|---|---|---|
| **Z-Image-Turbo** | [Tongyi-MAI/Z-Image-Turbo](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo) | ✅ | 2025-11-26 | 预训练 + SFT + RL(Decoupled-DMD 蒸馏 + DMDR) | 8 NFE / 不用 CFG | 不支持 | 出图主力: 画质 Very High, 多样性 Low |
| **Z-Image**(社区和 ComfyUI 文档叫它 "Z-Image-Base") | [Tongyi-MAI/Z-Image](https://huggingface.co/Tongyi-MAI/Z-Image) | ✅ | 2026-01-27 | 预训练 + SFT, 无 RL | 28–50 / CFG 3–5 | 支持, 官方强烈推荐 | 微调底座、风格更多样; 画质 High, 多样性 Medium |
| Z-Image-Omni-Base | 无 | ❌ To be released | — | 只有预训练 | 50 / ✅ | — | 生成与编辑合一的「最原始」底座, 多样性 High |
| Z-Image-Edit | 无 | ❌ To be released | — | 预训练 + SFT | 50 / ✅ | — | 指令编辑 |

- 出处: GitHub README「Model Zoo」表与 News(@26f23ed, 2026-02-09); HF Z-Image 模型卡「Z-Image vs Z-Image-Turbo」表。
- **没有叫 "Z-Image-Base" 的官方仓库**, Base 的官方名字就是 `Tongyi-MAI/Z-Image`。
- 2026-09-27 查看 HF 组织页, Tongyi-MAI 只有 4 个模型: Z-Image-Turbo、Z-Image、MAI-UI-8B、MAI-UI-2B。Edit 和 Omni-Base 仍未上架。
- 技术报告 [arXiv 2511.22699](https://arxiv.org/abs/2511.22699)(2025-12-01)。蒸馏方法另有两篇论文: Decoupled-DMD([2511.22677](https://arxiv.org/abs/2511.22677))和 DMDR([2511.13649](https://arxiv.org/abs/2511.13649))。
- 参数量 6,154,908,736(HF API 的 safetensors 统计)。

## 能力与限制

- **架构**: 单流 DiT。文本 token、视觉语义 token 和图像 VAE token 在序列维拼成同一条输入流(README)。VAE 就是 Flux 1 的 `ae.safetensors`(ComfyUI_examples 说明)。
- **强项**(README / 官方主页): 写实照片级画质、中英双语文字渲染(海报排版、小字号)、世界知识(地标、知名人物、具体物件)。搭配官方 PE(提示词增强)还能做「推理型」出图, 比如鸡兔同笼、古诗配图、经纬度转实景。
- **分辨率**: Base 官方推荐总像素 512²–2048², 任意宽高比(GitHub README)。Turbo 官方 Space 提供 1024 / 1280 / 1536 三档各 11 种比例(`app.py`); 百炼 API 推荐总像素 1024²–1536²。
- **显存**: 官方称 Turbo「fits comfortably within 16G VRAM consumer devices」, 在 H800 上亚秒出图。stable-diffusion.cpp 最低 4 GB 可跑(README Community Works)。
- **Turbo 的代价**: 不同种子之间变化很小(Model Zoo 标 Diversity Low)。HF #8 讨论里有用户反馈, 600–1000 词的长提示词换种子也几乎出同一张图。
- **提示词长度**: Diffusers 管线默认 `max_sequence_length=512` token, 超出截断。本地可以改成 1024(团队成员 QJerry 在 HF #8 的回复)。

## 怎么调用

| 方式 | 说明 |
|---|---|
| **本地 ComfyUI(本人在用)** | 原生支持。官方模板: `image_z_image_turbo`、`image_z_image`(Base)、两者的 `*_int8` 版、`image_z_image_turbo_fun_union_controlnet`、`utility_z_image_turbo_2k_upscaler`(Comfy 官方做的, 不是 Tongyi 官方, MIT, 随 ComfyUI 安装; 本机装的 `comfyui-workflow-templates-json` 0.1.95 与上游 @25ff90a 的关键节点参数一致, 2026-09-27 对比), 默认参数见下方参数表。权重用 Comfy-Org 重打包版: [Comfy-Org/z_image_turbo](https://huggingface.co/Comfy-Org/z_image_turbo)(bf16 / int8_convrot / nvfp4 三种扩散模型, 加 distill patch LoRA; 文本编码器 qwen_3_4b 有 bf16 / fp8_mixed / fp4_mixed; 另有 ae.safetensors)和 [Comfy-Org/z_image](https://huggingface.co/Comfy-Org/z_image)(bf16 / int8_convrot)。GGUF 版要装 ComfyUI-GGUF 插件, 见本机现状 |
| Diffusers | `ZImagePipeline`, README 要求从源码安装 diffusers。也可以用 GitHub 仓库自带的 PyTorch 原生 `inference.py` |
| DiffSynth-Studio(ModelScope) | LoRA 训练、全量训练、蒸馏训练、低显存推理(README Community Works) |
| stable-diffusion.cpp / vllm-omni / SGLang-Diffusion / Candle / Cache-DiT | README 列出的社区推理框架 |
| 官方在线 Demo | HF Space [Z-Image-Turbo](https://huggingface.co/spaces/Tongyi-MAI/Z-Image-Turbo)、[Z-Image](https://huggingface.co/spaces/Tongyi-MAI/Z-Image), 以及 ModelScope 在线生成 |
| 阿里云百炼 API | model `z-image-turbo`, 只有 Turbo。提示词不超过 800 字符, 没有 `negative_prompt` 参数; `prompt_extend=true` 时由大模型改写并返回思考过程, 价格翻倍。每次只出 1 张 |
| fal / Replicate | 第三方托管: `fal-ai/z-image/turbo`(另有 Base 和 Turbo LoRA 端点); Replicate 上是 `prunaai/z-image-turbo`, 由 PrunaAI 做过加速优化, **不是原版权重** |

**ControlNet**: ComfyUI 官方模板用的是 [alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union](https://huggingface.co/alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union)(v1, 支持 Canny / HED / Depth / Pose / MLSD)。新版在 [alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union-2.1](https://huggingface.co/alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union-2.1), 按月份分 2601 / 2602 两批, 各有 lite 版和 Tile 版; 2602 增加了 Scribble / Gray 条件。alibaba-pai 是阿里 PAI 团队, **不是 Tongyi-MAI**, 但被 Comfy 官方模板采用。模板用 `QwenImageDiffsynthControlnet` 接入, 强度 1, 只演示了 Canny; 换其他条件时替换 `Image preprocessing` 子图即可。输入图超过约 2500px 时, 模板注释建议启用缩放节点(选中后按 Ctrl-B)。

### 本机现状(2026-09-27 查看 `E:\ComfyUI-aki-v27\ComfyUI-aki-v2\ComfyUI`, 只读)

秋叶整合包, ComfyUI 0.37.4, RTX 3060。

| 文件 | 位置 | 大小 | 说明 |
|---|---|---|---|
| `z_image_turbo_bf16.safetensors` | `models/unet/z-image-turbo/` | 12.3 GB | Comfy-Org 官方重打包 |
| `z_image_base_Q4_K_M.gguf` | `models/unet/z-image-base/` | 4.93 GB | GGUF 元数据只有 `architecture=lumina2`、`file_type=15`(即 Q4_K_M)。文件大小 4,929,682,496 字节, 和 unsloth(5,066,995,776)、jayn7(4,981,532,736)的 Q4_K_M 都对不上, **来源待核实**。本机没有任何工作流用它 |
| `Z-Image-Turbo-Fun-Controlnet-Union-2.1-2602-8steps.safetensors` | `models/loras/z-image-turbo/` | 6.71 GB | **放错目录**, 见踩坑 ① |
| `z_image_turbo_distill_patch_lora_bf16.safetensors` | `models/loras/z-image-turbo/` | 159 MB | 目录正确, 用途见踩坑 ② |
| `qwen_3_4b.safetensors`、`qwen_3_4b_fp8_mixed.safetensors` | `models/text_encoders/` | 8.0 / 5.6 GB | |
| `ae.safetensors` | `models/vae/` | 335 MB | |
| (空) | `models/model_patches/` | — | 只有占位文件 `put_model_patches_here` |

- 已装 [city96/ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF), 它的 `loader.py` 架构白名单里有 `lumina2`, 能加载 Z-Image 的 GGUF。
- 没有 `extra_model_paths.yaml`(只有 `.example`), 所以各类模型只从默认目录读取。
- 自用工作流 `user/default/workflows/文生图/z-image文生图基础.json`(2026-09-13 保存)的节点和参数, 分析见踩坑 ③:
  - UNETLoader `z-image-turbo\z_image_turbo_bf16.safetensors`
  - CLIPLoader `qwen_3_4b.safetensors`, 类型 `lumina2`
  - VAE `ae.safetensors`
  - ModelSamplingAuraFlow shift 3
  - EmptySD3LatentImage 1024×1024
  - KSampler: 8 步, cfg 1, `res_multistep` / `simple`, denoise 1
  - 正面提示词 `1girl,现实风格，粉色毛衣， 上面写着“骄傲”，精致的脸`
  - 负面提示词 `low quality, bad anatomy, extra digits, missing digits, extra limbs, missing limbs`, 用 CLIPTextEncode 编码后接在 KSampler 的 negative 上
- 其他工作流里也有 Z-Image 段, 但都处于旁路(mode 4)状态:
  - `语义分割/sam3.json`: 一组 Fun ControlNet, 用 ModelPatchLoader 加载 `...Union-2.1-lite-2601-8steps.safetensors`, **本机没有这个文件**; ZImageFunControlnet strength 1; KSamplerAdvanced 9 步 cfg 1 `res_multistep`
  - `qwen2.1.json`: 一组 Turbo 文生图, 8 步 cfg 1 `euler`
  - 两者的 UNETLoader 都写 `z_image_turbo_bf16.safetensors`, 不带子目录, 启用前要重新选成 `z-image-turbo\...`

## 提示词要点

**写提示词调用 skill `z-image-prompt-writing`**, 官方 PE 模板原文 `pe.py` 和官方 Space 的示例提示词放在它的 `references/` 目录。官方要点如下:

1. **Turbo 喜欢长而详细的提示词**。官方建议先手写, 再交给 LLM 用官方 PE 模板扩写(团队成员 Dongyang Liu 在 HF #8 的回复)
2. **官方 PE 模板**是官方 Space 里的 `pe.py`, 中文 system prompt, 开头是「你是一位被关在逻辑牢笼里的幻视艺术家…」。它规定了四步流程:
   - 锁定不能改动的核心要素: 主体、数量、动作、状态、IP 名、颜色、文字
   - 判断是否需要「生成式推理」: 需求是问答、设计或解题时, 先构思出可以画出来的具体方案
   - 补齐构图、光影、材质、色彩和空间层次
   - 最后处理文字
3. **画面里的文字**要一字不差地写出来, 用**英文双引号**括住。海报、菜单、UI 要写全所有文字以及字体和排版; 招牌、屏幕上的字要写明内容、位置、尺寸和材质(PE 模板)
4. **客观具象**: 不用比喻和情绪化修辞, 不加「8K」「杰作」这类元标签或绘制指令(PE 模板)
5. **中英文都可以写**。官方示例两种语言都有, 也有中英混写(如 `pagoda (西安大雁塔)`)。有位社区用户称 PE 扩写成英文后效果更好, 只是个例
6. **Turbo 不用负面提示词**: 蒸馏后不依赖 CFG, 负面提示词完全不起作用(HF #8 团队回复)。**Base 相反**, 负面提示词效果明显, 官方「强烈推荐」使用(GitHub README)
7. **Base 的 CFG normalization**: 做风格化时设 `False`, 追求写实时设 `True`(GitHub README)
8. **长度**: 本地 Diffusers 默认按 512 token 截断, 可以调到 1024; 百炼 API 上限 800 字符
9. **Turbo 换种子几乎不变**: 要多样性就改提示词本身, 或者换用 Base
10. **官方 Space 怎么调 PE**: 把用户输入填进 `{prompt}`, 整段作为 system 消息, user 消息只放一个空格; 调 DashScope `qwen3-max-preview`, temperature 0.7, top_p 0.8(`app.py`)。**Space 目前把 PE 关掉了**(代码注释 "Temporarily disabled")

## 参数

| 参数 | Turbo 官方(Diffusers / Space) | Turbo ComfyUI 官方模板 | Base 官方 | Base ComfyUI 官方模板 |
|---|---|---|---|---|
| 步数 | `num_inference_steps=9`, 注释说实际是 8 次 DiT 前向; Space 滑块显示 8, 代码里 +1 | 8 | 28–50(README); 示例 50; Space 默认 30 | 25(模板注释写 Steps 30～50) |
| CFG | `guidance_scale=0.0`, 注释「Guidance should be 0 for the Turbo models」 | cfg 1, negative 接 ConditioningZeroOut | 3.0–5.0, 示例 4 | cfg 4, negative 为空文本 |
| 采样 | FlowMatchEuler, shift 3.0(Space) | `res_multistep` / `simple`, ModelSamplingAuraFlow 3 | — | `res_multistep` / `simple`, ModelSamplingAuraFlow 3 |
| 尺寸 | 示例 1024², Space 三档 | EmptySD3LatentImage 1024² | 总像素 512²–2048², 示例 720×1280 | 1024² |
| 文本编码器 | Qwen3-4B | `qwen_3_4b`, CLIPLoader 类型选 `lumina2` | 同左 | 同左 |

## 价格与许可

- **许可 Apache 2.0**: GitHub 仓库的 LICENSE, 以及两个 HF 模型卡的 license 标签, 可以商用。alibaba-pai 的 ControlNet 和 DiffSynth 的 DistillPatch 也是 Apache 2.0(HF 标签)
- 本地运行不花钱
- **阿里云百炼**(北京区): `prompt_extend=false` 每张 0.1 元, `=true` 每张 0.2 元, 前 90 天免费 100 张; 新加坡区 0.110089 / 0.220177 元
- **fal**: 约 $0.005 每百万像素(第三方汇总站 Lumenfall)。**Replicate**(prunaai): 不超过 1 MP 每张 $0.005(Replicate 页面)

## 踩坑

- **① Fun ControlNet 放在 `loras/` 下, ComfyUI 找不到**
  - 官方模板用 `ModelPatchLoader` 加载它, 这个节点只列 `models/model_patches/` 里的文件: ComfyUI 0.37.4 源码 `comfy_extras/nodes_model_patch.py` 第 235 行调用 `get_filename_list("model_patches")`, `folder_paths.py` 第 55 行把它映射到 `models/model_patches`
  - Comfy 官方文档和模板也写明放 `models/model_patches/`
  - 这个文件本身也不是 LoRA: 本机读取文件头, 共 295 个张量、3.36B 参数, 没有任何 lora 键。所以用 LoRA 加载器加载也不会生效
  - **处理**: 把它移到或复制到 `models/model_patches/`(本次只读, 未动文件)。ComfyUI 0.37.4 的加载代码认识 2.1 的 15 层结构; 本机文件的 `control_noise_refiner` 权重非零, 不会被判为 broken
  - 官方模板里默认填的是 v1 文件名, 打开后要手动改选 2.1-2602
- **② `z_image_turbo_distill_patch_lora_bf16` 只给 Turbo 用, 而且只在挂了「直接在 Turbo 上训练的 LoRA」时才需要**
  - 出处是 ModelScope 的 DiffSynth-Studio 团队发布的 [Z-Image-Turbo-DistillPatch](https://huggingface.co/DiffSynth-Studio/Z-Image-Turbo-DistillPatch)(2025-12-12, Apache 2.0)。Comfy-Org 把它转成 bf16, 放进 `Comfy-Org/z_image_turbo` 的 `split_files/loras/`, README 也写明放 `loras/`
  - 本机文件的参数量 79,380,480, 和 DiffSynth 原版(F32)完全一致, 键名带 ComfyUI 的 `diffusion_model.` 前缀
  - 用途: 直接在 Turbo 上训练的 LoRA 会丢掉加速能力, 按 8 步 cfg 1 出图会糊, 30 步 cfg 2 才正常。推理时把这个补丁和那个 LoRA 一起挂上, 就能恢复 8 步出图
  - **不是给 Base 用的**; Turbo 不挂 LoRA 时也不需要。有社区用户称纯 Turbo 再叠这个补丁等于「turbo 叠两次」, 偶尔反而出伪影(Civitai 转载页作者自述)
  - Base 想少步数出图要用另一个东西: alibaba-pai 的 Z-Image-Fun-Lora-Distill(4 / 8 步, 同时蒸馏步数和 CFG, 不含 Turbo 权重)。这条信息来自 cnb.cool 上的 README 镜像, HF 原页未核实
- **③ 自用工作流: 模型和采样参数与官方 Turbo 模板完全一致, 没有冲突。差别有两处, 都不致命**
  - 负面提示词没用: 官方模板的 negative 接的是 ConditioningZeroOut, 自用工作流接了一段真实文本。cfg=1 时 ComfyUI 直接跳过 uncond 计算(`comfy/samplers.py` 第 610 行), 这段负面提示词完全不起作用, 只是多编码一次; 官方也明确说 Turbo 不用负面提示词。要去掉瑕疵, 应该写进正面描述, 或者改用 Base
  - 正面提示词太短, 而且是 tag 写法。官方要长而详细的自然语言描述, 要渲染的文字用英文双引号(PE 模板); 这里用的是中文弯引号 “骄傲”。官方 README 和 Space 的示例全部用 ASCII 直引号 `"`(逐字节核对, 共 38 处, 没有弯引号), 按官方写法改成 `"骄傲"`
- **GGUF 要用 GGUF 专用节点**: ComfyUI 原生的 UNETLoader 读不了 `.gguf`, 要用 ComfyUI-GGUF 的 `Unet Loader (GGUF)`。本机已装, 它会扫描 `unet` / `diffusion_models` 目录。本机的 GGUF 是 **Base**, 要按 Base 的参数跑(CFG 3–5、28–50 步、写负面提示词), **不能**直接套 Turbo 的 8 步 cfg 1 工作流
- **RTX 3060 显存**: Turbo bf16 文件 12.3 GB, 加上 8 GB 的 bf16 文本编码器, 超出显存时 ComfyUI 会自动卸载到内存, 速度会下降。可以换官方 int8 模板(`z_image_turbo_int8_convrot` + `qwen_3_4b_fp8_mixed`, 后者本机已有)。本机显卡是 12 GB 还是 8 GB 版、int8_convrot 在 Ampere 上的效果, 均待核实
- **以为 PE 在 GitHub 仓库里**: 其实不在。GitHub 主仓库全部 78 个提交里都没有 `pe.py`, 也没有 `skills/` 目录; PE 只在两个官方 HF Space 里(两处内容相同)
- **官方主页称已公开发布 Z-Image-Edit**(「We are publicly releasing … Z-Image-Turbo for generation and Z-Image-Edit for editing」), 但 README 和 HF 上都没有 Edit 权重, 以 README 为准

## 社区资源(均未引入, star 为 2026-09-27 GitHub 搜索)

官方**没有** skill: Tongyi-MAI 组织的 8 个仓库都没有 `skills/` 或 `.claude/skills/`。社区里值得看的:

| 仓库 | ★ | 最近更新 | 性质 | 建议 |
|---|---|---|---|---|
| [leejet/stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp) | 7403 | 2026-09-25 | 社区, 官方 README 引用 | 低显存 / C++ 推理, 本机用 ComfyUI 就不需要 |
| [martin-rizzo/AmazingZImageWorkflow](https://github.com/martin-rizzo/AmazingZImageWorkflow) | 504 | 2026-01-15 | 社区, Unlicense | 预置风格的 Turbo 工作流(有 GGUF / safetensors 两版), 可以参考风格写法 |
| [martin-rizzo/ComfyUI-ZImagePowerNodes](https://github.com/martin-rizzo/ComfyUI-ZImagePowerNodes) | 362 | 2026-09-01 | 社区 | Z-Image 专用节点集, 需要时再看 |
| [Koko-boya/Comfyui-Z-Image-Utilities](https://github.com/Koko-boya/Comfyui-Z-Image-Utilities) | 130 | 2026-03-11 | 社区, Apache-2.0 | ComfyUI 的 PE 节点。内置官方 PE 中文模板(已逐字核对一致), 另附**社区自译**的英文版。想在 ComfyUI 里直接接 LLM 做 PE 时可用 |
| [BennyDaBall930/ComfyUI-Z-Engineer](https://github.com/BennyDaBall930/ComfyUI-Z-Engineer) | 101 | 2026-08-19 | 社区 | 用自己微调的 LFM2.5-1.2B 做扩写, system prompt 也是自己写的, 不是官方 PE |
| [yizhiyanhua-ai/zimage-skill](https://github.com/yizhiyanhua-ai/zimage-skill) | 52 | 2026-01-22 | 社区, MIT | Claude Code skill, 只是调 ModelScope API 出图, 没有提示词方法论, **不值得引入** |

星数很少(<20)的提示词 skill 和合集不推荐, 例如 maciejdzierzek/z-image-prompt-generator(1★)、L-Trunks/ai-portrait-light(8★)、weshopai/awesome-Z-Image-prompts(11★)。

## 待核实

- **Omni-Base 和 Edit 何时发布**: README @26f23ed(2026-02-09)仍写 To be released; 2026-09-27 HF 组织页没有这两个模型; ModelScope API 查这两个名字返回 404。官方主页的「已发布 Edit」说法与此矛盾
- **Z-Image Turbo++**([arXiv 2606.12575](https://arxiv.org/abs/2606.12575)): 从 Turbo 再蒸馏出的 2 步模型。作者是否是 Tongyi-MAI、有没有权重, 都没查到
- **两个 README 的推荐参数不一样**: GitHub README 的 Base「Recommended Parameters」有 5 条; HF 模型卡当前版只有 3 条, 没有负面提示词和 CFG normalization 两条。HF 旧版对比表的步数写 "50", 新版写 "28~50"
- **步数是否等价**: Diffusers 写 9 步(= 8 次前向), ComfyUI 模板写 8 步。两者应该都是 8 次模型调用, 这是推断, 没有实测
- **ComfyUI 会不会按 512 token 截断**: HF #8 里有人推荐用 `ConditioningTruncate`、`CLIPTokenCounter` 这类社区节点来处理, 但 ComfyUI 原生的截断行为没有查证
- **本机 Base GGUF 的来源**: 见本机现状
- **2.1-2602 ControlNet 的推荐强度**: 模型卡「Model Features」一节没抓到正文, 只看到 scale 0.65–1.0 的对比图; ComfyUI 官方模板只给过 v1 的用法(strength 1)
- **百炼的 `prompt_extend` 改写**用的是不是同一份 PE 模板
- 百炼 API 文档里 size 的写法, 宽高之间是 `*` 还是 `×`(抓取时这个符号丢了)
- alibaba-pai Z-Image-Fun-Lora-Distill 的 HF 原页

## 来源

- [Tongyi-MAI/Z-Image](https://github.com/Tongyi-MAI/Z-Image) — 官方 GitHub README、LICENSE, @26f23ed (2026-02-09), 另查了全部 78 个提交的文件历史, 取于 2026-09-27
- [Tongyi-MAI 组织仓库列表](https://github.com/Tongyi-MAI)(GitHub API)、[HF 组织页](https://huggingface.co/Tongyi-MAI) — 取于 2026-09-27
- [HF 模型卡 Tongyi-MAI/Z-Image](https://huggingface.co/Tongyi-MAI/Z-Image) @04cc4ab (2026-01-28)、[Tongyi-MAI/Z-Image-Turbo](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo) @f332072 (2026-01-30) — 官方, 取于 2026-09-27
- [HF Space Z-Image-Turbo `pe.py`](https://huggingface.co/spaces/Tongyi-MAI/Z-Image-Turbo/blob/main/pe.py) — 官方 PE 模板, 由 QJerry 上传, commit 9baf60e; 最早存档 [2025-11-27](https://web.archive.org/web/20251127201114/https://huggingface.co/spaces/Tongyi-MAI/Z-Image-Turbo/blob/main/pe.py); 同文件也在 [Z-Image Space](https://huggingface.co/spaces/Tongyi-MAI/Z-Image/raw/main/pe.py); 取于 2026-09-27
- [HF Space Z-Image-Turbo `app.py`](https://huggingface.co/spaces/Tongyi-MAI/Z-Image-Turbo/raw/main/app.py)、[Z-Image `app.py`](https://huggingface.co/spaces/Tongyi-MAI/Z-Image/raw/main/app.py) — 官方, 用于 PE 调用方式、采样参数、分辨率表, 取于 2026-09-27(没拿到 commit 号)
- [HF 讨论 #8 PROMPTING GUIDE](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo/discussions/8) — 团队成员 Dongyang Liu、QJerry 的回复(身份靠头像和组织成员列表比对), 取于 2026-09-27
- [官方主页 Z-Image-blog](https://tongyi-mai.github.io/Z-Image-blog/) — 官方, 仓库最后推送于 2025-12-13, 取于 2026-09-27
- [Z-Image 技术报告 arXiv 2511.22699](https://arxiv.org/abs/2511.22699) — 官方(正文未抓到, PE 训练方式来自 [alphaXiv 摘要](https://www.alphaxiv.org/abs/2511.22699v3)), 取于 2026-09-27
- [ComfyUI 文档 Z-Image-Turbo](https://docs.comfy.org/tutorials/image/z-image/z-image-turbo)、[Z-Image](https://docs.comfy.org/tutorials/image/z-image/z-image) — Comfy 官方, [Comfy-Org/docs](https://github.com/Comfy-Org/docs) @b0605b5 (2026-09-26), 取于 2026-09-27
- [Comfy-Org/workflow_templates](https://github.com/Comfy-Org/workflow_templates) — `image_z_image*.json` 五个模板, @25ff90a (2026-09-26), 取于 2026-09-27
- [Comfy-Org/z_image_turbo](https://huggingface.co/Comfy-Org/z_image_turbo) @08d0445 (2026-08-17)、[Comfy-Org/z_image](https://huggingface.co/Comfy-Org/z_image) @94d47a4 (2026-09-23) — Comfy 官方重打包, 文件清单来自 HF API, 取于 2026-09-27
- [ComfyUI_examples: Z Image](https://comfyanonymous.github.io/ComfyUI_examples/z_image/) — Comfy 官方, 取于 2026-09-27
- 本机 ComfyUI 0.37.4 源码 `comfy_extras/nodes_model_patch.py`、`folder_paths.py`、`comfy/samplers.py`, 以及 `custom_nodes/ComfyUI-GGUF/loader.py`、本机模型文件头 — 只读查看, 2026-09-27
- [alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union-2.1](https://huggingface.co/alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union-2.1) @5155fc5 (2026-02-26)、[v1 仓库](https://huggingface.co/alibaba-pai/Z-Image-Turbo-Fun-Controlnet-Union) @01c794d — 阿里 PAI, 取于 2026-09-27
- [DiffSynth-Studio/Z-Image-Turbo-DistillPatch](https://huggingface.co/DiffSynth-Studio/Z-Image-Turbo-DistillPatch) @a627ca9 (2025-12-16) — ModelScope DiffSynth 团队, 取于 2026-09-27
- [Civitai: Z-Image Turbo Distill Patch for ComfyUI](https://civitai.com/models/2233614/z-image-turbo-distill-patch-for-comfyui) — 社区转载, 取于 2026-09-27
- [alibaba-pai Z-Image-Fun-Lora-Distill README(cnb.cool 镜像)](https://cnb.cool/ai-models/alibaba-pai/Z-Image-Fun-Lora-Distill/-/blob/main/README.md) — 取于 2026-09-27
- [unsloth/Z-Image-GGUF](https://huggingface.co/unsloth/Z-Image-GGUF) @c9913e6、[jayn7/Z-Image-GGUF](https://huggingface.co/jayn7/Z-Image-GGUF) @a40d12f — 社区量化, 用于比对文件大小, 取于 2026-09-27
- [百炼 z-image API 参考](https://help.aliyun.com/zh/model-studio/z-image-api-reference)、[模型价格](https://help.aliyun.com/zh/model-studio/model-pricing) — 阿里云官方, 取于 2026-09-27
- [fal Z-Image Turbo](https://fal.ai/z-image-turbo)、[Replicate prunaai/z-image-turbo](https://replicate.com/prunaai/z-image-turbo)、[Lumenfall 价格汇总](https://lumenfall.ai/models/alibaba/z-image-turbo/providers?mode=text-to-image) — 第三方, 取于 2026-09-27
- 社区仓库 star 数: GitHub search API, 2026-09-27; Koko-boya/Comfyui-Z-Image-Utilities @6e07c28、yizhiyanhua-ai/zimage-skill @25a2b5e 读过源码
