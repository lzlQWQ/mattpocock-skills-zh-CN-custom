---
name: tdd
description: 测试驱动开发。适用于用户明确要求 test-first 或 red-green-refactor，或 regression 需要 tight feedback loop 时。
---

# Test-Driven Development

TDD 是 red -> green loop。这个 skill 是让该 loop 产出值得保留的 tests 的 reference：什么是好 test、tests 应该放在哪里、anti-patterns，以及 loop 的规则。每个 cycle 前和 cycle 中都要参考这些内容，而不是事后才看。

探索 codebase 时，读取 `CONTEXT.md`（如果存在），让 test names 和 interface vocabulary 与项目 domain language 对齐，并尊重你触碰区域的 ADRs。

## What a good test is

Tests 应通过 public interfaces 验证 behavior，而不是 implementation details。代码可以完全改变；tests 不该随之改变。一个好 test 读起来像 specification："user can checkout with valid cart" 能清楚说明存在什么能力；因为它不关心 internal structure，所以能承受 refactors。

示例见 [tests.md](tests.md)，mocking 规则见 [mocking.md](mocking.md)。

## Seams — where tests go

**Seam** 是你测试的 public boundary：可以观察 behavior、但不伸手进入内部的 interface。Tests 放在 seams 上，绝不针对 internals。

**只测试预先认可的 seams。** 写任何 test 前，先写下要测试的 seams 并与用户确认。未经确认的 seam 不写 test。你无法测试所有东西；提前认可 seams，才能把测试精力放在 critical paths 和复杂 logic 上，而不是每个 edge case。

询问："What's the public interface, and which seams should we test?"

## Anti-patterns

- **Implementation-coupled** — mock internal collaborators、测试 private methods，或通过 side channel 验证（例如不用 interface 而直接查询 database）。特征是 refactor 时 test 失败，但 behavior 没变。
- **Tautological** — assertion 以和代码相同的方式重新计算 expected value（`expect(add(a, b)).toBe(a + b)`、手工按同一逻辑生成 snapshot、把 constant 断言等于它自己），因此天然 pass，永远无法与代码 disagree。Expected values 必须来自独立 source of truth：known-good literal、worked example 或 spec。
- **Horizontal slicing** — 先写所有 tests，再写所有 implementation。批量 tests 验证的是 _想象中的_ behavior：你测试的是东西的 _shape_，不是 user-facing behavior；tests 会对真实变化迟钝，并在理解 implementation 前承诺 test structure。改用 **vertical slices**：一个 test -> 一个 implementation -> repeat，每个 test 都是回应上一轮学习的 **tracer bullet**。

## Rules of the loop

- **Meaningful red before green.** Red 必须让 test command 到达 public interface，并因目标 behavior 尚未实现而 assertion failure；然后只写足够让它通过的代码。不要预判未来 tests，也不要添加 speculative features。
- **One slice at a time.** 每个 cycle 只处理一个 seam、一个 test、一个 minimal implementation。
- **Refactoring is not part of the loop.** Refactoring 属于 review stage（见 `code-review` skill），不属于 red -> green implementation cycle。

## Meaningful red

不要为了记录一个必然的 failure 而运行 test。缺失 method、type、import、syntax，或错误的 fixture/config 只说明 test harness 尚未准备好，不是 Red。

- 新 public interface 尚不存在时，先建立可编译的最小 signature 或 placeholder implementation，但不要实现目标 behavior。
- 第一次实际执行必须到达 seam，并因预期 behavior 不满足而失败。
- 如果执行暴露的是 compile、import、fixture 或 environment 问题，先修复 test harness；这些 failure 不计为 Red。
- 已有 failing test 已准确覆盖目标 behavior 时，直接使用它，不新增重复 test。
