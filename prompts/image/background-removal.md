---
title: 去背景 / 抽出主体为透明 PNG
modality: image
models: [qwen-image-2-1]
tags: [rgba, background-removal, cutout, edit, official-comfy]
status: draft
source: https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_qwen_image_2_1_background_removal.json (371a7b7)
created: 2026-09-27
---

## 用途

输入一张普通照片, 让 Qwen-Image-2.1 去掉背景, 输出带 alpha 通道的 PNG。官方 README 把「从照片抽出主体」列为原生透明能力之一; 这条提示词来自 ComfyUI 官方的去背景模板。

## 提示词

```text
Remove the background, and output a PNG image
```

## 变量

无。

## 参数

ComfyUI 模板设置: 25 步、cfg 1、euler / simple, `resolution` 为 0(保持输入图尺寸, 只取整到 32 的倍数), `custom_size` 关闭。这个模板没有接 PE 改写, 这一句就是直接送进模型的原文。

## 效果

未实跑。

## 踩坑

- 只想抽出画面里某一个物体时, 要在指令里点名那个物体(社区教程演示过只抽出剑, 未复核)
