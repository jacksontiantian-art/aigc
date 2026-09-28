---
title: 透明背景素材(RGBA)官方句式
modality: image
models: [qwen-image-2-1]
tags: [rgba, transparent, sticker, asset, official]
status: draft
source: https://github.com/QwenLM/Qwen-Image-2.1 (README「Transparent Image Generation (RGBA)」, @fb7ae1d)
created: 2026-09-27
---

## 用途

用 Qwen-Image-2.1 从文本直接生成带 alpha 通道的透明 PNG, 适合贴纸、图标、商品图、角色立绘等要叠到别处的素材, 不需要再抠图。

## 提示词

官方推荐格式:

```text
This is an RGBA image with transparency. <your description>. The image has alpha channel and the background is transparent.
```

官方示例:

```text
This is an RGBA image with transparency. A cute cartoon dragon sticker. The image has alpha channel and the background is transparent.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `<your description>` | 官方原文里的占位符, 原样保留; 填写主体描述 | `A cute cartoon dragon sticker` |

## 参数

官方示例用 `num_inference_steps=40`、2048×2048、seed 42。ComfyUI 模板默认 25 步、cfg 1。输出必须存 PNG。

## 效果

未实跑。

## 踩坑

- ComfyUI 官方模板注释里的句式略有不同: `This is an RGBA format image with transparency. [your description]. The image has an alpha channel and a transparent background.` 两个版本哪个效果更好待实测
- 官方改写规则(PE-T2I)没涉及透明图, 而且要求描述背景。用 skill 扩写时, 中间部分只写主体、不写场景背景, 再套这个句式(自拟做法, 待实测)
- 社区实测(dseditor, 未复核): 如果文字本身就是抠出来的主体, 字形会崩; 文字印在不透明底板上则没问题。空白区 alpha 会残留低值, 交给引擎前做 `alpha < 32 → 0`。检查透明效果看 alpha 分布, 普通输出也可能是 RGBA 模式、只是 alpha 全为 255
