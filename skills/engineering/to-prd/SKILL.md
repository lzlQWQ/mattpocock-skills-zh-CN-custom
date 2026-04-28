---
name: to-prd
description: 将当前 conversation context 转成 PRD 并提交为 GitHub issue。Use when user wants to create a PRD from the current context.
---

这个 skill 使用当前 conversation context 和 codebase understanding 产出 PRD。**不要**访谈用户，只综合你已经知道的内容。

## Process

1. 如果还没有探索 repo，先探索它以理解 codebase 当前状态。

2. 草拟完成 implementation 需要 build 或 modify 的主要 modules。主动寻找可以抽出 deep modules 并独立测试的机会。

Deep module（相对 shallow module）会把大量功能封装在一个简单、可测试、很少变化的 interface 后面。

与用户确认这些 modules 是否符合预期。与用户确认他们希望为哪些 modules 写 tests。

3. 使用下面模板写 PRD，并提交为 GitHub issue。

<prd-template>

## Problem Statement

用户正在面对的问题，从用户视角描述。

## Solution

问题的解决方案，从用户视角描述。

## User Stories

一份很长的编号 user stories 列表。每条 user story 使用以下格式：

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

这份 user stories 列表应该非常完整，覆盖 feature 的所有方面。

## Implementation Decisions

已作出的 implementation decisions 列表。可以包括：

- 将 build/modify 的 modules
- 将 modify 的 module interfaces
- 来自 developer 的技术澄清
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

不要包含具体 file paths 或 code snippets。它们可能很快过时。

## Testing Decisions

已作出的 testing decisions 列表。包括：

- 什么是好测试的描述（只测试 external behavior，不测试 implementation details）
- 哪些 modules 会被测试
- 测试的 prior art（即 codebase 中类似类型的 tests）

## Out of Scope

本 PRD 范围外事项的描述。

## Further Notes

关于 feature 的其他 notes。

</prd-template>
