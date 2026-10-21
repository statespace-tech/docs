# Documentation instructions

This repository is the public Statespace documentation, built with Mintlify. It follows the practices of
Mintlify's own guides and of popular Mintlify sites, such as Trigger.dev and Resend.

## Product

- A branch named `exp/<name>` is an experiment with one arm. `exp/<experiment>/<arm>` adds arms to one
  experiment. An arm runs while GitHub says that its pull request can merge.
- Settings live in `statespace.toml`. Arm settings come from the `exp/` branch; app settings come from the
  default branch.
- The GitHub App opens the "Set up Statespace" pull request, reports pull request state, posts the
  `statespace/apply` and `statespace/arm` checks, commits dashboard edits, and makes pull requests drafts
  for stops.
- The CLI records builds in CI (`ssp apply`), shows and stops arms (`status`, `stop`, `resume`), manages
  tokens, and runs `ssp proxy` and `ssp controller kubernetes`. It has no command that only reads recorded
  data: SQL reads data.
- Each account has one PostgreSQL database, with one schema of views per app.

Document current behavior only. Mark proposed behavior with a visible `Planned` warning.

## Words

- Write in ASD-STE100 Simplified Technical English: short sentences, one idea in each, active voice,
  second person, present tense.
- Do not use filler such as "simply", "just", "please note", or "it's worth noting".
- Use one term for each thing: **arm** (the image of a commit of an `exp/` branch), **control** (the
  default build of an arm's base), **default** (the deployed version), **subject** (who gets an arm),
  **base** (the commit where the branch left the default branch). Use `control group` and
  `treatment group` only for statistics.
- Use one example everywhere: the organization `acme`, the repository `acme/atlas`, the app `atlas`, the
  experiment `ranking` with the arms `recent-clicks` and `bm25`, and pull request `#418`. For a one-arm
  example, use `exp/recent-clicks`. Write tokens as `ssp_tok_...`, never complete.

## Pages

Give each page one type (Diátaxis), and keep it to that type.

| Type | Folder | Shape |
| --- | --- | --- |
| Tutorial | root (`quickstart`) | What you get, prerequisites, `<Steps>`, a check of the result, next steps |
| How-to | `getting-started/`, `workflow/`, `examples/` | The task in the title, `<Steps>` or short sections, next steps |
| Explanation | `concepts/` | Starts with the question it answers; no procedures |
| Reference | `reference/` | Complete and scannable; follows the structure of the thing it describes |

- Every page has `title`, `description`, and a short `sidebarTitle` when the title is long.
- Do not add an H1: Mintlify makes it from `title`. Use sentence case. Do not skip heading levels.
- Start with one or two sentences that say what the page is for. No preamble.
- Say each fact in one place. Link to it from other pages, or reuse a snippet from `snippets/`.
- End tutorials and how-to pages with a `## Next steps` `<CardGroup cols={2}>` of two to four cards.

## Components

- `<Steps>` for a sequence that the reader does in order. Each step title starts with a verb.
- `<ParamField>` for each setting, flag, and argument in reference pages, with `type`, `default`, and
  `required` where they apply: `<ParamField path="share" type="number" required>` for a
  `statespace.toml` key, and `<ParamField body="--image" type="string" required>` for a CLI flag or
  argument.
- Tables for short comparisons and lists of names, such as views or presets. Not for flags.
- `<CodeGroup>` for alternatives of one step, such as Docker and Kubernetes.
- `<Note>` for useful context, `<Tip>` for a better way, `<Warning>` for data loss, cost, or a step that
  cannot be undone. One callout per section at most.
- `<AccordionGroup>` for troubleshooting and questions.
- `<Card>` icons come from Lucide.

## Code

- Give each code block a language and, when it is a file, its path as the title:
  ` ```toml statespace.toml `, ` ```yaml .github/workflows/statespace.yml `.
- Put one sentence before each code block that says what it does.
- Keep code lines to 60 characters, so blocks fit without scrolling at 1440 pixels. One key per line in
  `print(json.dumps({...}))`; one SQL condition per line, with `FILTER` and `WITHIN GROUP` on a
  continuation line and `::float` casts; `\` continuations in shell; comments on their own line.
- Use real numbered lists for sequences, never `a → b → c` in a code block.

## Checks

Run the Mintlify checks before you commit. Mintlify needs Node 22.

```bash
npx -y -p node@22 -p mint@latest mint broken-links
npx -y -p node@22 -p mint@latest mint dev
```

## Commits

Use Conventional Commits, such as `docs(reference): document flags with ParamField`. Keep each commit to
one change.
