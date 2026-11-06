# Documentation instructions

This repository is the public Statespace documentation, built with Mintlify. It follows the practices of
Mintlify's own guides and of popular Mintlify sites, such as Trigger.dev and Resend.

## Product

`statespace-tech/design/` is the source of truth. The docs must match it.

- An app is one running copy of a service, such as `shop-dev` or `shop-prod`. `ssp app add` adds it. It is
  linked to one repo and one folder.
- CI makes builds and records each one with `statespace-tech/action@v1` and GitHub OIDC. A build is an
  image by digest (`image:`) or a folder of files in a bucket (`files:`). The proxy runs only recorded builds.
- An experiment is a named comparison of commits in one app. Its table `[experiment.<name>]` is in
  `statespace.toml`. Its arms are commits with exact shares, set with
  `ssp experiment update <name> --arm <commit>=<share>`. The first arm shows first.
- `ssp experiment start` and `ssp experiment stop` run an experiment. Each start is a new version. A
  running experiment does not change.
- Experiments in one app never overlap, because each arm is a full build. Different apps are independent
  layers.
- Live is the service that the customer runs. It is never recorded. There is no control arm: to compare
  with live, add the live commit as an arm.
- `ssp proxy docker|cloudrun|kubernetes|fargate|fly|lambda|s3|gcs` runs in front of the app. It gets its app from its
  runtime token. It runs alongside live: it never changes what it did not create, and each arm is a resource
  that it creates, labels, and deletes. The instance that holds the lease runs the arms. `s3` and `gcs` serve static sites: live is the bucket root, and each arm
  is a build folder. Any CDN can sit in front of the proxy.
- Events and experiments live in `statespace.toml`, with `[app.<name>]` sections. An event is a CEL rule
  over one request and its response.
- The proxy counts the subjects of each arm. Results per arm: p50, p95, and p99 latency, and for
  `requests_4xx`, `requests_5xx`, and each event the share of subjects with one or more matches, as a
  fraction with 4 decimals and its 95% Wilson interval. The split check shows `ok` or `off`.
  Statespace keeps no requests, bodies, headers, cookies, logs, or subject IDs.
- Each arm carries labels for the customer's own data tools: resource labels, `SSP_*` variables,
  `OTEL_RESOURCE_ATTRIBUTES`, `ssp-experiment`/`ssp-arm` headers, and the `ssp_arms` cookie.
- Evidence goes on each arm's commit as the commit status `statespace/<app>/<experiment>`.
- Overrides (`?ssp=<experiment>:<commit>`) let testers open an arm of a running experiment. They are always
  on; there is no setting.
- Permissions come from GitHub roles. The only Statespace token is the runtime token of one app.

Document current behavior only. Never mention plans, removed features, or earlier models. When the spec
is silent, do not invent behavior.

## Words

- Write in ASD-STE100 Simplified Technical English: short sentences, one idea in each, active voice,
  second person, present tense. Keep sentences in procedures to 20 words or fewer.
- Do not use filler such as "simply", "just", "please note", or "it's worth noting".
- Use one term for each thing: **app**, **live**, **live commit**, **build**, **experiment**, **arm**
  (never "variant"), **subject**, **share**, **version**, **unit**, **override**,
  **event**, **orchestrator**. A **visitor** is a person who visits the site, such as in a traffic
  diagram. Never use "visitor" for a subject.
- Use one example everywhere: the organization `acme`, the repo `acme/shop`, the apps `shop-dev` and
  `shop-prod`, the experiments `checkout` and `us-landing`, the live commit `c0ffee1`, the commit
  `a1b2c3d`, the event `purchase`, and the image
  `ghcr.io/acme/shop@sha256:…`. Use only neutral, invented names. Never
  use the names of customers. Write secrets as `<secret>`.

## Pages

Give each page one type (Diátaxis), and keep it to that type.

| Type | Folder | Shape |
| --- | --- | --- |
| Tutorial | root (`quickstart`) | What you get, prerequisites, `<Steps>`, a check of the result, next steps |
| How-to | `getting-started/`, `runtimes/`, `guides/` | The task in the title, `<Steps>` or short sections, next steps |
| Explanation | `concepts/` | Starts with the question it answers; no procedures |
| Reference | `reference/` | Complete and scannable; follows the structure of the thing it describes |

The site has three tabs: **Docs** (Get started, Concepts, Guides), **Runtimes** (Containers: Docker,
Kubernetes, Cloud Run, ECS Fargate, Fly.io; Functions: AWS Lambda; Static sites: S3, Cloud Storage), and **Reference**
(`statespace.toml`, CEL, commit status, CLI). Each runtime page has the logo of its platform as its `icon`,
from `images/runtimes/`: one gray (`#8A8F98`), from Simple Icons. Other pages have no icon. The HTTP API is internal: the CLI, the action, the proxy, and
the dashboard call it. Do not document its routes. Scripts and agents use the CLI with `--json`. The runtimes are in the order of the CLI README. When you move a page, add a redirect from its
old URL in `docs.json`.

### Length

- Overview: the README intro, the how-it-works diagram, then cards. Quickstart: 10 steps (or 7), about 250
  words outside code.
- Most pages: 200 to 600 words outside code. Use H2 sections, H3 only when needed, never H4.
- Every runtime page has the same sections, in this order: what it runs (diagram), prerequisites, steps
  (CI, token, run the proxy, check), how it works, troubleshooting, next steps.
- Cut what the reader does not need for the task of the page. Link to the one page that has the fact.

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
