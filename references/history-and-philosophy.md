# History and philosophy

## Contents

- Thesis
- Origins
- Kyoto rebuild
- Product eras
- Design DNA
- Persistent tensions
- Application notes

## Thesis

Notion did not begin as “better notes.” Its durable ambition is to make software toolmaking accessible to non-programmers. The document editor is the familiar entry point; composable software is the deeper model.

## Origins

Ivan Zhao's recurring historical frame starts with analog office tools: the typewriter and filing cabinet became word processors, documents, folders, and siloed SaaS. Early computing pioneers instead imagined computers as a medium that augments human intellect.

Key influences repeatedly named in first-party material:

- Douglas Engelbart: computers should expand people's ability to understand and solve complex problems.
- Ted Nelson: hypertext, bidirectional relationships, and transclusion.
- Alan Kay: personal computing as a dynamic medium that people can shape.
- Christopher Alexander: good form is a fit between solution and context; pattern language makes complex systems intelligible.

Ivan's founding insight, recorded in Sequoia's profile, was that creative people often had taste but lacked tools. The leverage was not to code each person's bespoke site or workflow, but to give people components from which to make their own.

## Kyoto rebuild

In 2015 the early product approached failure. Ivan later said the team had built something they considered cool, not what people needed. Ivan Zhao and Simon Last returned to first principles and rebuilt in and around Kyoto.

The Kyoto interview connects Notion's craft culture to:

- simple, functional, timeless architecture;
- long-horizon craftsmanship;
- hospitality and delight;
- quiet concentration paired with exposure to new ideas.

Do not romanticize Kyoto as a single causal explanation. The more useful product lesson is that the mission survived while the entry point changed: broad visual programming narrowed into familiar pages, lists, wikis, and templates.

## Product eras

### 2016: Notion 1.0 — approachable canvas

- Pages, writing, lists, drag-and-drop composition, wikis, and starter templates.
- Familiar document behavior served as the ramp into a deeper block model.
- Product Hunt and direct founder support helped a small community discover and teach use cases.

### 2018: Notion 2.0 — structured work

- Relational databases, multiple views, and project-management use cases became central.
- Content and structure coexisted: a database item was both a record with properties and a page with open-ended content.
- Templates and community examples turned assemblies into shareable products.

### 2021–2022: platform and collaboration

- Public API, published block data model, Synced Blocks, redesigned comments, teamspaces/sidebar.
- The architecture moved from individual composition toward integrations, reuse, organization, and enterprise governance.

### 2023–2024: packaged products and contextual AI

- Notion AI appeared inside existing page and database contexts rather than as only a standalone chat surface.
- Calendar, Forms, layouts, Sites, and Mail extended Notion into more opinionated products while reusing views, blocks, properties, and customization.

### 2025–2026: delegated agency and runtime

- Notion 3.0 Agents could create pages, databases, and multi-step work.
- Custom Agents made repeatable behavior shareable and governable.
- Developer Platform added Workers, data sync, tools, CLI, and external agents.
- Notion's boundary expanded from user-built workspace to a shared operating environment for people, structured knowledge, code, and agents.

## Three generations of user agency

This is an analytical synthesis, not an official Notion taxonomy:

1. **Direct manipulation:** users create, drag, nest, and transform blocks.
2. **Declarative configuration:** users define schemas, views, templates, formulas, and automations.
3. **Delegated action:** users describe goals and supervise agents using those same primitives.

The generations accumulate. A good agent result remains inspectable and editable through direct manipulation and declarative structure.

## Design DNA

1. Give users tool-shaping power.
2. Prefer a small grammar of composable primitives over disconnected features.
3. Use familiar behavior as a bridge into a novel computational model.
4. Reveal structure when intent or consequence makes it relevant.
5. Turn expert assemblies into templates and packaged tools.
6. Preserve quiet reading while supporting powerful editing.
7. Let system rules create predictability and explicit exceptions protect real jobs.
8. Treat craft and infrastructure as one experience: pixels, latency, permissions, and recovery all matter.
9. Put AI inside the object and workflow model, not above it as a decorative chat layer.
10. Increase governance as autonomy and organizational scale increase.

## Persistent tensions

| Tension | Notion's recurring response | Residual risk |
|---|---|---|
| Flexibility vs approachability | Familiar editor, templates, packaged workflows, AI | Blank-canvas anxiety; hidden power |
| Universal system vs purpose-built speed | Shared principles plus Calendar/Mail surfaces | Fragmentation across product family |
| Quiet UI vs discoverability | Hover/focus/selection affordances | Users miss weak signals |
| Consistency vs contextual fit | Reusable grammar plus justified exceptions | Exceptions become design debt |
| Creation freedom vs reading order | Page rhythm, hierarchy, layouts, views | Beautiful dashboards can become hard to maintain |
| Composition vs reliability | Heavy investment in data, cache, sync, offline, permissions | Edge-state explosion |
| Personal customization vs team governance | Templates, teamspaces, permissions, shared databases | Local systems become organizational entropy |
| Agent autonomy vs control | Permissions, logs, reversible changes, progressive trust | Users may not understand continuously changing systems |

## Application notes

- Use history to explain invariant intent, not to prove current UI behavior.
- Distinguish original vision, launch behavior, and current implementation.
- When borrowing from Notion, ask whether the target product truly benefits from composition. Many products need a decisive workflow, not a general-purpose canvas.
- Visual restraint is an outcome of a coherent model, not a substitute for one.
