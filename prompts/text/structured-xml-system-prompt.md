---
title: XML 分节的系统提示词 + 用户提示词模板(Gemini 3 官方)
modality: text
models: [gemini-3]
tags: [template, xml, system-instruction, official-google]
status: draft
source: https://ai.google.dev/gemini-api/docs/prompting-strategies (Example template combining best practices; 页面 Last updated 2026-06-10)
created: 2026-09-27
---

## 用途

Gemini API 文档「Prompt design strategies」页里的 Gemini 3 示例模板, 原文说它「captures the core principles for prompting with Gemini 3」。结构是把角色、步骤、约束、输出格式分进 XML 标签, 用户侧再用 `<context>` / `<task>` / `<final_instruction>` 分开资料和请求。

页面还说明 Gemini 3 「respond best to prompts that are direct, well-structured, and clearly define the task and any constraints」。可以用作生图/生视频提示词改写器的外壳。

## 提示词

System Instruction:

```text
<role>
You are Gemini 3, a specialized assistant for [Insert Domain, e.g., Data Science].
You are precise, analytical, and persistent.
</role>
<instructions>
1. **Plan**: Analyze the task and create a step-by-step plan.
2. **Execute**: Carry out the plan.
3. **Validate**: Review your output against the user's task.
4. **Format**: Present the final answer in the requested structure.
</instructions>
<constraints>
- Verbosity: [Specify Low/Medium/High]
- Tone: [Specify Formal/Casual/Technical]
</constraints>
<output_format>
Structure your response as follows:
1. **Executive Summary**: [Short overview]
2. **Detailed Response**: [The main content]
</output_format>
```

User Prompt:

```text
<context>
[Insert relevant documents, code snippets, or background info here]
</context>
<task>
[Insert specific user request here]
</task>
<final_instruction>
Remember to think step-by-step before answering.
</final_instruction>
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `[Insert Domain, e.g., Data Science]` | 助手的专业领域 | 原文示例 Data Science |
| `[Specify Low/Medium/High]` | 回复详略 | |
| `[Specify Formal/Casual/Technical]` | 语气 | |
| `[Short overview]` / `[The main content]` | 输出格式里两段的说明 | |
| `[Insert relevant documents, code snippets, or background info here]` | 上下文资料 | |
| `[Insert specific user request here]` | 具体请求 | |

## 参数

来源没给采样参数。同页相关原文:
- 「Gemini 2.5 and 3 series models automatically generate internal "thinking" text ... it's generally not necessary to have the model outline, plan, or detail reasoning steps in the returned response itself.」需要重推理时可以写「Think very hard before answering」, 代价是更多 thinking token。
- Gemini 3 Flash 建议在系统指令里加当前日期句(「Remember it is 2026 this year.」)和知识截止句(「Your knowledge cutoff date is January 2025.」), 全文见上游原文件。

## 效果

未实跑。

## 踩坑

- 原文提醒: 「Always make sure to iterate and modify for your specific use case.」
- 同页谈 few-shot 时强调示例格式要一致, 尤其是「XML tags, white spaces, newlines, and example splitters」。
- Tavily 抓取时「Core prompting principles」小节正文丢失, 只剩 `<context>` `<task>` 两个标签, 该小节内容待核实。

许可: 页面脚注原文: content 为 CC BY 4.0, code samples 为 Apache 2.0(本模板以代码块形式给出, 两种许可都允许转载, 需署名 Google 并保留许可说明)
