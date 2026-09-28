---
title: 名人金句卡(左头像右金句, 中英文均可)
modality: image
models: [nano-banana-pro]
tags: [text-to-image, typography, quote-card, portrait, chinese-text, community, reprint]
status: draft
source: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/README.md (Nano Banana Pro 例 25 金句卡片, 第 694 行); 原作者 X @stark_nico99(Nicolechan) https://x.com/stark_nico99/status/1991718646570426763
created: 2026-09-27
---

## 用途

生成一张横版名人金句卡: 棕色背景, 左边是人物头像(带渐变过渡), 右边是浅金色衬线体金句和署名, 文字占画面 2/3。原帖强调 Nano Banana Pro 人像一致性好、支持中文, 两者结合效果好; 原帖是配合「上传参考图」一起用的。

这一条同时被两个高 star 合集收录: PicoTrex(Nano Banana Pro 例 25), 以及 YouMind 的 Nano Banana Pro 合集精选第 1 条(Featured No.1)。原帖(2025-11-21)抓取时有 12.3 万浏览。

## 提示词

```text
一张宽的名人金句卡，棕色背景，衬线体浅金色 “保持饥饿, 保持愚蠢” 小字“——Steve Jobs”，文字前面带一个大的淡淡的引号，人物头像在左边，文字在右边，文字占画面比例2/3，人物占1/3，人物有点渐变过渡的感觉
```

## 变量

原文没有占位符语法。原帖说明「引号里的文字可替换」。

| 变量 | 说明 | 示例 |
|---|---|---|
| `“保持饥饿, 保持愚蠢”` | 金句正文, 可替换 | 保持饥饿, 保持愚蠢 |
| `“——Steve Jobs”` | 署名, 可替换 | ——Steve Jobs |
| (参考图) | 原帖配合上传人物头像参考图使用 | 名人或自己的头像 |

## 参数

原帖是在 YouMind 里配合上传参考图使用的。来源没有给出比例、分辨率等参数, 只写了「宽的」。

## 效果

未实跑。示例图:
- PicoTrex: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/pro_case25/output.jpg
- YouMind: https://cms-assets.youmind.com/media/1763886933714_5zqn1e_G6QBjQHbgAE3Yt_.jpg 、 https://cms-assets.youmind.com/media/1763886938314_wbcfc7_G6QBiiracAInQ8z.jpg

## 踩坑

- 已对照原帖, PicoTrex 中文版和原帖逐字一致; PicoTrex 英文 README 里的是译文。
- YouMind 合集(`YouMind-OpenLab/awesome-nano-banana-pro-prompts` README 精选 No.1)收录的是改写过的英文版, 把金句和署名换成了 `{argument name="famous_quote" default="Stay Hungry, Stay Foolish"}` 这种 YouMind 自己的参数语法, 不是原文。
- 原帖正文里有「现在在 YouMind 里面使用这段提示词」, 作者和 YouMind 可能有推广关系, 待核实。

许可: PicoTrex 仓库的 LICENSE 文件是 Apache-2.0, README 徽章却写 CC BY 4.0, 两者不一致, 待核实; YouMind 仓库是 CC BY 4.0。提示词本身的作者是 X 用户 @stark_nico99, 合集的许可不一定能覆盖原帖内容。
