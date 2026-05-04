# CLAUDE.md — FrankenPress docs

Guidance for Claude Code (and other AI agents) when working in this docs
repo.

## What this repo is

[Mintlify](https://mintlify.com/)-powered documentation for **FrankenPress**:
the four-repo opinionated WordPress-on-Kubernetes stack maintained by
[EightOEight](https://eightoeight.io).

The docs site is auto-deployed by Mintlify on every push to `main`.

## Mintlify essentials

- `docs.json` — config + sidebar navigation. Adding a page means adding it to a `pages: [...]` array under the appropriate `groups[]` entry.
- `*.mdx` — Markdown + JSX components. Frontmatter (`title`, `description`, `icon`) controls the page header. Mintlify uses [Lucide-style](https://lucide.dev/) and FontAwesome icon names.
- Components: `<Card>`, `<CardGroup>`, `<Steps>` / `<Step>`, `<AccordionGroup>` / `<Accordion>`, `<Tip>`, `<Note>`, `<Warning>`, `<Check>`, `<Tabs>` / `<Tab>`. Use them — they render way better than equivalent Markdown.
- Internal links are absolute paths without the `.mdx` extension: `[X](/components/fp-runtime)`.
- External links use full URLs.
- Code blocks: triple backticks with a language tag. For shell commands, prefer `bash`. Use `text` for plain output / diagrams.

## FrankenPress conventions

The four repos:

- [`fp-runtime`](https://github.com/EightOEight/fp-runtime) — Caddy + FrankenPHP + Souin base image
- [`fp-mu-plugin`](https://github.com/EightOEight/fp-mu-plugin) — slim must-use plugin (S3 bootstrap + Souin invalidator)
- [`fp-site-template`](https://github.com/EightOEight/fp-site-template) — GitHub template repo for new sites
- [`fp-charts`](https://github.com/EightOEight/fp-charts) — Helm chart `fp-site` for Kubernetes deployment

When you reference these in docs, **link to the repo**. When you
reference a section of docs, link to the `/components/<name>` or
`/operations/<topic>` page.

## Style rules

- **Be terse.** Engineers reading these docs are time-pressed. Cut filler.
- **No emoji** in user-facing copy unless it's clearly a UI affordance (the 🚧 hardhat is acceptable for "under construction"). Avoid 🎉 / 🚀 / 🔥 / etc.
- **Show defaults next to params.** The default goes in the same row as the description, not below it.
- **Show the failure mode.** "If X is missing, you'll see Y" — readers are usually here because something broke.
- **Production-vs-dev symmetry.** Every "this is the dev default" statement is paired with the production swap. The chart's bundled subcharts are the easiest example.
- **Lockdown is a feature, not a bug.** The "Update WordPress" buttons are absent on purpose. Frame this clearly when it comes up — readers will assume it's a bug otherwise.

## Don'ts

- **Don't reference Bitnami images by their `bitnami/<name>` path.** Always `bitnamilegacy/<name>`. Bitnami withdrew the public `bitnami/*` images in 2025.
- **Don't recommend Argo CD / Kargo / Flux as required.** The chart is GitOps-agnostic. Mention them as compatible options, not prerequisites.
- **Don't link to the Bitnami "Secure Images" commercial subscription as the canonical install path.** It's the workaround we don't use.
- **Don't write multi-paragraph "introduction" sections.** Two sentences max, then jump to the actionable content.
- **Don't add Mintlify starter-kit-style copy** ("Welcome to your new docs!", "Make it yours", etc.). The starter copy was deleted on purpose; don't bring it back.

## Quickstart smoke test

The kind-cluster quickstart is **smoke-tested** on every release:

1. Create a fresh kind cluster (`kind create cluster --name <name>`)
2. Run the steps from `quickstart.mdx` verbatim
3. Verify the WP admin loads, an upload lands in MinIO, and the lockdown buttons are absent

If a doc step fails when run verbatim, treat it as a release blocker.
The Mintlify preview is `mint dev` from the repo root after `npm i -g mint`.

## When you change a public env var or values key

If you bump or rename anything in `fp-runtime`, `fp-mu-plugin`,
`fp-site-template`, or `fp-charts` that's documented here, update:

1. The component-specific page (`/components/<name>`)
2. `/operations/configuration` (the env-var reference)
3. The relevant section of `quickstart.mdx` if it changed the user-facing flow

## Companion repos quick-link

| Repo | Purpose | Local path |
|---|---|---|
| [`fp-runtime`](https://github.com/EightOEight/fp-runtime) | Base container image | `~/Developer/EightOEight/fp-runtime/` |
| [`fp-mu-plugin`](https://github.com/EightOEight/fp-mu-plugin) | Must-use plugin | `~/Developer/EightOEight/fp-mu-plugin/` |
| [`fp-site-template`](https://github.com/EightOEight/fp-site-template) | Site template | `~/Developer/EightOEight/fp-site-template/` |
| [`fp-charts`](https://github.com/EightOEight/fp-charts) | Helm chart | `~/Developer/EightOEight/fp-charts/` |
| [`docs`](https://github.com/EightOEight/docs) (this repo) | Mintlify docs | `~/Developer/EightOEight/docs/` |
