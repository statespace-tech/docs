# Documentation instructions

This repository is the public Statespace documentation, built with Mintlify. It follows the practices of
Mintlify's own guides and of popular Mintlify sites, such as Trigger.dev and Resend.

## Product

`statespace-tech/design.md` is the source of truth. The docs must match it.

- An app is one running copy of a service, such as `shop-dev` or `shop-prod`. `ssp app add` adds it. It is
  linked to one repo and one folder.
- CI builds images and records each build ("commit → image digest") with `statespace-tech/action@v1` and
  GitHub OIDC. The proxy runs only images of recorded builds.
- An arm is a commit that runs on a share of an app's traffic. The commit is the arm's ID. A person or an
  agent adds it with `ssp arm add <commit> --app <app>`.
- Live is the service that the customer runs. It is never recorded. The control is a copy of the live
  image that serves only the control's subjects.
- `ssp proxy docker|cloudrun|kubernetes --app <app>` runs in front of the app. The instance that holds
  the lease runs the control and the arms.
- Settings, events, and metrics live in `statespace.toml`, with `[app.<name>]` sections. Events and
  metrics are CEL.
- Evidence goes on the arm's commit as the commit status `statespace/<app>`.
- Holds (`ssp app stop`, `ssp arm stop`) take arms out of traffic. Overrides (`?ssp_arm=<commit>`) let
  testers open a running arm.
- Permissions come from GitHub roles. The only Statespace token is the runtime token of one app.

Document current behavior only. Mark proposed behavior with a visible `Planned` warning. Do not document
models that were removed. When the spec is silent, do not invent behavior.

## Words

- Write in ASD-STE100 Simplified Technical English: short sentences, one idea in each, active voice,
  second person, present tense. Keep sentences in procedures to 20 words or fewer.
- Do not use filler such as "simply", "just", "please note", or "it's worth noting".
- Use one term for each thing: **app**, **live**, **live commit**, **build**, **arm** (never "variant"),
  **control**, **side** (`arm` or `control`), **subject**, **share**, **version**, **hold**,
  **override**, **event**, **metric**, **orchestrator**.
- Use one example everywhere: the organization `acme`, the repo `acme/shop`, the apps `shop-dev` and
  `shop-prod`, the commit `a1b2c3d`, the live commit `9f1ce8a`, the event `checkout`, the metric
  `checkout rate %`, and the image `ghcr.io/acme/shop@sha256:…`. Use only neutral, invented names. Never
  use the names of customers. Write secrets as `<secret>`.

## Pages

Give each page one type (Diátaxis), and keep it to that type.

| Type | Folder | Shape |
| --- | --- | --- |
| Tutorial | root (`quickstart`) | What you get, prerequisites, `<Steps>`, a check of the result, next steps |
| How-to | `getting-started/`, `guides/` | The task in the title, `<Steps>` or short sections, next steps |
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
  `required` where they apply: `<ParamField path="max_traffic" type="number" default="0.2">` for a
  `statespace.toml` key, and `<ParamField body="--app" type="string" required>` for a CLI flag or
  argument.
- Tables for short comparisons and lists of names, such as states or fields. Not for flags.
- `<CodeGroup>` for alternatives of one step.
- `<Note>` for useful context, `<Tip>` for a better way, `<Warning>` for data loss, cost, or a step that
  cannot be undone. One callout per section at most.
- `<AccordionGroup>` for troubleshooting and questions.
- `<Card>` icons come from Lucide.

## Code

- Give each code block a language and, when it is a file, its path as the title:
  ` ```toml statespace.toml `, ` ```yaml .github/workflows/build.yml `.
- Put one sentence before each code block that says what it does.
- Keep code lines to 60 characters where you can, and never over 80, so blocks fit without scrolling.
  Use `\` continuations in shell, one CEL condition per line in `'''` strings, and comments on their own
  line.
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
