---
name: triage-issue
description: 通过探索 codebase 查找 root cause 来 triage bug 或 issue，然后创建带 TDD-based fix plan 的 GitHub issue。Use when user reports a bug, wants to file an issue, mentions "triage", or wants to investigate and plan a fix for a problem.
---

# Triage Issue

调查报告的问题，找到 root cause，并创建带 TDD fix plan 的 GitHub issue。这基本是 hands-off workflow，尽量减少向用户提问。

## Process

### 1. Capture the problem

从用户获取 issue 的简短描述。如果他们尚未提供，只问一个问题：“What's the problem you're seeing?”

暂时不要问 follow-up questions。立即开始调查。

### 2. Explore and diagnose

使用 Agent tool，subagent_type=Explore，深入调查 codebase。目标是找到：

- **Where** bug 在哪里表现出来（entry points、UI、API responses）
- **What** 涉及哪个 code path（trace the flow）
- **Why** 为什么失败（root cause，不只是 symptom）
- **What** 有哪些 related code（similar patterns、tests、adjacent modules）

查看：

- Related source files 及其 dependencies
- Existing tests（已测什么、缺什么）
- 受影响 files 的 recent changes（对相关 files 执行 `git log`）
- code path 中的 error handling
- codebase 其他地方可正常工作的 similar patterns

### 3. Identify the fix approach

基于调查判断：

- 修复 root cause 所需的 minimal change
- 哪些 modules/interfaces 受影响
- 需要通过 tests 验证哪些 behaviors
- 这是 regression、missing feature 还是 design flaw

### 4. Design TDD fix plan

创建一个具体、有序的 RED-GREEN cycles 列表。每个 cycle 是一个 vertical slice：

- **RED**：描述捕获 broken/missing behavior 的 specific test
- **GREEN**：描述让该 test 通过的 minimal code change

规则：

- Tests 通过 public interfaces 验证 behavior，不测试 implementation details
- 一次一个 test，vertical slices（不是先写所有 tests，再写所有 code）
- 每个 test 应经受 internal refactors
- 如有需要，包含最终 refactor step
- **Durability**：只建议能经受 radical codebase changes 的 fixes。描述 behaviors 和 contracts，不描述 internal structure。Tests 断言 observable outcomes（API responses、UI state、user-visible effects），不测 internal state。好建议读起来像 spec，坏建议读起来像 diff。

### 5. Create the GitHub issue

使用 `gh issue create` 按下面模板创建 GitHub issue。不要先要求用户 review；直接创建并分享 URL。

<issue-template>

## Problem

清晰描述 bug 或 issue，包括：
- 发生了什么（actual behavior）
- 应该发生什么（expected behavior）
- 如何复现（if applicable）

## Root Cause Analysis

描述调查发现：
- 涉及的 code path
- 当前 code 为什么失败
- 任何 contributing factors

不要包含会绑定当前 code layout 的具体 file paths、line numbers 或 implementation details。改用 modules、behaviors 和 contracts。issue 应在 major refactors 后仍然有用。

## TDD Fix Plan

RED-GREEN cycles 的编号列表：

1. **RED**: Write a test that [describes expected behavior]
   **GREEN**: [Minimal change to make it pass]

2. **RED**: Write a test that [describes next behavior]
   **GREEN**: [Minimal change to make it pass]

...

**REFACTOR**: [Any cleanup needed after all tests pass]

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] All new tests pass
- [ ] Existing tests still pass

</issue-template>

创建 issue 后，打印 issue URL 和 root cause 的一句话 summary。
