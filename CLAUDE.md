# CLAUDE.md — docs

Guidance for Claude Code (and other AI agents) when working in this docs
repo.

## What this repo is

[Mintlify](https://mintlify.com/)-powered documentation for **FrankenPress**:
the four-repo opinionated WordPress-on-Kubernetes stack maintained by
[EightOEight](https://eightoeight.io).

Live at **<https://docs.frankenpress.com/>** — auto-deployed by Mintlify
from this repo's main branch.

## Mintlify essentials

- `docs.json` — config + sidebar navigation. Adding a page means adding it to a `pages: [...]` array under the appropriate `groups[]` entry under `navigation.tabs[].groups[]`.
- `*.mdx` — Markdown + JSX. Frontmatter (`title`, `description`, `icon`) controls the page header. Mintlify uses [Lucide-style](https://lucide.dev/) and FontAwesome icon names.
- Components: `<Card>`, `<CardGroup>`, `<Steps>` / `<Step>`, `<AccordionGroup>` / `<Accordion>`, `<Tip>`, `<Note>`, `<Warning>`, `<Check>`, `<Tabs>` / `<Tab>`. Use them — they render way better than equivalent Markdown.
- Internal links are absolute paths without the `.mdx` extension: `[X](/components/runtime)`.
- External links use full URLs.
- Code blocks: triple backticks with a language tag. For shell commands, prefer `bash`. Use `text` for plain output / diagrams.

## File layout

- `index.mdx` — landing page.
- `architecture.mdx` — ASCII flow diagram + image-promotion model.
- `quickstart.mdx` — 3-step kind-cluster deploy. **Smoke-tested verbatim on every release** — if a step fails when run as written, treat it as a release blocker.
- `components/<repo>.mdx` — one per `*` repo. Env vars, build args, install patterns.
- `operations/{production,configuration,upgrade,troubleshooting}.mdx` — operational reference.
- `docs.json` — navigation + branding.

## Style rules

- **Be terse.** Engineers reading these docs are time-pressed. Cut filler.
- **No emoji** in user-facing copy unless it's clearly a UI affordance. Avoid 🎉 / 🚀 / 🔥 / etc.
- **Show defaults next to params.** Default goes in the same row as the description, not below it.
- **Show the failure mode.** "If X is missing, you'll see Y" — readers are usually here because something broke.
- **Production-vs-dev symmetry.** Every "this is the dev default" statement is paired with the production swap. The chart's bundled subcharts are the easiest example.
- **Lockdown is a feature, not a bug.** The "Update WordPress" buttons are absent on purpose. Frame this clearly when it comes up — readers will assume it's a bug otherwise.
- **Internal links use the canonical path.** `/components/runtime` not `https://docs.frankenpress.com/components/runtime` (Mintlify rewrites correctly + makes preview deployments work).

## Don'ts

- **Don't reference Bitnami images by their `bitnami/<name>` path.** Always `bitnamilegacy/<name>`. Bitnami withdrew the public `bitnami/*` images in 2025.
- **Don't recommend Argo CD / Kargo / Flux as required.** The chart is GitOps-agnostic. Mention them as compatible options, not prerequisites.
- **Don't link to the Bitnami "Secure Images" commercial subscription as the canonical install path.** It's the workaround we don't use.
- **Don't write multi-paragraph "introduction" sections.** Two sentences max, then jump to the actionable content.
- **Don't add Mintlify starter-kit-style copy** ("Welcome to your new docs!", "Make it yours", etc.). The starter copy was deleted on purpose; don't bring it back.
- **Don't add an `agent-ready` / `ai-tools` / `essentials` Mintlify-starter group back to the navigation.** Their `.mdx` files are still in the repo but de-referenced — feel free to delete in a cleanup PR if you want.

## Quickstart smoke test

The kind-cluster quickstart is **smoke-tested** on every release:

1. Create a fresh kind cluster (`kind create cluster --name <name>`)
2. Run the steps from `quickstart.mdx` verbatim
3. Verify the WP admin loads, an upload lands in MinIO, and the lockdown buttons are absent

If a doc step fails when run verbatim, treat it as a release blocker.
The Mintlify preview is `mint dev` from the repo root after `npm i -g mint`.

## When you change a public env var or values key in another repo

If anyone bumps or renames anything in `runtime`, `mu-plugin`,
`site-template`, or `charts` that's documented here, update:

1. The component-specific page (`/components/<name>`)
2. `/operations/configuration` (the env-var reference)
3. The relevant section of `quickstart.mdx` if it changed the user-facing flow

## Local development

```bash
npm i -g mint     # one-time
mint dev          # serves at http://localhost:3000
```

## Companion repos

| Repo | Purpose |
|---|---|
| [`runtime`](https://github.com/frankenpress/runtime) | Base container image |
| [`mu-plugin`](https://github.com/frankenpress/mu-plugin) | Must-use plugin |
| [`site-template`](https://github.com/frankenpress/site-template) | GitHub template for new sites |
| [`charts`](https://github.com/frankenpress/charts) | Helm chart `site` |
| [`docs`](https://github.com/frankenpress/docs) (this repo) | Mintlify docs |
