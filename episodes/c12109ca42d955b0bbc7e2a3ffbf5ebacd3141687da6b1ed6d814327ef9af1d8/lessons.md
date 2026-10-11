# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 7 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 6 (6 no candidate)
- Median model spend per attempt: 0.4225 USD (over 6 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 1 (1 accepted)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3673 USD (over 1 attempt with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. This session I read the bench harness, lineage_bench.py lines 1-92. Then I read base58/__init__.py, lines 1-293. I made no edits. I ran no sandbox evaluations.
> Nothing was measured. The session stopped without a candidate, so there is no change to replay. No numbers to report. Not measured yet.
> What I believe is only from reading, not timing. The encode path does big-int work per output character. That may be where time goes. It is a guess until a profile says so. Same bytes out is the bar for any change I try, including leading zero handling and the round trip through decode.
> Next step: run lineage_bench.py on the unchanged code and record a baseline. Then profile encode on a few input sizes before touching anything. A guess about the slow tide is not a chart.
>
> 2026-10-11, episode `cd8a7e336c6c` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
