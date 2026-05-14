# CLAUDE.md — docs

Guidance for Claude Code (and other AI agents) when working in this docs
repo.

## What this repo is

[Mintlify](https://mintlify.com/)-powered documentation for **FrankenPress**:
the four-repo opinionated WordPress-on-Kubernetes stack maintained by
[FrankenPress](https://frankenpress.com).

Live at **<https://docs.frankenpress.com/>** — auto-deployed by Mintlify
from this repo's main branch.

## Mintlify essentials

- `docs.json` — config + sidebar navigation. Adding a page means adding it to a `pages: [...]` array under the appropriate `groups[]` entry under `navigation.tabs[].groups[]`.
- `*.mdx` — Markdown + JSX. Frontmatter (`title`, `description`, `icon`) controls the page header. **Use FontAwesome icon names — not Lucide.** Mintlify can resolve both in principle but defaults to FontAwesome, and Lucide-style names (anything with the `-round`, `-text`, `-circle-*` suffixes — e.g. `key-round`, `scroll-text`) render as missing icons in the sidebar. The Operations group shipped two of these in production before they were caught. If you want an icon that only exists in Lucide, render it locally with `mint dev` first to confirm it resolves.
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
- **Lockdown is a feature, not a bug — and it's gated, not hard-coded.** The "Update WordPress / plugins / themes" buttons are absent in-cluster on purpose. Frame this clearly when it comes up — readers will assume it's a bug otherwise. `DISALLOW_FILE_EDIT` / `DISALLOW_FILE_MODS` are gated on `KUBERNETES_SERVICE_HOST` (and a narrow `FP_ALLOW_FILE_MODS` opt-out used by the install Job during snapshot apply); out-of-cluster (docker-compose, bare local) both are `false` so designers can install evaluation plugins/themes locally and promote via `wp fp snapshot`. Don't describe the constants as "hard-coded" — `site-template/config/application.php` is the source of truth and contradicted that copy in the docs for a while.
- **Internal links use the canonical path.** `/components/runtime` not `https://docs.frankenpress.com/components/runtime` (Mintlify rewrites correctly + makes preview deployments work).
- **Bitnami `common.names.fullname` collapses when the release name contains the chart name as a substring.** Chart name is `site`. `helm install mysite charts/site` produces `Service: mysite`, `Secret: mysite-install`, `Secret: mysite-keys`, `ConfigMap: mysite-env` — no `-site-` suffix anywhere — because `mysite` contains `site`. `helm install myblog charts/site` produces `myblog-site`, `myblog-site-install`, etc. The quickstart's example release `mysite` is in the collapsing case; the abstract `<release>-site-*` placeholder elsewhere assumes the non-collapsing case (real tenants like `sts` / `eoe` don't contain `site`). When writing concrete examples, run `helm template <release> charts/site` to see what the rendered names actually are. The quickstart ships a `<Note>` explaining the rule.

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

## When you tag a new chart release

The pinned `--version X.Y.Z` appears in **four** pages and drifts silently
— it shipped at `0.7.0` while the chart was at `0.12.0`, which would have
hard-failed the quickstart smoke test on `helm install` because the old
version is no longer published. When you bump `charts/charts/site/Chart.yaml`,
grep for the previous pin and update every hit:

```bash
grep -rn -- '--version [0-9]' docs/ --include='*.mdx'
```

Expected hits: `quickstart.mdx`, `your-first-site.mdx`,
`components/charts.mdx`, `operations/upgrade.mdx`. Bump all four to
match `Chart.yaml.version`.

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
