---
name: component-semantics-writer
description: Generate AI-friendly semantic Markdown documentation for design-system components from Figma libraries, component documentation sites, Storybook/dumi pages, and source code/API metadata. Use when the user asks to create, review, standardize, or refine component semantics docs, variant-selection rules, component usage guidance, anti-patterns, or AI-friendly design-system specifications for components such as Input, Select, Button, Table, DatePicker, Form, etc.
---

# Component Semantics Writer

## Overview

Create concise, AI-friendly component semantic documentation that helps future AI agents choose the right component, variant, composition, and copy. Optimize for decision logic over visual description.

Use Chinese for natural-language content by default when the user writes in Chinese. Keep component names, prop names, Figma variant values, file paths, links, and code identifiers in their original language.

## Default Sources

Use these SSC UI sources by default when the user asks for an SSC component semantic document and does not provide overrides:

- Figma Library: `https://www.figma.com/design/nkFgp5CZyxcE7CB399pYd0/PC-Library-%E9%87%8D%E6%9E%84`
- Figma file key: `nkFgp5CZyxcE7CB399pYd0`
- Documentation root: `https://shopee.git-pages.garena.com/bg-logistics/sscfe/business/base/ssc-ui/ssc-ui-react/#/components`
- Documentation path pattern: `/record/<component-kebab-name>` for record/data-entry components when applicable.

Do not require the local component code package by default. If no local code package or repository path is provided, rely on verified Figma, Code Connect, documentation site, and generated API metadata, then state that source code was not inspected.

## Workflow

1. Ask for optional module-specific reference materials before writing:
   - Ask whether the user has extra materials for `Semantic Intent`, `Contexts`, `Content Guidelines`, and `Best Practices`.
   - Use one concise prompt. Mention that the default SSC UI Figma Library and documentation site will be used unless the user provides replacements.
   - If the user says there are no extra materials, continue with available Figma, docs, Code Connect, and code evidence.
   - Treat user-provided module references as guidance for those modules only; do not use them to override verified component API, Figma variants, or Code Connect facts.
2. Gather component evidence from all available sources:
   - Figma Library: use the default SSC UI file unless the user provides another Figma file. Inspect component sets, variants, component properties, slots, and instance examples.
   - Documentation site: use the default SSC UI documentation root unless the user provides another docs site. Inspect usage examples, API tables, caveats, and demo descriptions.
   - Code Connect: if present, inspect mappings/snippets and source links.
   - Code package: optional. Inspect props interfaces, exports, deprecated props, default values, and internal naming only when a local package or repository path is provided.
3. Separate facts from recommendations:
   - If a prop exists in code but is semantically risky, keep it and add usage boundaries.
   - If a prop is not verified, do not present it as code truth.
   - If Figma naming is awkward but current, document the mapping and recommend future cleanup separately.
4. Write the Markdown using the required section order.
5. Run the semantic self-check in `references/self-check.md`.
6. Save the file as `<ComponentName>.component-semantics.md` unless the user asks for inline output.

## Evidence Rules

- Prefer source/API metadata over memory for prop names.
- For dumi/Storybook static sites, inspect bundled JS when pages are static-rendered or route-based.
- For Figma writes or unique Figma reads, load `figma-use` before `use_figma`.
- For Figma Library analysis, inspect component sets and component property definitions. Focus on variant axes and designer-facing component names.
- Do not infer internal DOM structure unless source code has been inspected. Treat `Anatomy` as semantic slots and recommended composition by default.
- If local code package is missing, say so and rely only on verified docs/Figma data.
- Never invent Code Connect links. Leave `Code Connect:` blank if not available.

## Output Template

Use this exact section order unless the user provides another template:

```markdown
# <Component> 组件语义化规范

## 组件元数据

Component ID:

Code Connect:

Figma Link:

Storybook Path:

## 语义与意图 (Semantic Intent)

### 主要用途

### AI 别名

## 组件适用上下文 (Contexts)

### 何时使用

### 何时不使用

## Anatomy (解剖)

## 常见组合（Composition）

## 变体及选择规则 (Variant Selection)

## 常见错误指引 / 反模式 (Anti-Patterns)

## 文案规范 (Content Guidelines)

## 最佳实践 (Best Practices)
```

## Writing Rules

- Use “如果…则…” rules for variant and component selection.
- Keep visual descriptions minimal. Preserve decision logic, semantic boundaries, and mapping rules.
- In `Anatomy`, describe semantic slots, ownership, and recommended composition, not source-code DOM. Add a boundary note when using tree diagrams.
- Distinguish component family from concrete subcomponents.
  - Example: `Input` is a family; `Input / Base` is single-line text.
- Use alias mapping when synonyms span multiple subcomponents.
  - Example: “Text Area -> `Input.TextArea`”, “Number Field -> `Input.Number`”.
- Keep Figma, semantic, and code names aligned in mapping tables when names differ.
- Explain current-state mappings and recommended future naming separately.
- Avoid examples that combine incompatible semantics, such as money prefix `$` with weight suffix `kg` in one field.
- Mention deprecated props only when useful, and explicitly say what to use instead.
- Treat loading, ellipsis, custom render, search, and add-new features as high-risk affordances: verify they exist and add usage boundaries.

## Useful Patterns

### Reference Intake Prompt

Ask this before generating a component semantic document:

```text
在生成这个组件的语义化文档前，你是否有以下模块的补充资料需要我参考？

1. 语义与意图 (Semantic Intent)
2. 组件适用上下文 (Contexts)
3. 文案规范 (Content Guidelines)
4. 最佳实践 (Best Practices)

如果有，请按模块补充链接、文档或要点；如果没有，我会默认使用 SSC UI 的 Figma Library 和文档站，并结合 Code Connect / API metadata 继续生成。
```

### Metadata

- `Component ID`: use package/component identity, e.g. `ssc-ui-react/Input`.
- `Code Connect`: leave blank unless verified.
- `Figma Link`: link to the canonical component frame or component set.
- `Storybook Path`: link to the docs page.

### Semantic Intent

Write the essence in one short paragraph. Clarify neighboring components:

```markdown
`Select` 用于让用户从一组预定义选项中选择一个或多个值。它表达的是“选择已有选项”，而不是“自由输入新值”。
```

### Alias Mapping

Prefer a table when one component family contains subcomponents:

```markdown
| 用户说法 | 应映射组件 |
| --- | --- |
| 文本框、Text Field | `Input / Base` |
| 数字输入、Number Field | `Input.Number` |
| 长文本、Text Area | `Input.TextArea` |
```

### Anatomy

Use `Anatomy` to show semantic slots and composition ownership. Unless the real DOM was inspected, include this boundary note:

```markdown
以下结构描述组件的语义槽位和推荐组合关系，不代表源码内部 DOM 结构；生成代码时应以 Code Connect snippet 和真实 API 为准。
```

### Figma-Code Mapping

Use a mapping block when Figma naming, semantic naming, and code props differ:

```text
内部前缀 / 内部后缀 -> Figma: Type=prefix text / Type=suffix text -> Code: prefix / suffix
外部前置元素 / 外部后置元素 -> Figma: Input / Prepend Label -> Code: prepend / append
```

## Handling Review Feedback

When the user asks whether issues need changes:

1. Verify each challenged prop or variant against source evidence.
2. Classify each item:
   - Must change: semantically misleading or factually wrong.
   - Keep with boundary: true API, risky usage.
   - Keep as is: accurate and not likely to mislead.
3. Apply edits directly when the requested direction is clear.
4. Summarize what changed and what was verified.
