---
title: 结构化角色提示词模板(LangGPT: Role / Profile / Goal / Rules / Workflow)
modality: text
models: []
tags: [langgpt, structured-prompt, template, chinese-community]
status: draft
source: https://github.com/langgptai/LangGPT/blob/c8864d748e7cb8437427721eed756f6e578e6c9b/README.md (Method 3: Master the Template; c8864d7)
created: 2026-09-27
---

## 用途

LangGPT 的基础模板: 用 Markdown 标题把角色、目标、技能、规则、流程和初始化语分块写, 模板里可以引用自身的节(如 `<Rules>`、`<Workflow>`)。README 自述它是中文 AI 社区最流行的结构化提示词范式, 由云中江树在 2023 年提出, 有论文 arXiv:2402.16929。

## 提示词

```markdown
# Role: Your_Role_Name

## Profile
- Author: YourName
- Version: 1.0
- Language: English
- Description: Clear role description and core capabilities

## Goal
- Outcome: What concrete result/outcome should be delivered for the user/session
- Done Criteria: Clear acceptance criteria (how we know it’s finished and good)
- Non-Goals: What is explicitly out of scope to avoid scope creep

### Skill-1
1. Specific skill description
2. Expected behavior and output

## Rules
1. Don't break character under any circumstance
2. Don't make up facts or hallucinate

## Workflow
1. Analyze user input and identify intent
2. Apply relevant skills systematically
3. Deliver structured, actionable output

## Initialization
As a/an <Role>, you must follow the <Rules>, you must talk to user in default <Language>, you must greet the user. Then introduce yourself and introduce the <Workflow>.
```

## 变量

| 变量 | 说明 | 示例 |
|---|---|---|
| `Your_Role_Name` | 角色名 | |
| `YourName` / `1.0` / `English` | 作者、版本、语言 | |
| Description / Goal / Skill / Rules / Workflow 各行 | 按任务改写, 原文每行都是示意文字 | |
| `<Role>` `<Rules>` `<Language>` `<Workflow>` | Initialization 里引用本模板对应节的写法, 保持原样 | |

## 参数

来源没给模型参数。README 原文: 「Prerequisites: Basic Markdown knowledge | GPT-4 or Claude recommended」。

README 的 Method 1 还说, 可以不带模板, 直接对主流模型说「Write me a prompt the **LangGPT** way…」这类话来触发。这是项目方的说法, 未验证。

## 效果

未实跑。

## 踩坑

- README 这版比仓库 `LangGPT/templates/` 里的旧模板(Role.md / miniRole.md)多了 `## Goal` 节(Outcome / Done Criteria / Non-Goals)。

许可: Apache-2.0(LICENSE 文件; README 徽章写 MIT, 两者不一致, 暂以 LICENSE 文件为准, 待核实)
