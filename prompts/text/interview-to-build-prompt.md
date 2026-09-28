---
title: 先提问再写: 把草稿整理成三段式提示词(OpenAI 官方 meta-prompt)
modality: text
models: []
tags: [meta-prompt, prompt-builder, official-openai]
status: draft
source: https://github.com/openai/openai-cookbook/blob/6dc6324fb9ed780b32b787f23fad336e9f1eff15/examples/chatgpt/chatgpt_prompt_guide/chatgpt_prompt_guide.md (6dc6324, 第 5 节 Use ChatGPT to help you prompt)
created: 2026-09-27
---

## 用途

来自 OpenAI cookbook 的《ChatGPT Enterprise: Practical prompt engineering for everyday work》(registry 日期 2026-04-27, 未归档)。它的做法是先贴出凌乱的草稿或笔记, 让模型先向你提问、复述意图、找出缺口, 再整理成 `# Context` / `# Instructions` / `# Additional Information` 三段式提示词。

指南推荐的布局原文是 `# Context` / `# Instructions` / `# Additional info / Constraints`, 并说明「This is a minimal recommended structure, not a fixed template」。

## 提示词

```text
Help me build a prompt that I can feed to a GenAI language model. I'm going to paste my draft prompt below - you ask me questions, then write up a clean prompt summarizing what we covered. The final prompt should have 3 sections ("# Context", "# Instructions", and "# Additional Information").
```

## 变量

原文没有占位符。草稿提示词直接贴在这段话后面(原文: 「I'm going to paste my draft prompt below」)。

## 参数

来源没给模型参数。指南第 4 节列了 Additional Information 里值得写的项(原文): Goal、Audience / tone、Format & length、Coverage、Constraints、Acceptance criteria。

指南配套一个 ChatGPT Skill「Prompting Best Practices Skill」, 下载地址 `https://cdn.openai.com/cookbook/chatgpt-prompt-guide/prompt-redrafter.zip`(未下载核对)。

## 效果

未实跑。

## 踩坑

- 指南列出不适合 meta-prompt 的情况(原文): 任务本来就简单、可以直接写; 缺的是 **source material** 而不是更漂亮的提示词; 想靠提示词绕开没想清楚的需求(「scoping still comes first」)。
- 事实性或高风险输出, 指南建议: 要求给出处或直接引用材料; 用 「(needs verification)」 标出不确定处; 结尾附检查清单; 要求列出所做的假设。指南给的复核提示词原文:

```text
Check the output for:
- Accuracy: Does it avoid making up facts?
- Completeness: Did it include the required points?
- Format: Does it match the requested structure?
- Tone: Is it appropriate for the audience?
- Assumptions: Are any guesses clearly labeled?
```

许可: MIT(openai/openai-cookbook LICENSE, Copyright (c) 2025 OpenAI)
