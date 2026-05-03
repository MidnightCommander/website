# Midnight Commander's website

This website is made with [ProperDocs](https://properdocs.org) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

```shell
$ uv sync --all-extras --dev
$ uv run properdocs serve
```

## Redirects

The following redirects have been implemented in `properdocs.yml`:

* All basic Trac pages (`wiki`, `browser`, `log`, etc.)
* Ticket redirects (`ticket/*`, statically generated)

The following redirects are probably not worth the effort:

* `attachment/*` / `raw-attachment/*`, see the [trac-archive repository](https://github.com/MidnightCommander/trac-archive)
* `milestone/*`, see the [mc repository](https://github.com/MidnightCommander/mc/milestones)
