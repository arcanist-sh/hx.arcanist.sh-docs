# hx.arcanist.sh-docs

The documentation for [hx](https://github.com/arcanist-sh/hx), the whole Haskell toolchain in one
fast binary, served at **[docs.arcanist.sh/hx](https://docs.arcanist.sh/hx/)**: installation and the
quick start, every command, `hx.toml` and lockfiles, workspaces, compiler backends, guides for moving
from Cabal or Stack, and working with AI agents.

The product page, benchmarks and blog are on [arcanist.sh/hx](https://arcanist.sh/hx/) (in the
arcanist.sh website repository).

## Working on it

Built with [Zola](https://www.getzola.org/) **0.22** (`mise install` sets it up; newer Zola releases
reject the config).

```bash
zola serve    # http://127.0.0.1:1111
zola build
```

```
config.toml          site config: nav, footer and lockup = "hx"
content/docs/        the pages, by section: commands, configuration, features, concepts, guides
templates/shortcodes/ hx_version and other shortcodes
themes/purple-haze/  the docs theme, shared with bhc.arcanist.sh-docs (keep the two copies identical)
```

Commands and flags should match the current hx release; check them against `hx --help` and
`crates/hx-cli/src/cli.rs` in the hx repository.

## How it is published

A push to `main` triggers the [docs.arcanist.sh](https://docs.arcanist.sh) build, which clones this
repository and builds it under `/hx/`.
