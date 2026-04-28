# ADR Format

ADRs 位于 `docs/adr/`，使用顺序编号：`0001-slug.md`、`0002-slug.md` 等。

懒创建 `docs/adr/` 目录，只有第一个 ADR 需要时才创建。

## Template

```md
# {Short title of the decision}

{1-3 sentences: what's the context, what did we decide, and why.}
```

就这样。一个 ADR 可以只有一段。价值在于记录_做出了_某个决策以及_为什么_，不是填满各种 section。

## Optional sections

只有真正增加价值时才包含这些。多数 ADR 不需要。

- **Status** frontmatter（`proposed | accepted | deprecated | superseded by ADR-NNNN`）— 当决策被重新审视时有用
- **Considered Options** — 只有 rejected alternatives 值得记住时才写
- **Consequences** — 只有非显然的下游影响需要指出时才写

## Numbering

扫描 `docs/adr/` 中现有最高编号，并加一。

## When to offer an ADR

以下三点必须都成立：

1. **Hard to reverse** — 以后改变主意的成本有意义
2. **Surprising without context** — 未来读者看代码会想“为什么要这么做？”
3. **The result of a real trade-off** — 存在真实替代方案，并基于具体理由做了选择

如果决策容易逆转，就跳过；反正会逆转。如果它不令人意外，没人会问为什么。如果没有真实替代方案，就没有什么需要记录，除了“我们做了显而易见的事”。

### What qualifies

- **Architectural shape.** “我们使用 monorepo。”“write model 是 event-sourced，read model 投影到 Postgres。”
- **Integration patterns between contexts.** “Ordering 和 Billing 通过 domain events 通信，而不是 synchronous HTTP。”
- **Technology choices that carry lock-in.** Database、message bus、auth provider、deployment target。不是每个 library，只记录需要一个季度才能换掉的选择。
- **Boundary and scope decisions.** “Customer data 由 Customer context 拥有；其他 contexts 只通过 ID 引用。”明确的 no 和 yes 一样有价值。
- **Deliberate deviations from the obvious path.** “我们用 manual SQL 而不是 ORM，因为 X。”任何合理读者会默认相反的地方都要记录，避免下个工程师把有意选择“修掉”。
- **Constraints not visible in the code.** “由于 compliance requirements，我们不能用 AWS。”“由于 partner API contract，response times 必须低于 200ms。”
- **Rejected alternatives when the rejection is non-obvious.** 如果你考虑过 GraphQL 并因微妙理由选择 REST，记录它，否则六个月后又会有人建议 GraphQL。
