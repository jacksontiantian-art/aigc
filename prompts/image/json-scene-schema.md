---
title: JSON 结构化场景提示词骨架
modality: image
models: [flux-2]
tags: [json, structured-prompt, template, t2i, official-bfl]
status: draft
source: https://github.com/black-forest-labs/skills/blob/8907d515b0ac270a988ec7a239add81ee13d6cba/skills/flux-image-best-practices/rules/json-structured-prompting.md (Basic Structure)
created: 2026-09-27
---

## 用途

多主体、要精确摆位、要批量替换字段的场景, 用 JSON 把场景、主体、风格、技术参数、配色分开写。来源(BFL 官方 agent skill)列的适用场景: 多角色且属性各不相同、精确空间位置、复杂构图、可复现的模板化提示词、程序化生成、带变量替换的生产流程。OpenAI 官方指南也写到 JSON-like 结构可用, 所以骨架本身可以跨模型试。

## 提示词

```text
{
  "scene": {
    "setting": "description of environment",
    "time": "time of day/period",
    "mood": "atmospheric quality"
  },
  "subjects": [
    {
      "type": "person/object/animal",
      "description": "detailed description",
      "position": "location in frame",
      "action": "what they're doing"
    }
  ],
  "style": {
    "medium": "photography/painting/illustration",
    "technique": "specific style details",
    "reference": "artist or style reference"
  },
  "technical": {
    "camera": "camera and lens",
    "lighting": "lighting setup",
    "composition": "framing details"
  },
  "colors": ["#hex1", "#hex2"]
}
```

## 变量

JSON 里的字段值是占位说明, 原样保留。

| 字段 | 占位说明(原文) |
|---|---|
| `scene.setting` / `time` / `mood` | `description of environment` / `time of day/period` / `atmospheric quality` |
| `subjects[].type` / `description` / `position` / `action` | `person/object/animal` / `detailed description` / `location in frame` / `what they're doing` |
| `style.medium` / `technique` / `reference` | `photography/painting/illustration` / `specific style details` / `artist or style reference` |
| `technical.camera` / `lighting` / `composition` | `camera and lens` / `lighting setup` / `framing details` |
| `colors` | `#hex1`, `#hex2` |

## 参数

- 同文件 Best Practices: 主体之间有互动时给主体加 `id`; scene / subjects / style / technical 分开写; 术语前后一致; 细节写全; 模板做版本管理。
- BFL 文档(docs.bfl.ai `guides/usecases_t2i_json_prompting`)称 FLUX.2 对 JSON 和自然语言理解得一样好; 适合 JSON 的是生产流程、自动化、多主体复杂场景、需要单独迭代某个元素; 快速探索和单主体简单场景用自然语言更好。
- BFL 文档称 FLUX.2 提示词上限 32K tokens(`guides/prompting_unified_building`)。

## 效果

未实跑。

## 踩坑

- 同一厂商两处说法不一致: skill 文件 Best Practices 第 5 条是 "Flatten for Execution - Convert to natural language before sending to model"; docs.bfl.ai 的 JSON 页却说可以把 JSON 直接放进提示词, 也可以改写成自然语言, "FLUX.2 understands both formats"。待实测。
- 两处的骨架也不同: docs.bfl.ai 的 Base Schema 是扁平的(`scene` / `subjects` / `style` / `color_palette` / `lighting` / `mood` / `background` / `composition` / `camera`), skill 文件是嵌套的(本条)。docs 页没有开放许可, 所以这里收录 MIT 的 skill 版本。
- hex 色值要和具体物体绑定(docs.bfl.ai `guides/prompting_guide_flux2`: "The car is #FF0000" 比 "use red #FF0000 in the image" 效果好)。

许可: black-forest-labs/skills 仓库 MIT(Copyright (c) 2026 Black Forest Labs Inc.), 转载需保留版权与许可声明。docs.bfl.ai 页面未见开放许可, 只作参考链接。
