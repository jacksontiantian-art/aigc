---
title: 参考图换装(保留人物, 穿上另一张图的衣服)
modality: image
models: [qwen-image-2-1]
tags: [edit, multi-reference, virtual-try-on, official-comfy]
status: draft
source: https://docs.comfy.org/tutorials/image/qwen/qwen-image-2-1 (「Prompting for image edit」, docs 9d13379)
created: 2026-09-27
---

## 用途

`<image1>` 是人物, `<image2>` 是衣服。把衣服穿到人物身上, 脸、发型、体型、姿势和背景都不变。这是 ComfyUI 官方文档给出的多图编辑示例写法。

## 提示词

```text
Keep the character and pose in <image1> unchanged, put this light blue denim shirt from <image2> on the character, preserve the original facial features, hair, body shape and pose
```

## 变量

原文未改。可复用的部分是 `light blue denim shirt`(衣服描述), 要改成 `{{变量}}` 时再定。

## 参数

ComfyUI 编辑模板: `image_1` = 人物(画布), `image_2` = 衣服; 25 步、cfg 1、euler / simple; `custom_size` 关闭时输出跟随 `image_1`。示例素材是模板自带的 `portrait_model_denim.png` 和 `clothing_light_blue_denim_shirt.png`。

## 效果

未实跑。

## 踩坑

- 官方改写规则说, 多图时必须用 `<imageN>` 标签并说明每张图的角色, 人物身份要指向参考图而不是用文字描述; 这条示例符合这两点
- 本机「图像编辑-加速」工作流挂了 Viggle turbo LoRA; Viggle 自己的模型卡说它在保身份编辑上明显差于原版, 换装要保脸时用原版 25–40 步
