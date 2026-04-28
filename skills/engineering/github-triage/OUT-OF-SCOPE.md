# Out-of-Scope Knowledge Base

repo 中的 `.out-of-scope/` 目录保存 rejected feature requests 的持久记录。它有两个目的：

1. **Institutional memory** — 记录为什么 feature 被拒绝，避免 issue 关闭后理由丢失
2. **Deduplication** — 当新 issue 匹配先前 rejection 时，skill 可以展示之前的 decision，而不是重新争论

## Directory structure

```
.out-of-scope/
├── dark-mode.md
├── plugin-system.md
└── graphql-api.md
```

每个**概念**一个文件，不是每个 issue 一个文件。请求同一件事的多个 issues 聚合到同一个文件下。

## File format

文件应以轻松、可读的风格编写，更像短 design document，而不是 database entry。使用段落、code samples 和 examples，让第一次看到的人也能理解 reason。

```markdown
# Dark Mode

This project does not support dark mode or user-facing theming.

## Why this is out of scope

The rendering pipeline assumes a single color palette defined in
`ThemeConfig`. Supporting multiple themes would require:

- A theme context provider wrapping the entire component tree
- Per-component theme-aware style resolution
- A persistence layer for user theme preferences

This is a significant architectural change that doesn't align with the
project's focus on content authoring. Theming is a concern for downstream
consumers who embed or redistribute the output.

```ts
// The current ThemeConfig interface is not designed for runtime switching:
interface ThemeConfig {
  colors: ColorPalette; // single palette, resolved at build time
  fonts: FontStack;
}
```

## Prior requests

- #42 — "Add dark mode support"
- #87 — "Night theme for accessibility"
- #134 — "Dark theme option"
```

### Naming the file

为 concept 使用简短、描述性的 kebab-case 名称：`dark-mode.md`、`plugin-system.md`、`graphql-api.md`。名称应足够可识别，让浏览目录的人不打开文件也能知道拒绝了什么。

### Writing the reason

reason 应该实质化，不是“we don't want this”，而是为什么。好的 reasons 会引用：

- Project scope 或 philosophy（“This project focuses on X; theming is a downstream concern”）
- Technical constraints（“Supporting this would require Y, which conflicts with our Z architecture”）
- Strategic decisions（“We chose to use A instead of B because...”）

reason 应该 durable。避免引用临时情况（“we're too busy right now”），这不是拒绝，而是延期。

## When to check `.out-of-scope/`

triage 的 Step 1: Gather context 阶段，读取 `.out-of-scope/` 中所有 files。评估新 issue 时：

- 检查 request 是否匹配现有 out-of-scope concept
- 匹配依据是 concept similarity，不是 keyword；“night theme” 匹配 `dark-mode.md`
- 如果匹配，告诉 maintainer：“This is similar to `.out-of-scope/dark-mode.md` — we rejected this before because [reason]. Do you still feel the same way?”

maintainer 可以：

- **Confirm** — 新 issue 被添加到现有文件的 "Prior requests" list，然后关闭
- **Reconsider** — 删除或更新 out-of-scope file，issue 继续正常 triage
- **Disagree** — issues 相关但不同，继续正常 triage

## When to write to `.out-of-scope/`

只有 **enhancement**（不是 bug）作为 `wontfix` 被拒绝时才写入。流程：

1. Maintainer 决定 feature request 不在 scope 内
2. 检查是否已有匹配的 `.out-of-scope/` file
3. 如果有：把新 issue append 到 "Prior requests" list
4. 如果没有：创建新 file，包含 concept name、decision、reason 和第一个 prior request
5. 在 issue 上发布 comment 解释 decision，并提到 `.out-of-scope/` file
6. 使用 `wontfix` label 关闭 issue

## Updating or removing out-of-scope files

如果 maintainer 改变对先前 rejected concept 的想法：

- 删除 `.out-of-scope/` file
- skill 不需要 reopen old issues；它们是历史记录
- 触发 reconsideration 的新 issue 继续正常 triage
