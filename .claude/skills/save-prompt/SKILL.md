---
name: save-prompt
description: 把一段提示词收录进本项目的 prompts/ 库。用户说「收录/存一下/记下这个提示词」、贴出一段跑通的提示词想留档、或 /save-prompt 时使用。
---

# 收录提示词

1. **定模态与文件名**: 按 `image | video | audio | text` 放进 `prompts/<modality>/`, 文件名 kebab-case 描述用途(如 `pet-idle-loop.md`), 不带日期和模型名。
2. **先查重**: `grep -ril` 关键词扫 `prompts/`。已有相近条目就更新原文件(补变体、补踩坑、升 status), 不新建。
3. **按模板写**: 复制 `prompts/_template.md` 的结构。
   - 提示词原文**一字不改**放进 `text` 代码块; 可复用的部分改成 `{{变量}}` 前先问用户。
   - `models` 字段填 `models/` 里的 slug; 没有对应模型笔记时提醒用户可以用 `model-notes` 补。
   - `status`: 用户说跑通过 → `tested`; 没说 → `draft`。
   - 参数、效果、踩坑只写用户给过或对话里出现过的信息, 不编造。
4. 收录完回一行: 文件路径 + status。
