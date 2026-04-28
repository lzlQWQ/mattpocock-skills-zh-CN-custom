# Writing Agent Briefs

agent brief 是 issue 移到 `ready-for-agent` 时发布在 GitHub issue 上的结构化 comment。它是 AFK agent 工作所依据的权威 spec。原始 issue body 和讨论是 context；agent brief 是 contract。

## Principles

### Durability over precision

issue 可能在 `ready-for-agent` 中停留数天或数周。期间 codebase 会变化。写 brief 时要让它在 files 被重命名、移动或 refactor 后仍有用。

- **Do** 描述 interfaces、types 和 behavioral contracts
- **Do** 命名 agent 应查找或修改的具体 types、function signatures 或 config shapes
- **Don't** 引用 file paths，它们会过时
- **Don't** 引用 line numbers
- **Don't** 假设当前 implementation structure 会保持不变

### Behavioral, not procedural

描述系统应该做**什么**，而不是**如何**实现。agent 会重新探索 codebase，并自行作出 implementation decisions。

- **Good:** "The `SkillConfig` type should accept an optional `schedule` field of type `CronExpression`"
- **Bad:** "Open src/types/skill.ts and add a schedule field on line 42"
- **Good:** "When a user runs `/triage` with no arguments, they should see a summary of issues needing attention"
- **Bad:** "Add a switch statement in the main handler function"

### Complete acceptance criteria

agent 需要知道何时完成。每个 agent brief 都必须有具体、可测试的 acceptance criteria。每条 criterion 都应能独立验证。

- **Good:** "Running `gh issue list --label needs-triage` returns issues that have been through initial classification"
- **Bad:** "Triage should work correctly"

### Explicit scope boundaries

说明什么不在范围内。这能防止 agent gold-plating 或对相邻 features 做假设。

## Template

```markdown
## Agent Brief

**Category:** bug / enhancement
**Summary:** one-line description of what needs to happen

**Current behavior:**
Describe what happens now. For bugs, this is the broken behavior.
For enhancements, this is the status quo the feature builds on.

**Desired behavior:**
Describe what should happen after the agent's work is complete.
Be specific about edge cases and error conditions.

**Key interfaces:**
- `TypeName` — what needs to change and why
- `functionName()` return type — what it currently returns vs what it should return
- Config shape — any new configuration options needed

**Acceptance criteria:**
- [ ] Specific, testable criterion 1
- [ ] Specific, testable criterion 2
- [ ] Specific, testable criterion 3

**Out of scope:**
- Thing that should NOT be changed or addressed in this issue
- Adjacent feature that might seem related but is separate
```

## Examples

保留原始示例的结构和标识；编写自己的 brief 时，应使用项目的 domain language，并避免易过时的 file paths 与 line numbers。
