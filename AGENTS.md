# Documentation instructions

This repository contains the public Statespace documentation.

## Product boundary

Statespace guarantees who saw what, when, and why. Users analyze raw evidence with PostgreSQL and SQL.

Document current behavior as current. Mark proposed behavior with a visible `Planned` warning. Never present a planned CLI command, YAML field, SDK method, table, or response as released.

## Writing

- Use short, direct sentences.
- Put one main idea in each sentence.
- Use the same `atlas-search` and `rank-v2` example throughout the site.
- Use `ssp_token_7j...` for SDK token examples.
- Never include a complete token.
- Put one explanatory sentence immediately before each code block.
- Prefer concrete examples over placeholders.
- Keep Python, YAML, CLI, and SQL examples consistent.
- Use `randomization unit` for the identifier type defined by an experiment.
- Use `assignment` for one randomization-unit value supplied to an experiment.
- Use `subject` when describing a person, task, session, or other assigned entity.
- Use `group` for control and treatment groups.
- Each account has one isolated PostgreSQL database.
- The CLI creates and versions experiments. SDKs only consume running configurations.

## Structure

- Put onboarding content at the root and in `getting-started/`.
- Put the five high-level experiment concepts under `concepts/`.
- Put the five experiment stages under `workflow/`.
- Put complete worked recipes under `examples/`.
- Put complete schemas and command surfaces under `reference/`.
- Keep navigation in `docs.json` synchronized with files.
- Use noun phrases for concepts and reference titles.
- Use imperative verb phrases for procedures and workflow titles.
- Keep headings parallel within each page.

## Checks

Run the Mintlify checks before publication.

```bash
mint validate
mint broken-links
```

## Commits

- Use Conventional Commits for every commit.
- Use the format `<type>(<scope>): <description>` when a scope is useful.
- Omit the scope when it does not add useful context.
- Keep each commit focused on one change.
