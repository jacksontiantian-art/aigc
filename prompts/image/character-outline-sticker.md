---
title: 角色转白边贴纸(按参考贴纸风格 + 俏皮短语)
modality: image
models: [nano-banana]
tags: [edit, multi-reference, sticker, illustration, community, reprint]
status: draft
source: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/README_en.md (Case 10, 第 1004 行); 原作者 X @op7418(歸藏) https://x.com/op7418/status/1960385812132192509
created: 2026-09-27
---

## 用途

两张图输入: 图 1 是人物, 图 2 是贴纸风格参考。把人物改成网页插画风的白色描边贴纸, 再配一句描述图 1 的俏皮短语(同样带白边)。原帖说是复刻「Fenx 做的 AI 梗图贴纸」。

「贴纸 / 表情包」在多个合集里反复出现: PicoTrex 还有 Case 77(表情包, @vista8)和 Example 108(LINE 贴图); EvoLink 有 Die-Cut Sticker、Sticker Grid 等; YouMind 的 GPT Image 2 合集有 Doodle Sticker。

## 提示词

```text
Help me turn the character into a white outline sticker similar to Figure 2. The character needs to be transformed into a web illustration style, and add a playful white outline short phrase describing Figure 1.
```

## 变量

原文没有可填的占位符, 只用 Figure 1 / Figure 2 指代两张输入图。

| 变量 | 说明 | 示例 |
|---|---|---|
| `Figure 1` | 人物 / 角色图, 短语就是描述这张图的 | 一张人物照片 |
| `Figure 2` | 贴纸风格参考图 | 一张白边贴纸 |

## 参数

合集说明: "Need to upload a sticker reference image and a character image"。来源没有给出其他参数。

## 效果

未实跑。合集示例图:
- 输入: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case10/input.jpg
- 输出: https://github.com/PicoTrex/Awesome-Nano-Banana-images/blob/2558bf0bb825be150c5d1aeab918cd90004882d3/images/case10/output.jpg

## 踩坑

来源里没有写踩坑。已对照原帖: 原文是英文, 和 PicoTrex 英文版一致; PicoTrex 中文 README 里的是译文。

许可: PicoTrex 仓库的 LICENSE 文件是 Apache-2.0, README 徽章却写 CC BY 4.0, 两者不一致, 待核实。提示词本身的作者是 X 用户 @op7418, 合集的许可不一定能覆盖原帖内容。
