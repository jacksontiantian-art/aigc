---
name: model-notes
description: 调研一个 AI 生成模型(图像/视频/音频/文本)并写入或更新 models/<slug>.md。用户问「XX 模型怎么样/怎么用」、想整理某个新模型、或 /model-notes 时使用。
---

# 模型笔记

1. **查重**: 先看 `models/` 有没有这个模型。有就在原文件上更新, 改 `updated` 日期。
2. **调研**: 用 Tavily(`tavily_search` / `tavily_extract`)查, 优先级: 官方文档/博客 > 模型卡 > 第三方评测 > 社区帖。重点抓: 能力上限、调用方式与 model id、提示词写法、价格、许可限制、已知坑。
3. **写入**: 按 `models/_template.md` 结构。
   - 每条事实都能在「来源」里找到出处, 带取用日期。
   - 查不到或说法冲突的, 标 `待核实`, 不要猜。
   - 区分官方与社区(skill、工作流、第三方托管版本都注明来源)。
4. **联动**: 如果本项目 `.claude/skills/` 里有对应 skill, 在「怎么调用」里写明; 调研中遇到值得留的提示词写法, 提示用户用 `save-prompt` 收录。
