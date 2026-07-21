Quickstart:

```bash
npx skills add vinvcn/mattpocock-skills-zh-CN --skill=implement
```

```bash
npx skills update implement
```

[Source](https://github.com/vinvcn/mattpocock-skills-zh-CN/tree/main/skills/engineering/implement)

## What it does

`implement` 把 spec 或 issue 集合描述的工作落成代码：先按风险选择 low、medium 或 high validation budget，再集中执行最低必要 checks，最后交给 review 并提交到当前 branch。

它不决定要构建什么。Spec 和 seams 应该已经确定；`implement` 执行计划，而不是重新打开计划。

## When to reach for it

你通过输入 `/implement` 调用它；agent 不会自行触发。

当工作已经写成 spec 或拆成 issues，并准备转成代码时使用它。如果 spec 还不存在，用 [to-spec](https://aihero.dev/skills-to-spec)；只想 test-first 写一个具体 behavior，用 [tdd](https://aihero.dev/skills-tdd)。

## Risk-budgeted validation

Low budget 不编写或运行 tests；只有存在编译风险时才在结束前运行一次最小检查。Medium budget 优先复用现有 tests，最多新增或调整一到两个核心 test cases，并在 implementation 完成后集中运行一次。High budget 只覆盖 critical seams，并批量执行必要 tests；完整测试套件只在单个 high-risk issue 或标记为最终 verification point 的 integration issue 运行一次。后续人工 review 和 manual test 是主要验收步骤，自动化验证只覆盖高价值风险。

`implement` 默认不调用 [tdd](https://aihero.dev/skills-tdd)。只有用户明确要求 test-first，或 regression 需要 tight feedback loop 时才使用它。用户或 repo 明确要求的 checks 始终覆盖 validation budget。

## Where it fits

它是 main chain 末段的 build step：

```txt
grill-with-docs -> to-spec -> to-tickets -> implement -> code-review
```

它的邻居是 [to-tickets](https://aihero.dev/skills-to-tickets)、[tdd](https://aihero.dev/skills-tdd) 和 [code-review](https://aihero.dev/skills-code-review)。完整地图见 [ask-matt](https://aihero.dev/skills-ask-matt)。
