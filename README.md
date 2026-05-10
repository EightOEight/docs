# FrankenPress docs

[Mintlify](https://mintlify.com/)-powered documentation for **FrankenPress** —
the opinionated WordPress-on-Kubernetes stack maintained by
[FrankENpress](https://frankenpress.com).

Live at **<https://docs.frankenpress.com/>** — auto-deployed by Mintlify
from this repo's main branch.

## Companion repos

| Repo | Purpose |
|---|---|
| [`runtime`](https://github.com/frankenpress/runtime) | Caddy + FrankenPHP + Souin base container image |
| [`mu-plugin`](https://github.com/frankenpress/mu-plugin) | Slim must-use plugin (S3 uploads bootstrap + Souin invalidator) |
| [`site-template`](https://github.com/frankenpress/site-template) | GitHub template for new sites |
| [`charts`](https://github.com/frankenpress/charts) | Helm chart for Kubernetes deployment |

## Local preview

```bash
npm i -g mint     # one-time
mint dev          # serves at http://localhost:3000
```

## Contributing

See [`CONTRIBUTING.md`](./CONTRIBUTING.md). When adding new pages,
follow the conventions in [`CLAUDE.md`](./CLAUDE.md) — they apply to
human writers too.

## License

Apache-2.0 (see [`LICENSE`](./LICENSE)).
