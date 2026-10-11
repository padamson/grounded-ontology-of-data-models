# GODM — a grounded ontology of data models

GODM models data models (OWL ontologies, LinkML schemas, JSON Schemas,
SHACL shapes graphs and relational schemas), their parts, the relations
between models, and the datasets that conform to them, so that
data-modeling tools and schemas share one vocabulary for what they
describe.

It is a [LinkML](https://linkml.io) ontology grounded in BFO 2020
(ISO/IEC 21838-2:2020) and the Common Core Ontologies (CCO), authored via
Noy & McGuinness's *Ontology Development 101* (N&M) method, with the build
documented chapter by chapter as an mdbook, *Building GODM*. Prefix
`godm`, namespace `https://w3id.org/godm/`.

## Authoring the ontology

Work one N&M step at a time with the
**`ontology-authoring-advance-step`** skill (*"do Step 1"*), a managed
install pinned in `skills-lock.json` alongside `mdbook-listings` and
`panschema-development` (see `CLAUDE.md` "Agent skills"): write
`book/src/introduction.md`, then `book/src/ch01`…
`ch07`, so the rendered Chapter N is N&M Step N. Each step advances
`schema/godm.yaml` only as its worked example demands and freezes a
listing snapshot the chapter embeds. Grounding is by URI — reference reuse
vocabularies as prefixes, never LinkML `imports:`.

## Building the book

```
cd book && mdbook serve   # local preview with live reload
cd book && mdbook build   # output to book/build/
./scripts/build-book.sh   # the CI gate: mdbook build, failing on a logged ERROR
```

Preprocessors must be on `PATH`: `mdbook-listings`, the `mdbook-admonish`
fork, and `mdbook-panschema` — `scripts/install-assets.sh` installs all
three. Also once after cloning: `panschema fetch`, which downloads the
cqa contract (the schema the competency-question benchmark conforms to)
at the release `panschema.lock` pins, after which `panschema verify` /
`generate` and the pre-commit hook run offline. CI
(`.github/workflows/docs.yml`) checks the lockfile, verifies the
instance data, generates the machine-readable formats (via
`panschema.toml`), builds the book, and deploys the combined site to
Pages.

## Layout

```
schema/
  godm.yaml               # source of truth (LinkML)
data/
  godm-benchmark.yaml     # the competency questions as cqa benchmark records (Step 1)
book/                     # mdbook documenting the N&M build
scripts/                  # dev loop, rebuild, line-width + schema-path helpers
panschema.toml            # panschema manifest: generate (ttl/shacl/...) + check gates
panschema.lock            # the cqa contract's resolved checksum (written by panschema fetch)
panschema-publish.toml    # panschema's release + publish manifest
.github/workflows/docs.yml
```

The schema path is read from `panschema-publish.toml` (`[files].main`), so
CI, the pre-commit hooks, and the scripts never hardcode the schema
filename.

## License

The GODM schema and the book are licensed under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (`LICENSE`). The
template scaffolding this repo was built from is Apache-2.0
(`LICENSE-APACHE`).
