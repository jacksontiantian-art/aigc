---
name: qwen-image-prompt-writing
description: 按 Qwen-Image-2.1 官方提示词改写模型(PE-T2I / PE-I2I)的 system prompt, 把需求写成 Qwen-Image-2.1 的文生图或图像编辑提示词, 并给出画幅与本地 ComfyUI 设置。用户要用 Qwen-Image-2.1 / 千问图像 2.1 出图、改图、换装、多图合成、局部编辑、抠图或生成透明 PNG, 或要求「按 Qwen 官方格式改写提示词」时使用。纯文档, 不调 API、不跑 ComfyUI。
---

# Qwen-Image-2.1 提示词

`references/` 里两份文件是官方提示词改写模型的 system prompt **原文**, 未改动, 来自 [QwenLM/Qwen-Image-2.1](https://github.com/QwenLM/Qwen-Image-2.1) `prompt_rewrite/prompts/` @fb7ae1d (2026-09-20)。许可是 Qwen Research License(**仅限非商用**), 见同目录 `LICENSE` / `NOTICE`。本文件是自写的调用说明。

官方建议出图前先用改写模型扩写短提示词(README「Prompt Rewriting」)。改写模型是两个 Qwen3.5-VL 9B 微调版。本 skill 让 Claude 读同一份 system prompt 来当改写器, 输出同一套 JSON 契约, 这样本地显卡不用再额外加载 9B 模型。但 Claude 毕竟不是官方微调过的那个模型, 出图效果要实测对比, 不能当成等价。

## 1. 判任务

| 情况 | 任务 | 读哪份 |
|---|---|---|
| 没有输入图 | T2I 文生图 | `references/system_prompt_t2i.txt` |
| 有 1 张或多张输入图(改图、换装、合照、局部编辑、抠图、扩图、三视图…) | Edit | `references/system_prompt_edit.txt` |

读对应文件**全文**, 按里面的步骤逐条执行, 不要凭记忆概括。Edit 必须先看图: 用户给了路径就用 Read 打开; 看不到图就先问清楚哪些东西必须保持不变, 不要编造图里的内容。

## 2. 最容易写错的地方(速查, 以原文为准)

**T2I**
- 描述**永远是英文**; 画面里要出现的文字保留原文字种, 用直双引号包起来。
- 以旁观者口吻描述成品画面(现在时、第三人称), 不写 "create" "make sure" 这类指令, 不写 8K、masterpiece 之类的质量词。
- 写成一段, 约 20 句、400–500 词; 需求再短也要写到这个量, 没说的部分由你补全。
- 开头一句点明媒介、风格、主体、背景; 用 8–14 个方位短语覆盖画面四角和中心; 光线单独写一句; 最后只写一句总括构图、配色和情绪。
- 画面里没有要读的文字时, 不要自己编招牌、标语。
- 比例只放在 `wh_ratio`, 正文不写比例、分辨率、像素。用户没说时, 横图用 `3:2`, 竖图用 `2:3`。

**Edit**
- 语言分两层: 引号外的描述跟随用户指令的语言(中文→中文, 英文→英文, 其他语言→英文); 引号内要画进图里的文字按「用户指定 > 图中已有文字的主语言 > 指令语言」决定, 不混双语。
- 核心原则是**只改用户点名的属性, 而且改到位; 其余保持原样**。保持的部分用一句笼统的保持条款带过, 不要重新描述一遍, 描述越具体越容易被重画。
- 以操作开头, 写成指令; 一段, 不换行; 用肯定句(「保持背景与输入图完全一致」), 不写禁止句。
- 输入 2 张及以上时, 必须用 `<image1>` `<image2>`…, 并说明每张图的角色(哪张是画布、从哪张取什么); 只有 1 张图时不用标签。
- 人物身份要指向参考图, 不要用文字描述五官。
- `wh_ratio` 与 `ratio_follow` 只填一个: 普通单图编辑跟随 `<image1>`; 合照、写真、扩图、全景、三视图等新构图按原文里的表选比例。

## 3. 改写规则没覆盖的场景

以下内容不在两份 system prompt 里, 每条都标了出处:

- **透明图(RGBA)**: 官方 README 推荐的句式如下, 首尾包住描述:
  `This is an RGBA image with transparency. <your description>. The image has alpha channel and the background is transparent.`
  T2I 改写规则没提透明图, 而且要求写背景, 两者怎么组合官方没说明。自拟做法是: 中间只写主体、不写场景背景, 再套上这个句式。**未实测**, 跑过后把结果记进 `prompts/image/rgba-transparent-asset.md`。
  从照片抠出主体可参考 ComfyUI 官方去背景模板的提示词(`prompts/image/background-removal.md`)。输出必须存 PNG。
- **局部编辑**: 官方支持圈选、涂抹、独立蒙版三种方式(README)。据第三方对官方博客的转述: 可以用不同颜色圈出多个区域, 在提示词里按颜色分别指代; 圈选和涂抹会遮住原图, 需要保留原图信息时, 把原图和黑白蒙版作为两张输入图一起给。官方没给固定句式, 按 Edit 规则写, 用颜色或 `<image2>` 指明区域。
- **多参考图**: 最多 10 张(README)。输入顺序就是模型阅读的顺序(Diffusers 文档); 调换顺序, 所有 `<imageN>` 的指代都会跟着错位(官方 `prompt_rewrite/README.md`)。

## 4. 交付格式

默认给出可以直接粘进 ComfyUI 的结果:

````
**任务**: T2I / Edit(N 张图)
**画幅**: 3:2 → 2528×1696 (或: 跟随 <image1>)
**提示词**:
```text
(单段)
```
**说明**: 一两句关键取舍, 比如补了哪些用户没说的内容、保持了什么
````

用户要 JSON 或批处理时, 只输出官方契约的单行 JSON:
- T2I: `{"rewritten_prompt": "...", "wh_ratio": "3:2"}`
- Edit: `{"rewritten_prompt": "...", "wh_ratio": "", "ratio_follow": "<image1>"}`

### 比例 → 像素

官方推荐尺寸(README):

| wh_ratio | 宽×高 |
|---|---|
| 1:1 | 2048×2048 |
| 4:3 / 3:4 | 2400×1792 / 1792×2400 |
| 3:2 / 2:3 | 2528×1696 / 1696×2528 |
| 16:9 / 9:16 | 2752×1536 / 1536×2752 |

改写规则还可能给出表外比例(2:1、21:9、5:7 等)。官方集成示例遇到表外比例会直接回落到 2048×2048。想保留原比例就自己算: 总像素约 2048², 宽和高都取 32 的倍数(管线要求)。

### 本地 ComfyUI 怎么对应

- **T2I**: 在 `ResolutionSelector` 选比例; megapixels 设约 4 是原生 2K, 设 1 约为 1024² 的快速预览。
- **Edit**: `custom_size` 关闭时, 输出跟随 `image_1` 的比例。所以**先把画布图放在 `image_1`, 再按这个顺序写提示词**。规则给出 `wh_ratio`(合照、写真、扩图等)时, 打开 `custom_size`, 用 `ResolutionSelector` 设置画布。
- **采样**: 官方默认 40 步、不开 CFG(Diffusers); ComfyUI 官方模板默认 25 步、cfg 1、euler/simple。cfg=1 时负面提示词不生效, 不想要的东西要在正向提示词里用肯定描述排除。
- **官方 PE 节点**: 官方模板里的 `refine_prompt` 开关就是 PE 改写。用本 skill 改写后要把它**关掉**, 否则会被再改写一遍(编辑模板默认是开的)。本机 ComfyUI 0.37.4 自带的模板包还是旧版, 没有这个开关, 详见 `models/qwen-image-2-1.md`。
- 其余参数和踩坑见 `models/qwen-image-2-1.md`。

## 5. 收尾

提示词跑通后, 提醒用户用 `save-prompt` 收录(`models: [qwen-image-2-1]`), 把实测效果和踩坑一起记下来。
