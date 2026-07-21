---
name: implement
description: "基于 spec 或 ticket 集合实现一段工作。"
disable-model-invocation: true
---

实现用户在 spec 或 tickets 中描述的工作。

## 选择验证预算

实现前选择能覆盖风险的最低验证预算。Issue 已提供 `Validation budget` 时采用它；如果实际 code context 显示风险更高，则提高预算。用户或 repo 明确要求的 checks 始终优先。

- **Low** — docs、copy、styles、config、mechanical changes，或不含复杂 logic 的局部改动。不编写、不运行 tests。只有改动可能破坏编译或类型时，才在全部 edits 完成后运行一次最小 typecheck/build。
- **Medium** — isolated runtime behavior、有限 branching、明确且局部的 blast radius。优先复用现有 tests；只有它们无法覆盖核心 behavior 时，才新增或调整最多 1–2 个 test cases。完成连贯 implementation 后，用一个 command 集中运行一次相关 tests。仅在必要且未被该 command 覆盖时，再运行一次 typecheck/build。
- **High** — permissions、money、persistence、migration、concurrency、external contracts、复杂 business logic、高 uncertainty 或广泛 blast radius。只测试 critical seams；集中写完必要 tests 后，用一个 command 批量运行。单个 high-risk issue 可在最后运行一次 full suite；multi-issue effort 只在标有 `Full suite: yes` 的最终 integration issue 运行一次。

无法明确分类的 runtime behavior 使用 Medium；uncertainty 将预算提高一级。

## 集中验证

默认不调用 `/tdd`。只有用户明确要求 test-first，或 regression 需要 tight feedback loop 时才使用它。

- 不要在每个功能点、acceptance criterion、file 或 slice 后运行 checks。
- 一个 umbrella command 已包含 typecheck 和 tests 时，不要分别重复运行。
- Check 失败后，修复并只重跑失败的最小目标；不要重新执行整套 validation stack。
- 没有 `Full suite: yes` 的 multi-issue ticket 不运行 full suite，除非用户或 repo 明确要求。
- 后续人工 review 和 manual test 是主要验收步骤；自动化验证只覆盖预算定义的高价值风险，不要为了 coverage 扩大 tests。

完成后，使用 `/code-review` 审查这次工作。

最终报告 selected validation budget、增改的 tests、执行的 commands，以及主动跳过的 checks 和原因。

把工作提交到当前 branch。
