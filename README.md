# Notion Product Design Skill

A Codex skill for analyzing and designing products, workflows, UI/UX interactions, and AI agents through Notion's product-design philosophy.

It treats Notion as a system for user agency—not a visual style—and focuses on composable primitives, progressive disclosure, direct manipulation, local views over shared data, and careful state design.

## Contents

- `SKILL.md` — the skill instructions and evaluation workflow.
- `agents/openai.yaml` — Codex display metadata and a default prompt.
- `references/` — research notes, design frameworks, cases, product evolution, and source index.

## Install

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/Oreo992/notion-product-design-skill.git \
  ~/.codex/skills/notion-product-design
```

Then invoke it in Codex with:

```text
$notion-product-design 以 Notion 的设计理念和真实案例评审这个产品或交互方案。
```

## Scope and sources

The reference materials distinguish facts, inferences, and design guidance. For time-sensitive claims about current Notion behavior, verify the linked primary source during the task.

`references/source-index.md` lists the underlying sources. A local transcript used during research is deliberately excluded from this public repository because its redistribution rights have not been established.

Notion is a trademark of Notion Labs, Inc. This is an independent, unofficial project and is not affiliated with or endorsed by Notion.

## License

The original material in this repository is released under the [MIT License](LICENSE). Linked third-party sources retain their respective rights.
