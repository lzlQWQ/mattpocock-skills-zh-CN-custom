# CONTEXT.md Format

## Structure

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**Order**:
{A concise description of the term}
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account

## Relationships

- An **Order** produces one or more **Invoices**
- An **Invoice** belongs to exactly one **Customer**

## Example dialogue

> **Dev:** "When a **Customer** places an **Order**, do we create the **Invoice** immediately?"
> **Domain expert:** "No — an **Invoice** is only generated once a **Fulfillment** is confirmed."

## Flagged ambiguities

- "account" was used to mean both **Customer** and **User** — resolved: these are distinct concepts.
```

## Rules

- **Be opinionated.** 同一概念有多个词时，选择最好的一个，并把其他词列为 aliases to avoid。
- **Flag conflicts explicitly.** 如果 term 被模糊使用，在 "Flagged ambiguities" 中指出并给出清晰 resolution。
- **Keep definitions tight.** 最多一句。定义它_是什么_，不是它_做什么_。
- **Show relationships.** 使用粗体 term names，并在明显时表达 cardinality。
- **Only include terms specific to this project's context.** 通用编程概念（timeouts、error types、utility patterns）不属于这里，即使项目大量使用。添加 term 前先问：这是这个 context 独有概念，还是通用编程概念？只包含前者。
- **Group terms under subheadings** 当自然 clusters 出现时使用。如果所有 terms 属于一个 cohesive area，扁平列表即可。
- **Write an example dialogue.** 写一段 dev 与 domain expert 的对话，展示 terms 如何自然互动，并澄清相关概念边界。

## Single vs multi-context repos

**Single context（多数 repos）：** root 下一个 `CONTEXT.md`。

**Multiple contexts：** root 下的 `CONTEXT-MAP.md` 列出 contexts、位置以及相互关系：

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — receives and tracks customer orders
- [Billing](./src/billing/CONTEXT.md) — generates invoices and processes payments
- [Fulfillment](./src/fulfillment/CONTEXT.md) — manages warehouse picking and shipping

## Relationships

- **Ordering → Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment → Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

skill 会推断适用结构：

- 如果 `CONTEXT-MAP.md` 存在，读取它来查找 contexts
- 如果只有 root `CONTEXT.md`，就是 single context
- 如果两者都不存在，在第一个 term 被解决时懒创建 root `CONTEXT.md`

当存在多个 contexts 时，推断当前话题属于哪一个。不清楚就询问。
