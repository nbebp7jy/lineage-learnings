# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 217 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 150 (150 no candidate)
- Median model spend per attempt: 0.3912 USD (over 150 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 23 (3 accepted, 20 no candidate)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Accepted effects: 1.28%, 1.16%, 1.10%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3673 USD (over 23 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 21 (19 no candidate, 2 accepted)
- Best accepted effect: 1.69% (ratio 0.9831), episode `b9c9778dca21`
- Accepted effects: 1.69%, 1.06%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3663 USD (over 21 attempts with a recorded cost)

## Target perf:check_ir

- Attempts: 23 (20 no candidate, 3 accepted)
- Best accepted effect: 1.26% (ratio 0.9874), episode `28e22e8063e9`
- Accepted effects: 1.26%, 1.11%, 1.10%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3372 USD (over 23 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. This session I read base58/__init__.py, lines 1 to 297, and lineage_bench.py, lines 1 to 92. I looked for a change to encode that could clear the 1.0% bar on encode_ir. I made no edits and ran no sandbox evaluations.
> Nothing was measured. I stopped without submitting a candidate. Everything below comes from reading only. Not measured yet.
> What I believe, as estimates: the encode calls are roughly 40M of the ~207M instructions. The rest is mostly startup and building the inputs. Each four-character group already costs one single-digit divmod, two small-integer operations and two lookups. That is a short loop with little left to cut. I considered a base**3 table and larger divisors. On paper neither looked likely to clear the bar. The water here is already low.
> Next I will run the bench as it stands and split startup from encode cost. Then I will know whether 1.0% of the total is reachable at all.
>
> 2026-10-12, episode `8263dd7a051f` (no candidate)

> Test agent here. I read base58/__init__.py, lines 1 to 297, and lineage_bench.py, lines 1 to 92. I followed the check path end to end. I made no edits and ran no sandbox evaluations.
> Nothing was measured this session. The bar was a change likely to clear 1.0% on check_ir. I did not find one, so I stopped without a candidate. My rough estimates say most of the metric is fixed cost: interpreter startup, imports and module load. Not measured yet.
> What I believe, and it is belief, not data: the per-call work is already short for 21-34 byte payloads. About 10 single-digit divmods per encode. About 6 merge steps per decode. Unrolling the merge loop looks like under 1%. Chasing that is rowing against the tide.
> Next, I will measure the fixed cost directly. I will time a bare interpreter start and a bare import of base58, then compare both to the full check_ir run. If startup dominates, I will look at module load before touching any loop.
>
> 2026-10-12, episode `935cb1832fbd` (no candidate)

> Test agent here. I read base58/__init__.py, lines 1-297, and lineage_bench.py, lines 1-92. I looked for a decode_ir change that could clear the 1.0% bar. A gain under that bar does not count. I made no edits and ran no evaluations.
> Nothing was measured this session. No candidate was submitted. Reading the bench showed me where the time goes. Most of decode_ir is fixed cost: Python startup, plus the encode setup the benchmark runs. The decode calls are a small slice of the total. Trimming them is bailing a boat in a rising tide.
> What I believe now: decode is not the bottleneck inside this metric. Two ideas remain. First, unroll the merge loop. My rough count is ~0.5%. Not measured yet, and it sits below the bar. Second, multiply by base**h instead of 256**h - base**h. That likely helps long inputs and hurts the short 32/64-byte cases. Not measured yet.
> Next step: profile one decode_ir run and split fixed cost from decode cost before I touch any code.
>
> 2026-10-12, episode `49f8f05f54ea` (no candidate)

> Test agent here. I read base58/__init__.py and the bench input sizes. Then I tried one idea in the encoder. Peel five base58 digits per big-int divmod instead of one. That only kicked in for inputs of at least 120 digits. Base**5 still fits in one internal int digit, so each division stays cheap. Same bytes out was the bar.
> Outputs were identical. The numbers went the wrong way. encode_ir went from 211895488 to 217193514, ratio 1.0250. The bar was 0.9900. decode_ir rose to ratio 1.0138 and check_ir to 1.0108. Rejected for no improvement. I reverted the edit and submitted nothing.
> What I believe now. Fewer division passes did not pay for the extra work around them. The added table entries, the per-group character lookups and the larger function body cost more than they saved. Decode and check rose too, and they never touched the new path. So the larger body likely costs something on its own. Not measured yet.
> Next I will count instructions per output char in the original loop before changing anything.
>
> 2026-10-12, episode `ff72a038b580` (no candidate)

> Test agent here. I worked on b58encode in base58/__init__.py. Same bytes out was the bar. The loop now peels eight base**4 groups per pass while eight or more remain. Then it takes at most one four-group step before the old two-or-three-group tail. I also moved the zero-character prefix handling.
> Evaluation 1 used 26 lines and was rejected. encode_ir went from 209451760 to 208487006, a ratio of 0.9954 against a required 0.9900. Evaluation 3 used 31 lines and was accepted. encode_ir went from 209451766 to 207275378, a ratio of 0.9896. decode_ir sat at 0.9985 and check_ir at 1.0021. Outputs were identical both times. These are my own sandbox numbers, not a verdict.
> The gain is small and close to the line. I expect a 32-byte input to need one pass instead of two. Not measured per size yet.
> The candidate is submitted and queued for independent replays. Next I will read the replay ratios and check whether check_ir drifts past 1.0021.
>
> 2026-10-12, episode `49628bb9453d` (accepted)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
