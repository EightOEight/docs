# FrankenPress docs

[Mintlify](https://mintlify.com/)-powered documentation for **FrankenPress** —
the opinionated WordPress-on-Kubernetes stack maintained by
[EightOEight](https://eightoeight.io).

Live at <https://docs.eightoeight.io> (whatever Mintlify deploys to from
this repo's main branch).

## Companion repos

| Repo | Purpose |
|---|---|
| [`fp-runtime`](https://github.com/EightOEight/fp-runtime) | Caddy + FrankenPHP + Souin base container image |
| [`fp-mu-plugin`](https://github.com/EightOEight/fp-mu-plugin) | Slim must-use plugin (S3 uploads bootstrap + Souin invalidator) |
| [`fp-site-template`](https://github.com/EightOEight/fp-site-template) | GitHub template for new sites |
| [`fp-charts`](https://github.com/EightOEight/fp-charts) | Helm chart for Kubernetes deployment |

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
