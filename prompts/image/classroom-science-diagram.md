---
title: 课堂科普示意图(信息图, 带标注与箭头)
modality: image
models: [gpt-image-2, gpt-image-2-5-flare, gpt-image-2-5-sunburst]
tags: [infographic, diagram, education, text-rendering, official-openai]
status: draft
source: https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/examples/multimodal/image-gen-models-prompting-guide.ipynb (4.9 Scientific / Educational Visuals); 同文见 https://developers.openai.com/api/docs/guides/image-prompting (Create scientific and educational visuals)
created: 2026-09-27
---

## 用途

生成讲解型示意图: 生物、化学、课堂讲义、流程图。来源建议把提示词写成「教学设计需求」: 写清受众、要讲的知识点、版式、必须出现的标注和科学约束。这条是官方完整示例, 适合改成其他学科的讲解图。

## 提示词

```text
Create a simple biology diagram titled "Cellular Respiration at a Glance" for high school students.

Show how glucose turns into energy inside a cell. Include glycolysis, the Krebs cycle, and the electron transport chain.
Use arrows to connect the steps, and label the main molecules: glucose, pyruvate, ATP, NADH, FADH2, CO2, O2, and H2O.
Make it look like a clean classroom handout or slide, with a white background, simple icons, clear labels, and easy-to-read text.

Avoid tiny text, extra decoration, or anything that makes the diagram hard to understand.
```

## 变量

原文是完整示例, 没有占位符。复用时通常要换的是标题、受众、要包含的步骤和需要标注的名词(这是整理者的建议, 来源没有这么写)。

## 参数

- cookbook: `model="gpt-image-2"`, `size="1536x1024"`, `quality="high"`, 用 `client.images.generate`。
- API 文档 GPT Image 2.5 指南用同一段提示词, 设置同样是 `size="1536x1024"`, `quality="high"`, 分别在 GPT Image 2.5 Flare / Sunburst 上出了示例。
- 来源原话: 密集标注、示意图、要放进幻灯片或课程材料的素材用 `quality="high"`。

## 效果

未实跑。cookbook 示例图: https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/images/scientific_educational_cellular_respiration_gpt-image-2.png

## 踩坑

- 来源: 追求准确时要逐项列出必须包含的组成部分, 并说明不要出现什么(这条示例里点名了 3 个阶段和 8 个分子, 最后一句排除小字和多余装饰)。
- 来源: 要一套干净扁平的视觉系统, 图标风格统一、箭头清楚、标注可读, 留足白。
- API 文档在讲流程信息图时补充: 除了外观, 还要核对标注和事实关系。

许可: openai-cookbook 仓库 MIT(Copyright (c) 2025 OpenAI), 转载需保留版权与许可声明。同一提示词也出现在 OpenAI API 文档 GPT Image 2.5 prompting guide, 该页面未见开放许可, 以 cookbook 版本为准。
