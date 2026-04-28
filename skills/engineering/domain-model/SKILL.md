---
name: domain-model
description: 通过 grilling session 对照现有 domain model 挑战计划、收紧术语，并在决策成形时内联更新 documentation（CONTEXT.md, ADRs）。Use when user wants to stress-test a plan against their project's language and documented decisions.
disable-model-invocation: true
---

围绕这个计划的每个方面持续追问，直到我们达成共同理解。沿着 design tree 的每个分支走下去，一次解决一个决策之间的依赖。每个问题都给出你的推荐答案。

一次只问一个问题，并等待每个问题的反馈后再继续。

如果某个问题可以通过探索 codebase 回答，就去探索 codebase。

## Domain awareness

探索 codebase 时，也查找现有文档：

### File structure

多数 repo 只有一个 context：

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

如果 root 下存在 `CONTEXT-MAP.md`，repo 有多个 contexts。map 指向每个 context 的位置：

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

按需懒创建文件：只有当你有内容要写时才创建。如果没有 `CONTEXT.md`，在第一个 term 被解决时创建。如果没有 `docs/adr/`，在第一个 ADR 需要时创建。

## During the session

### Challenge against the glossary

当用户使用的 term 与 `CONTEXT.md` 中已有语言冲突时，立即指出。“你的 glossary 把 'cancellation' 定义为 X，但你现在似乎指 Y。到底是哪一个？”

### Sharpen fuzzy language

当用户使用模糊或 overloaded terms 时，提出精确的 canonical term。“你说 'account'，是指 Customer 还是 User？它们是不同的东西。”

### Discuss concrete scenarios

讨论 domain relationships 时，用具体场景 stress-test。编造能探测 edge cases 的场景，迫使用户精确定义概念边界。

### Cross-reference with code

当用户说明某个东西如何工作时，检查代码是否一致。如果发现矛盾，直接展示：“你的代码会 cancel 整个 Orders，但你刚说 partial cancellation 是可能的，哪个是对的？”

### Update CONTEXT.md inline

当 term 被解决时，立刻更新 `CONTEXT.md`。不要批量积攒；在发生时捕获。使用 [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md) 中的格式。

不要把 `CONTEXT.md` 绑定到 implementation details。只包含对 domain experts 有意义的 terms。

### Offer ADRs sparingly

只有同时满足三点时才提议创建 ADR：

1. **Hard to reverse** — 以后改变主意的成本有意义
2. **Surprising without context** — 未来读者会想“为什么这样做？”
3. **The result of a real trade-off** — 存在真实替代方案，并基于具体理由做了选择

任何一点缺失都跳过 ADR。使用 [ADR-FORMAT.md](./ADR-FORMAT.md) 中的格式。
