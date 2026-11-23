# site-generator-speed entry

A faster drop-in for makefun-site 1.0.0, written from scratch: `solve.js`
exports `run(input)`, returning exactly the output tree the reference build
returns for the same input, byte for byte. `main.ts` is the harness driver —
it reads `/input/cases.json`, answers each case alone with `run`, and writes
`/output/answers.json`.

## Run

```sh
node main.ts
```

Outside the grading container, `MAKEFUN_INPUT_DIR` and `MAKEFUN_OUTPUT_DIR`
stand in for `/input` and `/output`.

## How it is faster

The reference pipeline is reimplemented as one tight pass: Markdown is lexed
and rendered in a single scan with range-based inline rendering (no token
arrays, no per-character regex escapes), slug results are cached per build,
front matter is parsed without splitting the whole body, and templates are
rendered through a frame-stack walker that shares one output buffer. The
semantics — front matter, template dialect, Markdown subset, slug rule and
report — are the reference's own, verified by differential testing against
the kit's reference on the practice set and on freshly generated sites.

## Licence

MIT — see `LICENSE`.
