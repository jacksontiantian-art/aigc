---
name: Qwen-Image-2.1 (千问图像 2.1)
slug: qwen-image-2-1
vendor: 阿里巴巴 Qwen 团队(杭州通义实验室)
modality: image
access: open-weights    # 官方托管 API 是否提供 2.1: 待核实
updated: 2026-09-27
---

## 一句话

千问图像系列在 2026-09-20 开源的文生图 + 图像编辑一体模型: 视觉生成部分只有 7B, 原生 2K、原生透明图(RGBA)、最多 10 张参考图; **许可仅限非商用**。

## 能力与限制(官方 README / 模型卡, @fb7ae1d)

- **架构**: 32 层 single-stream DiT, 7B; 文本编码器是 Qwen3-VL 8B, 指令和条件图都由它编码; VAE 为 64 通道 RGBA, 16 倍空间压缩; 采样用 Flow Matching + Euler, 带 dynamic shifting
- **一个 checkpoint 同时做文生图和编辑**。有前缀 KV cache 复用: 文本和条件图只在第一步计算, 后续步直接复用, 参考图越多提速越明显
- **尺寸**: 原生 2K, 7 档推荐尺寸从 2048×2048 到 2752×1536(完整表见 skill `qwen-image-prompt-writing`); 宽高必须是 32 的倍数(Diffusers 代码)
- **透明图**: 可从文本生成 RGBA、编辑透明图层(包括改图层里的文字)、把 RGB 照片里的主体抽成 RGBA; 出普通图还是透明图由提示词决定
- **编辑**: 最多 10 张参考图, 官方示例有六人单照合成合影、五件单品搭一身穿搭。局部编辑支持圈选、涂抹、独立蒙版, 并强化了人像身份和商品文字/材质/形状的保真
- **其他官方展示**: 排版与文字渲染、从一张自拍生成全景、由三视图生成分镜、信息图
- **跑分**(厂商自测, 第三方转述): Qwen-Image-Bench 总分 60.28, 开源第一, 高于 Nano Banana 2.0(59.82)和 GPT Image 1.5(59.65); 独立评测尚未出现
- **沿革**(第三方报道): 透明图能力来自 2025-12 单独发布的 Qwen-Image-Layered, 2.1 把它并进了通用模型

## 怎么调用

| 方式 | 说明 |
|---|---|
| 本地 ComfyUI(**本人在用**) | 原生支持, 官方模板要求 ComfyUI ≥ 0.37.0(第三方 comfyui-wiki 称)。权重用 [Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1) 重打包版。三个官方模板(文生图 / 图像编辑 / 去背景)见下方列表 |
| Diffusers | `QwenImage21Pipeline`, 官方首推 |
| vLLM-Omni / SGLang / LightX2V | Day-0 支持, 用于服务化和加速 |
| ModelScope(DiffSynth-Studio) | 在线生成, 可训练 LoRA |
| wuli.art | 官方 README 推荐的国内免费在线入口, 支持透明图 |
| 官方托管 API | 截至 2026-09-27, 百炼文生图模型表只有 3.0 / 2.0 / max / plus 系列, 没有 2.1(调研代理查阅, 未附链接)。kie.ai 自称出售「可商用的 2.1 API」, 与非商用许可冲突, 不要用 |

**别和同名兄弟混淆**: Qwen-Image-2.0(2026-02-10, 旧仓库 [QwenLM/Qwen-Image](https://github.com/QwenLM/Qwen-Image) README @6b5e1f5)和 Qwen-Image-3.0(2026-07-21, 第三方报道)都**只有 API, 没有权重**; 2.1 比 3.0 晚发布, 是另一条开源线。fal / Replicate 上的 "Qwen-Image 2" 是 2.0。docs.comfy.org 教程页的 "GitHub Repository" 链接指向旧仓库, 2.1 的正确仓库是 [QwenLM/Qwen-Image-2.1](https://github.com/QwenLM/Qwen-Image-2.1)。

**提示词改写**: 官方另发了两个改写模型 PE-T2I / PE-I2I(Qwen3.5-VL 9B 微调, bf16 约 20 GB), 把短需求扩写成官方格式并推荐比例。ComfyUI 官方模板里用 `TextGenerate` 节点加载它们的 int8 版, 由 `refine_prompt` 开关控制: 文生图模板默认关, 编辑模板默认开(上游新版模板才有, 本机装的旧版没有, 见下方)。本仓库的 skill **`qwen-image-prompt-writing`** 用同一份 system prompt 让 Claude 代为改写, 不占显卡。

**ComfyUI 官方模板**(Comfy 官方做的, 不是 Qwen 官方; [Comfy-Org/workflow_templates](https://github.com/Comfy-Org/workflow_templates), MIT, 随 ComfyUI 安装。下面按上游 @99e3d43 (2026-09-25) 记):
- `image_qwen_image_2_1_t2i`: 文生图, 可选接 PE-T2I 改写, `refine_prompt` 默认关; 默认提示词是一段英文时装大片描述
- `image_qwen_image_2_1_image_edit`: 单图 / 多图编辑, 最多 10 张参考图, 可选接 PE-I2I 改写, `refine_prompt` 默认开(默认提示词只有 `Replace costumes for the character`, 靠 PE 扩写)。`image_1` 是被编辑的画布, 其余是参考图, 提示词里用 `<image1>`…`<image10>` 指代
- `image_qwen_image_2_1_background_removal`(@371a7b7, 2026-09-20): 去背景, 输出透明 PNG, 没有 PE 开关; 提示词见 `prompts/image/background-removal.md`
- `Qwen Image 2.1 Cache` 节点: `device` 可选 auto / gpu / cpu / off, `dtype` 可选 default / int8 / int4。显存吃紧时把 cache 放到 cpu, 或改用 int8(Comfy 文档说几乎不掉精度)
- **本机装的模板比上游旧**: ComfyUI 0.37.4 的 `requirements.txt` 锁定 `comfyui-workflow-templates==0.11.69`, 对应 `comfyui-workflow-templates-json` 0.1.95。这一版的文生图和编辑模板**还没有 PE**(没有 `TextGenerate` 节点和 `refine_prompt` 开关), 文生图模板也没有 Cache 节点; PE 版从 0.1.96 开始有(2026-09-27 对比本机文件和 PyPI wheel)。去背景模板两版相同。升级 ComfyUI 后再核对一次; 手动 `pip install -U` 会偏离 ComfyUI 锁定的版本

### 本机现状(2026-09-27 查看 `E:\ComfyUI-aki-v27`)

- ComfyUI 0.37.4。已下载: `qwen_image_2.1_int8_convrot`(7.26 GB, 放在 `models/unet/Qwen-image2.1/`)、`qwen3vl_8b_int8_convrot`(10.99 GB)、`qwen_image_2.1_vae_bf16`(0.68 GB)、PE-T2I / PE-I2I 的 int8 版(各 9.47 GB)
- 官方模板里的文件名不带子目录, 打开后要在 UNETLoader 里重选 `Qwen-image2.1\qwen_image_2.1_int8_convrot.safetensors`
- 已用的社区组件:
  - [Viggle turbo 4 步 LoRA](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)(见踩坑)
  - `TE-Speed-QwenImage21` 加速节点(闭源 `.pyd`, 自称提速 30–40%, 来源待核实)
  - `TE_Qwen_Image_2_1_Prompt_Enhancer` 节点

### 社区 skill(均未引入, 2026-09-27 查看)

官方仓库没有 `skills/` 目录, QwenLM 组织下也没有 Qwen-Image-2.1 的 skill。官方插件集 [QwenLM/Qwen-MM-Plugins](https://github.com/QwenLM/Qwen-MM-Plugins) 做的是多模态**理解**(OCR、视频记忆、剪辑等), 不包含 2.1 本地生图。社区版本如下:

| 仓库 | 许可 | 特点 |
|---|---|---|
| [iamyoki/qwen-image-2.1-skill](https://github.com/iamyoki/qwen-image-2.1-skill) | Apache-2.0 | 星最多(107)。转述官方规则, 输出为带 emoji 的中文三段式, 附 `validate_prompt.py` |
| [dseditor/qwen-image-21-prompting](https://github.com/dseditor/qwen-image-21-prompting) | MIT | 原样收录官方 system prompt, 另有实测的能力边界和 RGBA 专题, 只管提示词这一层 |
| [xiaonan0527/qwen-image-gen](https://github.com/xiaonan0527/qwen-image-gen) | MIT | 改写之后直接调本地 ComfyUI 出图, 带测试 |
| [0c0/qwen-image2.1-prompt-writing-skill](https://github.com/0c0/qwen-image2.1-prompt-writing-skill) | 无 | 复刻官方 PE 契约 |

其他值得收藏的社区资源(star 为 2026-09-27 GitHub 搜索):

- [comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI) `comfy_extras/nodes_textgen.py` 等 — @79be670 (2026-09-25), 取于 2026-09-27
- [wildminder/awesome-qwen-image](https://github.com/wildminder/awesome-qwen-image)(82★): 权重 / 量化 / 蒸馏 LoRA / GGUF 索引, 更新勤。它列出三条**互不兼容**的加速线: Viggle turbo(DMD)、[alibaba-pai Fun-Acc](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Acc-LoRAs) 4 步(阿里 PAI 出品, 但不是 Qwen 团队, Qwen README 未引用)、[Pruna](https://huggingface.co/PrunaAI/Pruna-Qwen-Image-2.1) 8 / 5 步(各有固定 sigma 表, 要求 shift 1.0、关 dynamic shifting); 只能选一条, 不能叠
- [benjiyaya/ComfyUI-Qwen-Image-2.1-Prompt-Enhancer](https://github.com/benjiyaya/ComfyUI-Qwen-Image-2.1-Prompt-Enhancer)(160★): ComfyUI 改写节点; 官方模板已内置 PE, 优先用官方
- 补充: Qwen-MM-Plugins 里并非完全没有生图, `omni-chatcut` 的 music-to-mv 带一个 Qwen-Image **3.0** API 的 provider(`image_providers/qwen_image30.py`, 文件树 @0773667), 但它走云端, 与本地 2.1 无关

## 提示词要点(官方改写模型的 system prompt)

写提示词直接调用 skill `qwen-image-prompt-writing`, 规则原文在 `.claude/skills/qwen-image-prompt-writing/references/`。要点如下:

- **文生图**: 一段英文、约 20 句、400–500 词, 以旁观者口吻描述成品画面。结构是开头一句(媒介 + 风格 + 主体 + 背景)→ 8–14 个方位短语走遍画面 → 光线单独一句 → 一句总括。不写质量词, 正文不写比例; 画面里的文字用直双引号, 保留原文字种
- **编辑**: 只改点名的属性并改到位, 其余用一句保持条款带过、不重新描述。以操作开头, 用肯定句。引号外的描述跟随指令语言(中文指令就写中文); 多图必须用 `<image1>`… 并说明每张图的角色; 人物身份指向参考图, 不用文字描述
- **透明图**: `This is an RGBA image with transparency. <your description>. The image has alpha channel and the background is transparent.`(README 原句, 收录在 `prompts/image/rgba-transparent-asset.md`)
- **局部编辑**: 可用不同颜色圈出多个区域, 在提示词里按颜色指代; 要保留原图信息时, 把原图和黑白蒙版作为两张输入图(第三方转述官方博客, 官方未给固定句式)

## 参数

| 参数 | 官方 | ComfyUI 官方模板 | 说明 |
|---|---|---|---|
| 步数 | 40(Diffusers 默认, 文档称是 Qwen 推荐值) | 25 | 模板注释: 官方管线约 40–50 步 euler |
| CFG | `true_cfg_scale=1.0`, 即不开 | cfg 1 | 大于 1 时必须配负面提示词才生效, 每步计算量翻倍 |
| 采样器 | Flow Matching Euler | euler / simple | |
| 尺寸 | 见 7 档表 | 文生图默认 1 MP(1024²), 要原生 2K 设约 4 MP | |

## 价格与许可

- **[Qwen Research License](https://github.com/QwenLM/Qwen-Image-2.1/blob/main/LICENSE)**(2026-09-20): 只授权非商用, 协议把非商用定义为「仅限研究或评估」。商用须另行申请, 联系 model-business@notice.qwencloud.com
- 再分发须附带协议副本, 并在 NOTICE 文件里保留版权声明。用它的输出训练并发布模型时, 须标注「Built with Qwen」。衍生品不得以 "Qwen" 作为主名
- 这是收紧: 前代 Qwen-Image 用的是 Apache 2.0(旧仓库 README 与 GitHub license 字段)。**给客户干活或商用出图前, 先拿到商用许可**
- 本仓库的 skill 按上述条款附了 `LICENSE` 和 `NOTICE`
- 本地运行不花钱

## 踩坑

- **Diffusers 不传宽高时出图是 1024×1024**, 不是 README 表格写的 2048: 管线参数 `output_resolution` 默认 1024(diffusers @6256aa7)。要 2K 就显式传 `width` / `height`
- **多图编辑时两边的输出比例不一样**: Diffusers 跟随**最后一张**输入图的比例(同一处代码), ComfyUI 模板在 `custom_size` 关闭时跟随 `image_1`。官方改写模型会给出 `ratio_follow`, 要按它显式设置尺寸
- **ComfyUI 编辑模板的 `resolution`** 是像素总预算, 不是边长; 设 0 表示保持参考图原尺寸, 只取整到 32 的倍数。开 `custom_size` 时画布要接近 `image_1` 缩放后的尺寸, 否则编辑结果会偏移(模板注释)
- **透明图句式有两个版本**: README 是 "This is an RGBA image …"; ComfyUI 模板注释是 "This is an RGBA format image … an alpha channel and a transparent background", 哪个更好待实测。必须存 PNG
- **官方 PE 改写模型的采样参数不能乱改**: 必须开 thinking; T2I 用 presence_penalty 1.5, 编辑用 0。拿原版 Qwen3.5-VL 9B 代替, 大多数情况下输出的 JSON 解析不了(官方 `prompt_rewrite/README.md`)
- **ComfyUI 模板里的 PE 参数和官方不一致**(按 `TextGenerate` 的控件顺序解读模板 JSON; 节点源码 `comfy_extras/nodes_textgen.py` @79be670, 模板 @25ff90a):

  | | 官方 T2I | 模板 T2I | 官方 Edit | 模板 Edit |
  |---|---|---|---|---|
  | thinking | 必须开 | **关** | 必须开 | 开 |
  | presence_penalty | 1.5 | **0** | 0 | 0 |
  | min_p | 0 | 0 | 0 | 0.05 |
  | repetition_penalty | 不用 | 1.05 | 不用 | 1.05 |
  | max tokens | 16256 | 16256 | 24000 | 16256 |

  官方说没有 thinking 时改写模型会退化, presence_penalty 错了也不会报错(`prompt_rewrite/README.md`)。所以文生图模板的 `refine_prompt` 就算打开, 效果也不等于官方 PE; 要么在界面上把 thinking 和 presence_penalty 改成官方值, 要么用 skill 改写后关掉它
- **不要沿用前代的习惯**: 旧版 Qwen-Image 官方示例会在提示词后面拼 `", Ultra HD, 4K, cinematic composition."`, 尺寸表是 1328² 那一套, 还配负面提示词和 true_cfg 4(旧仓库 README @6b5e1f5)。2.1 的改写规则明确不写质量词, 尺寸表换成 2K 那 7 档, 默认也不开 CFG
- **编辑节点的槽位比模型能力多**: ComfyUI 的 2.1 文本编码节点开了 `image_1`–`image_16` 共 16 个槽位(`comfy_extras` 源码 @79be670), 但官方只支持最多 10 张参考图
- **Viggle turbo LoRA 是预览版**: 本机装的是 `loras/qwen-image2.1/Qwen-Image-2.1-viggle-turbo-4step-lora-r64`(2026-09-23 下载), 即 v0.1 的 4 步版, CFG 1, 支持 1–3 张参考图。上游 2026-09-24 已出 v0.2.1, 改为 6 步, sigma 表 `[1.0, 0.9375, 0.875, 0.75, 0.5, 0.25]`(awesome-qwen-image @834276c 转述, 未到 HF 复核), 升级时整套采样要跟着换。Viggle 自己在模型卡里写明, 复杂编辑(多参考合成、换脸、保身份、多约束指令)明显差于 40 步原版, 只有文生图算可用。本机「图像编辑-加速」工作流挂了它, 复杂编辑要换回原版
- **社区实测(dseditor/qwen-image-21-prompting, 2026-09, 未复核)**:
  - 360° 全景左右边缘接不上, 不能当 VR 环境贴图
  - 透明图里如果文字本身就是抠出来的主体, 字形会崩; 文字印在不透明底板(比如按钮)上则没问题
  - 透明图空白区 alpha 会残留 1–31 的低值, 交给游戏引擎前做一次 `alpha[alpha < 32] = 0`; 检查透明效果要看 alpha 分布, 不要只看图片模式
  - 角度数字控制不了转身, "seen from directly behind" 这类语义视角词才有效
  - 每张参考图约多占 2 GB 的 KV cache
- **社区反馈(comfyui-wiki 汇总)**: 分辨率明显超过 2048 级别时曝光会不稳; 参考图太大会显著变慢, 先缩小再输入; 用参考图做风格迁移不稳定, 用文字描述风格更可靠

## 待核实

- **CFG 该不该调高**: 官方默认不开 CFG, ComfyUI 称「只有用负面提示词时才调高」。社区说法互相矛盾: 有人说模型是蒸馏的、调高会烧图; 也有人说出现畸形或发黄时把 CFG 调到 2–3 并配负面提示词能改善
- 早期体验者称参考图从 3 张左右开始一致性下降(第三方转述)
- 官方博客 https://qwen.ai/blog?id=qwen-image-2.1 是 JS 渲染的, Tavily 和 curl 都抓不到正文; 本文关于博客的内容全部来自第三方转述
- 百炼(阿里云)是否上架 2.1 的托管 API(2026-09-27 未见)

## 来源

- [QwenLM/Qwen-Image-2.1](https://github.com/QwenLM/Qwen-Image-2.1) — 官方仓库 README、LICENSE、`prompt_rewrite/`, fb7ae1d (2026-09-20), 取于 2026-09-27
- [QwenLM/Qwen-Image](https://github.com/QwenLM/Qwen-Image) — 官方旧仓库 README(2.0 发布记录), 6b5e1f5 (2026-02-10), 取于 2026-09-27
- [wildminder/awesome-qwen-image](https://github.com/wildminder/awesome-qwen-image) — 社区索引(加速 LoRA 三条线), 取于 2026-09-27
- [Qwen/Qwen-Image-2.1 模型卡](https://huggingface.co/Qwen/Qwen-Image-2.1)、[PE-T2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I)、[PE-I2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-I2I) — 官方, 取于 2026-09-27
- [diffusers `pipeline_qwenimage21.py`](https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/qwenimage21/pipeline_qwenimage21.py) 与 `docs/.../qwenimage21.md` — @6256aa7 (2026-09-18), 取于 2026-09-27
- [ComfyUI 文档: Qwen-Image-2.1](https://docs.comfy.org/tutorials/image/qwen/qwen-image-2-1) — Comfy 官方, 9d13379 (2026-09-22), 取于 2026-09-27
- [Comfy-Org/workflow_templates](https://github.com/Comfy-Org/workflow_templates) — 三个 `image_qwen_image_2_1_*.json`, 取于 2026-09-27
- [ComfyUI Blog: Qwen-Image-2.1 in ComfyUI](https://blog.comfy.org/p/qwen-image-21-in-comfyui-open-weight) — Comfy 官方, 取于 2026-09-27
- [Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo) — 社区(Viggle), 取于 2026-09-27
- [机器之心: 拿下开源生图第一…](https://m.36kr.com/p/3991785951198217)、[七牛云解读](https://news.qiniu.com/archives/1789956243584)、[apidog](https://apidog.com/blog/what-is-qwen-image-2-1) — 第三方(转述官方博客与跑分), 取于 2026-09-27
- [comfyui-wiki 新闻](https://comfyui-wiki.com/en/news/2026-09-21-qwen-image-2-1) — 社区, 取于 2026-09-27
- [dseditor/qwen-image-21-prompting](https://github.com/dseditor/qwen-image-21-prompting) `references/community-findings.md`、`rgba-and-sprites.md` — 社区实测, MIT, 取于 2026-09-27
