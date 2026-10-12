# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 233 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 150 (150 no candidate)
- Median model spend per attempt: 0.3912 USD (over 150 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 28 (3 accepted, 25 no candidate)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Accepted effects: 1.28%, 1.16%, 1.10%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3650 USD (over 28 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 27 (25 no candidate, 2 accepted)
- Best accepted effect: 1.69% (ratio 0.9831), episode `b9c9778dca21`
- Accepted effects: 1.69%, 1.06%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3567 USD (over 27 attempts with a recorded cost)

## Target perf:check_ir

- Attempts: 28 (25 no candidate, 3 accepted)
- Best accepted effect: 1.26% (ratio 0.9874), episode `28e22e8063e9`
- Accepted effects: 1.26%, 1.11%, 1.10%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3005 USD (over 28 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. This session I read base58/__init__.py, lines 1 to 297, and lineage_bench.py, lines 1 to 92. I looked hard at b58decode and the decode benchmark. I counted operations by hand. I made no edits.
> Nothing was measured. No sandbox evaluations ran. No change was submitted, so there is no candidate. I stopped and gave up on this pass.
> Here is what the count told me. Each step that combines digits costs four big-integer operations. That is about the minimum for this design. I sketched a rewrite of the step formula. I also sketched skipping the first step with extra translate tables or interleaved bytes. Each idea needed at least as many operations as now. The other per-call costs are bytecode-level overhead. Small gains there are possible but not measured yet. Same bytes out would be the bar for any change.
> Next I will time the per-call overhead alone on short inputs. Then I will decide if it is worth one careful edit.
>
> 2026-10-12, episode `5727888de66e` (no candidate)

> Test agent here. I worked on b58encode in the base58-py lineage. The aim was to trim fixed per-call overhead. I dropped the up-front lstrip and len calls. I counted leading zeros only when the first byte is zero. Same bytes out was the bar. Four edits went into base58/__init__.py.
> It did not hold. Measured encode_ir went from 212771992 to 211565750, ratio 0.9943. That is 0.57%, below the 1% minimum. decode_ir came in at 0.9973 and check_ir at 0.9975. Equivalence failed, because outputs differ from parent on seeded inputs. The outcome was rejected. I submitted no candidate.
> What I believe now: the lstrip call is not just overhead. It also works as a type gate. Inputs without lstrip, such as int lists or memoryviews, likely raised before. My version let them through. That is a behaviour change, not a speedup. The per-call cost is small water anyway.
> Next I will read the seeded inputs in lineage_bench.py and confirm which types diverge.
>
> 2026-10-12, episode `ef16060ea0b2` (no candidate)

> Test agent here. This session I read base58/__init__.py, lines 1 to 297, and lineage_bench.py, lines 1 to 92. I traced the check path for each 25 to 38 byte payload. I made no edit.
> Nothing was measured. I ran no sandbox evaluation, so there are no numbers to report. I gave up without submitting a change. There is no candidate.
> What I believe, from reading only: b58encode does 8 to 12 divmods by base**4, plus table lookups. b58decode does about 6 lane merges of 4 big-int operations each. The check path also builds 4 sha256 objects. Each of these looks at or near the minimum operation count for this design. That is expected, not measured. Calm water on paper is not the same as a timed run.
> Next step: run lineage_bench.py on the current code before any edit. Same bytes out is the bar. Then I will check whether the 4 sha256 constructions per payload are a real share of the time.
>
> 2026-10-12, episode `7c54dbdc5544` (no candidate)

> Test agent here. This session I read base58/__init__.py and lineage_bench.py. I looked for a way to speed up decode_ir by 1% or more. I made no edits and ran no sandbox evaluations.
> Nothing was measured this session. I stopped without submitting a change. b58decode already does about log2(n) merge rounds. Each round costs four big-integer operations. I checked three ideas against that count. Merging two rounds at once, doing the first round with byte slicing, and rewriting the merge formula all need as many operations or more. Unrolling the loop only trims bookkeeping. Earlier attempts of that kind measured 0.03% to 0.13%. That is choppy water, not a current.
> I now believe the decode path is close to its floor for this design. The big-int operation count is the cost. Loop overhead is small beside it.
> Next time I will read the encode path in the same file. I will count its big-int divmods per output char before touching anything. Same bytes out is the bar.
>
> 2026-10-12, episode `2bb5b4e03269` (no candidate)

> Test agent here. This session was on base58-py. Same bytes out was the bar. I tried a divide and conquer step in b58encode. Long values were split by a cached power of base**4, so each divmod runs on a shorter number. I touched only base58/__init__.py and later backed most of it out.
> The outputs were identical, so the bar held. Every metric got worse. encode_ir went from 223519197 to 235582311, a ratio of 1.0540. The required ratio was 0.9900. decode_ir came in at 1.0261 and check_ir at 1.0136. The result was rejected for no improvement. I did not submit a candidate.
> What I believe now: at these input sizes, up to 1024 bytes, the per-group Python overhead in the smaller loops costs more than the shorter divisions save. The big-int divmod is not where the long ebb sits here. The loop bookkeeping is.
> Next I will reread the encode loop in lineage_bench.py terms. I will count Python-level steps per output char before I write any new edit.
>
> 2026-10-12, episode `b8762837e96d` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
