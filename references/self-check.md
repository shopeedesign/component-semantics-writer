# Component Semantics Self-Check

Run this checklist before final delivery.

## Language

- Natural-language content is in the user's requested language.
- Technical identifiers remain exact: component names, prop names, Figma variant values, code paths, URLs.
- If the document is Chinese, replace generic English logic words like `If/Then`, `When to use`, `Do not` with Chinese section text and “如果…则…” rules.

## Evidence

- Every code prop mentioned as real has been verified from source, generated API metadata, or official component docs.
- Deprecated props are labeled and replacement props are named.
- Figma component set names and variant values match the actual library.
- If evidence is missing, the document says so instead of pretending.

## Semantics

- Component family and concrete variant/subcomponent are not conflated.
- Synonyms map to exact targets when they span multiple subcomponents.
- Open input, fixed choice, tree choice, cascade choice, date/time choice, upload, and display-only cases are separated.
- Range means two boundary values unless the verified component explicitly supports more.
- Examples do not combine incompatible semantics in one field.
- Anatomy describes semantic slots and recommended composition, not internal DOM, unless source code was actually inspected.
- If Anatomy uses a tree diagram, it includes a boundary note that generated code should follow Code Connect snippets and real APIs.

## Variant Rules

- Variant rules use condition-first logic: “如果场景 X，则使用 Y”.
- Visual state and semantic state are separated, especially hover/open/disabled/error/loading.
- Error state always includes error copy or validation ownership.
- Figma-to-code mappings are explicit when names differ.

## High-Risk Props

Check these carefully before finalizing:

- Search: verify exact code shape, e.g. `onSearch`, `showSearch`, `Input.Search`, or custom search component.
- Count: separate length limit from count display, e.g. `maxLength`, `hideCount`, `showCount`.
- Add-ons: verify code names such as `prepend/append` vs `addonBefore/addonAfter`.
- Loading: verify whether the base component supports it and limit it to field-level async states.
- Ellipsis: avoid recommending it for active free editing unless the library intentionally supports that use.
- TextArea autosize: verify exact API, e.g. `autoRows` vs `autoSize`.
- Custom render: warn about performance and search/filter side effects.

## Content Guidelines

- Labels are short business nouns and do not end with colons.
- Placeholder is not the only label and does not carry critical rules.
- Error copy tells users what is wrong and how to fix it.
- Helper text describes format, limits, or examples before input.
- Option text and field labels use consistent business vocabulary.

## Final Artifact

- File name follows `<ComponentName>.component-semantics.md`.
- `Code Connect:` is blank unless verified.
- Links are current and point to the most relevant component page or Figma node.
- The final answer mentions any unavailable source, such as missing local code package.
