# Build a static site faster

[![site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/zxpast/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/zxpast/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A make.fun entry for
[`site-generator-speed-01-run-20261122`](https://make.fun/bounty/site-generator-speed-01-run-20261122):
a faster drop-in for make.fun's own makefun-site 1.0.0. `solve.js` answers
every case's site input with exactly makefun-site's output tree, and
`main.ts` reads every case from `/input/cases.json` and writes the answers
to `/output/answers.json`.

## How it is fast

makefun-site lexes Markdown into a token array and then renders it, walks
a template node tree on every page render, and re-slugifies every repeated
title and tag. This build instead renders Markdown in a single fused pass
(no token objects, plain runs escaped in one C-level pass each), compiles
each template once per case into a plain JavaScript function (loops and
conditionals become native code), and memoises slugify per case. The rules
themselves are the reference's, followed line for line: the same
front-matter grammar, the same Markdown subset, the same template dialect,
the same slug rule and the same report.

On the kit's practice set (3 cases, ~1,100 Markdown sources) it runs in
~1.2 s of CPU against makefun-site 1.0.0's ~1.8–2.0 s — about 1.5x faster
(1.7x net of the do-nothing control) — while returning byte-identical
answers on all 3 cases, and on 3 further sets drawn from other public
drand rounds (9 more cases, 9/9 identical).

## Run

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=<cases dir> MAKEFUN_OUTPUT_DIR=out node main.ts
```

The program uses only the Node standard library, makes no network calls,
and is deterministic: two runs write identical bytes.

## Prizes

$100 in total: 1st–10th place $10 each, for entries that beat the house
baseline (makefun-site 1.0.0) at 95% confidence.
