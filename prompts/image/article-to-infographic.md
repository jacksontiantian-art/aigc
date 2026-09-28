---
title: 把一篇文章做成卡通风信息图(英文版面)
modality: image
models: [nano-banana]
tags: [text-to-image, infographic, summarization, cartoon, community, reprint]
status: draft
source: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/README.md (例 14 文章信息图, 第 1770 行); 原作者 微博 @黄建同学 https://weibo.com/5648162302/5204549851155423
created: 2026-09-27
---

## 用途

在提示词后面贴一段博客或文章, 让模型提炼关键信息, 生成一张带可爱卡通人物和元素的信息图, 图上文字用英文。

「信息图 / 图解」是所有高 star 合集里都有的大类: freestylefly 有「图表与信息可视化」53 个案例, ZeroLu 的 Nano Banana Pro 合集有 3.1「Concept Visualization (Text to Infographic)」, YouMind 两个合集都有「Infographic / Edu Visual」分类。这一条是其中最通用、不限定主题的写法。

## 提示词

```text
为文章内容生成信息图
要求：
1. 将内容翻译成英文，并提炼文章的关键信息
2. 图中内容保持精简，只保留大标题
3. 图中文字采用英文
4. 加上丰富可爱的卡通人物和元素
```

## 变量

原文没有占位符。文章正文由用户附在提示词后面(合集说明: 需上传一段博客/文章)。

| 变量 | 说明 | 示例 |
|---|---|---|
| (文章正文) | 要做成信息图的博客或文章内容 | 一篇中文博客 |

## 参数

来源没有给出分辨率、比例等参数。

## 效果

未实跑。合集示例图: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case14/output.jpg

## 踩坑

- 原帖在微博上, 没有抓取核对, 原文待核实。作者用中文发帖, 所以按 PicoTrex 中文版收录; 英文 README 里的 "Generate an infographic for the article content..." 应该是译文。
- 来源里没有写其他踩坑。

许可: PicoTrex 仓库的 LICENSE 文件是 Apache-2.0, README 徽章却写 CC BY 4.0, 两者不一致, 待核实。提示词本身的作者是微博用户 @黄建同学, 合集的许可不一定能覆盖原帖内容。
