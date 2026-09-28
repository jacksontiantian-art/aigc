---
name: krea-2-prompt-writing
description: 按 Krea 2 官方提示词指南和官方扩写 system prompt(expansion.txt), 把需求写成 Krea 2 文生图提示词, 并给出画幅、风格 LoRA 与本地 ComfyUI(Turbo)设置。用户要用 Krea 2 / K2 / Krea-2 Turbo 出图、扩写或润色 Krea 提示词、挑 Krea 2 风格 LoRA, 或要求「按 Krea 官方方式写提示词」时使用。纯文档, 不调 API、不跑 ComfyUI。
---

# Krea 2 提示词

`references/` 里两份文件是 Krea 官方仓库的**原文**, 未改动, 来自 [krea-ai/krea-2](https://github.com/krea-ai/krea-2) `docs/` @db3984f (2026-07-24), 仓库许可 Apache-2.0(见 `LICENSE`)。本文件是自写的调用说明。

| 文件 | 内容 |
|---|---|
| `references/expansion.txt` | 官方扩写 system prompt。`prompting.md` 原话: 想让 LLM 帮忙写长提示词, 就把它当作「你选的任意 LLM」的 system prompt。ComfyUI 官方模板的 `prompt_enhance` 用的也是这一段, 只是交给本地 Qwen3-VL-4B 执行 |
| `references/prompting.md` | 官方提示词指南和 20 条示例提示词(均用 Turbo 出 2K 图); 图片是相对链接, 看不到 |

Krea 官方另有 skill 仓库 [krea-ai/skills](https://github.com/krea-ai/skills)(MIT), 但它只能走 Krea 托管的 MCP / API, 按算力点计费, 本地 ComfyUI 用不上, 所以没有引入。下文有几条出自它的 `krea-generate/SKILL.md` @ea45cca, 都已注明。

## 1. 先判断用户处于哪个阶段

| 阶段 | 做法 | 依据 |
|---|---|---|
| **探索**: 方向还没定, 比如只说「一只骑自行车的猫」 | 不要扩写成长段。给 2–3 条短提示词, 每条是「主体 + 一个不同的风格方向」, 让用户每条连出几张比较 | 官方博客 *Krea 2 Prompting Guide*: 先写模糊的提示词让模型铺开多种风格, 再加风格词收窄; 写得越细, 出图多样性越低 |
| **定稿**: 方向已定, 要出成品 | 读 `references/expansion.txt` **全文**, 按其规则扩写成一段 | 官方 `prompting.md`: 长而详细的提示词效果最好。技术报告: 训练以长描述为主, 同时也覆盖短、中长度 |
| **润色**: 用户给的提示词已经很详细 | 只轻度润色, 保留用户的措辞和方向 | `expansion.txt` 第 7 条 |

## 2. 扩写规则速查(以 `expansion.txt` 原文为准)

- **忠实第一**: 保留原有的主体、动作、颜色和空间关系。用户没有明显暗示的物件、道具、人物、动物一律不加。
- **不过度指定**: 输入里没有依据的服装、颜色、材质、场景细节不要编造。
- **结构要好解析**: 每个主体的属性和动作写在一起; 姿势、互动、空间布局用具体的说法。
- **风格规划放在心里**: 先想好主体和情绪, 再比较 2–3 种风格 / 媒介 / 光线方案并选一种, 然后决定构图和取景。输出里不带思考过程, 也不带任何标签。
- **用户指定了媒介就照用**: 比如 "photo of"、"illustration of"、"3D render of"。不能因为难画就换成别的媒介。
- **要出现在画面里的文字**: 把原文精确写出来, 用引号包住。
- **格式**: 一段连贯的文字, 不用列表、JSON 或 markdown。
- **人物**: 描绘人物要有尊严, 默认衣着遮住隐私部位。

规则之外的官方建议:

- **按编码器的预期写**: 官方文本编码器代码里固定的系统提示是 "Describe the image by detailing the color, shape, size, texture, quantity, text, spatial relationships of the objects and background"(`encoder.py`)。颜色、形状、大小、质感、数量、文字、空间关系这几项写到了, 就和编码器的预期对上了。
- **别超过 512 token**: 官方代码里文本编码 `max_length=512` 并截断(`encoder.py`), 英文大约 350 词。官方示例最长也只有 180 词。
- **风格、媒介、配色词往前放**, 例如 `risograph print, limited palette, bold silhouette`(Krea 2 Turbo 用户指南)。
- **两种写法都行**: 官方示例既有逗号分隔的短语串(如 3D 潮玩那条), 也有完整句子写成的段落(如微距老鼠那条), 长度 10–180 词, 多数在 50–120 词。短语串更适合风格化、平面感强的画面; 需要交代空间关系时用句子。这是从 `prompting.md` 的示例归纳出来的, 官方没有明说。
- **文字只写几个词**: 官方 skill 说 Krea 2 的文字渲染能力一般, 大约只能写几个词。长文案、海报正文换别的模型。
- **没有负面提示词**: Turbo 是蒸馏模型, 不开 CFG(ComfyUI 模板 cfg 1, 官方脚本 cfg 0), 负面提示词不起作用。不想要的东西, 在正向提示词里写成你想要的样子。
- **写实要有预期**: 官方 skill 对自家模型的评价是「所有 Krea 2 模型都有偏软的 AI 感: 对比度高、饱和、纹理偏软」, 而 Turbo 细节最弱。Krea 2 的强项是表现性插画、平面设计、风格化画面。需要锐利的写实细节、商品纹理或长文字时, 提醒用户换别的模型。
- **语言**: 官方示例和扩写 system prompt 全是英文, 因此**提示词一律写英文**, 只有画面里要出现的文字保留原文。文本编码器 Qwen3-VL 多语言能力较强(技术报告), 但官方没说中文提示词效果如何, 模型卡只提到提示词的语言会影响遵循度, 这一点待核实。用户坚持用中文时照写, 并建议同一个 seed 和英文版对比一次。

## 3. 风格 LoRA(本地, Krea 官方出品)

Comfy 官方教程列出的 9 个 LoRA, 文件在 [Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2) 的 `loras/` 下。**触发词要写进提示词**, 推荐强度 1.0(模板默认是 0.8)。在官方模板里用 `CustomCombo` 选 LoRA, 再打开 `enable_lora?`。

| LoRA | 触发词 |
|---|---|
| `krea2_darkbrush` | `monochrome ink wash style` |
| `krea2_dotmatrix` | `monochrome stippling style` |
| `krea2_kidsdrawing` | `naive expressive sketch style` |
| `krea2_neondrip` | `textured abstract style` |
| `krea2_rainywindow` | `rainy window style` |
| `krea2_retroanime` | `purple retro anime style` |
| `krea2_softwatercolor` | `art deco watercolor style` |
| `krea2_sunsetblur` | `ethereal motion blur style` |
| `krea2_vintagetarot` | `vintage tarot style` |

用了 LoRA 时, 提示词只写内容(主体、构图、光线), 画风交给 LoRA, 别再写和它冲突的媒介词。官方 skill 的分工原则也是这样: 提示词管画面里有什么, 风格参考、moodboard 和 LoRA 管审美。

风格参考: 本地模板 `image_krea2_turbo_int8_image_style_reference` 接 1–2 张参考图, 它用的 `krea2_style_reference` LoRA 由 **ostris(社区)** 训练, 与 Krea 托管的风格迁移 / moodboard 不是同一套东西。官方 skill 对托管版的建议可以借鉴: 只有要精确复刻某种风格时才用参考图, 一次只放一张; 提示词本身也要写成参考图的风格。

## 4. 交付格式

````
**阶段**: 探索(N 条) / 定稿 / 润色
**画幅**: 16:9 → 1376×768 (要 2K 就在 ResolutionSelector 调 megapixels)
**LoRA**: 无 / krea2_xxx(触发词已写进提示词)
**提示词**:
```text
(英文单段)
```
**说明**: 一两句取舍, 比如选了哪种风格方向、补了什么、没加什么
````

探索阶段每条提示词各给一个代码块, 并用一行说明每条的风格方向。

### 画幅 → 像素

官方画幅在 1K 下的尺寸如下(Krea 2 Turbo 用户指南):

| 画幅 | 1K 尺寸 | | 画幅 | 1K 尺寸 |
|---|---|---|---|---|
| 1:1 | 1024×1024 | | 4:5 | 928×1152 |
| 4:3 | 1184×896 | | 2:3 | 832×1248 |
| 3:2 | 1248×832 | | 9:16 | 768×1376 |
| 16:9 | 1376×768 | | 2.35:1 | 1568×672 |

本地 Turbo 能出 1K–2K(官方 README)。README 的示例直接用 2048×2048; Comfy 教程说在 `ResolutionSelector` 里把 megapixels 设成 2.0 就是「2K」。两种说法口径不一, 按显存和需要自选。宽高会被补齐到 16 的倍数(README)。Raw 模型最高只到 1K。

## 5. 本地 ComfyUI 怎么对应

- **模板**: `image_krea2_turbo_t2i`(fp8)、`image_krea2_turbo_t2i_int8`、`image_krea2_turbo_int8_image_style_reference`。Comfy 官方做的, 随 ComfyUI 安装, 需要 ComfyUI ≥ 0.26.0; 默认参数见 `models/krea-2.md`。
- **关掉 `prompt_enhance`**: 模板默认开着, 会让本地 4B 模型按同一份 `expansion.txt` 再扩写一遍。用本 skill 写好的提示词要**关掉它**, 否则会被改写。
- **采样**: 8 步、cfg 1、euler / simple(模板默认)。官方脚本用 Turbo 时是 8 步、cfg 0、`mu` 1.15。
- **Raw** 用来训练 LoRA(官方原话「在 Raw 上训练, 在 Turbo 上运行」), 不建议直接出图。要用的话, 官方 README 推荐 52 步、CFG 3.5, 这时负面提示词才有效。
- 其余参数、许可和踩坑见 `models/krea-2.md`。

## 6. 收尾

提示词跑通后, 提醒用户用 `save-prompt` 收录(`models: [krea-2]`), 把 LoRA、分辨率、seed 一起记下来。
