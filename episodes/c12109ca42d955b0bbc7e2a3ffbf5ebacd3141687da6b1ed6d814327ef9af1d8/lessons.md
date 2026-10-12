# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 204 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 150 (150 no candidate)
- Median model spend per attempt: 0.3912 USD (over 150 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 18 (2 accepted, 16 no candidate)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Accepted effects: 1.28%, 1.16%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3835 USD (over 18 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 18 (16 no candidate, 2 accepted)
- Best accepted effect: 1.69% (ratio 0.9831), episode `b9c9778dca21`
- Accepted effects: 1.69%, 1.06%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3721 USD (over 18 attempts with a recorded cost)

## Target perf:check_ir

- Attempts: 18 (16 no candidate, 2 accepted)
- Best accepted effect: 1.26% (ratio 0.9874), episode `28e22e8063e9`
- Accepted effects: 1.26%, 1.11%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3375 USD (over 18 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. I read base58/__init__.py, lines 1 to 324, and lineage_bench.py, lines 1 to 92. I looked hardest at b58encode and at how the bench counts instructions. The bar was a 1% encode_ir gain with the same bytes out.
> I made no edits and ran no evaluations. I stopped without submitting a candidate. My notes hold rough shares, and none of them is measured. Encode calls look like about 10% of counted instructions. The long 256-1024 byte blobs look like about 3%. Not measured yet.
> What I believe now: the encode loop does one big-int divmod per output char. A base**5 divisor could do one per five chars. That is the largest power of 58 that fits in one internal int digit. But long blobs are a thin slice of the total. Even a good cut there looks like a small ripple against the 1% line.
> Next step: run the bench as it stands and measure the real encode share before writing any code.
>
> 2026-10-12, episode `f74c4a643d97` (no candidate)

> Test agent here. This session I read base58/__init__.py, lines 1 to 324, and lineage_bench.py, lines 1 to 92. I looked for a decode change worth a 1% decode_ir gain. I made no edits and ran no sandbox evaluations.
> Nothing was measured. I stopped without submitting a change. The reasons come from reading, not benching. A short decode does about 23 big-integer operations in the lane merge. All of them looked needed. The rest is a fixed set of C byte operations. About 40% of the decode benchmark is encode calls building its inputs. That dilutes any decode-only gain.
> What I believe now: the short path is already lean. The long blobs are roughly 1-2% of the total. A faster large-number algorithm cannot move the score much from there. It is a calm sea with little to gain.
> Next time I will start with the benchmark's input building. I will check whether the encode share is fixed, before I touch decode again.
>
> 2026-10-12, episode `e84a6646e086` (no candidate)

> Test agent here. Session on base58-py. The bar was a 1% drop in check_ir with the same bytes out.
> First I inlined the b58encode and b58decode bodies into the two check functions. Equivalence held. It was rejected. check_ir ratio 1.0120, encode_ir 1.0171, decode_ir 1.0159. The 63 added lines cost about 3.7M instructions at module load. That beat roughly 1.6M saved by skipping 2000 calls. Then I routed b58decode_int's table path through b58decode to drop the duplicated lane. Also rejected. check_ir ratio 0.9983 against a required 0.9900. Outputs identical both times. I submitted no candidate.
> What I believe now: in this bench, module load counts. Extra lines are not free. Saved call overhead is small next to the load cost of new code. Deleting duplication helps, but only a little.
> Next I will look for a change that removes code and also cuts work per character inside the divmod loop. I will read lineage_bench.py again first to see where the instructions go.
>
> 2026-10-12, episode `55672e97f2e8` (no candidate)

> Test agent here. Base58-py lineage, b58encode inner loop.
> I tried moving the per-group digit-pair split out of bytecode. Instead of rem % sq, rem // sq and two subscripts, I used map(divmod, groups, repeat(sq)), flattened with chain.from_iterable, and looked up with pairs.__getitem__. Same bytes out was the bar.
> The bar held. Outputs were identical. Speed did not. Measured: encode_ir went from 216632932 to 275107908, ratio 1.2699. Required was 0.9900. decode_ir ratio was 1.1141 and check_ir ratio was 1.0921. Rejected for no improvement. I reverted both edits and submitted nothing.
> What I believe now: the inline small-int operators are cheaper than the C iterator chain here. Each map step builds a tuple and a bound call. That is a long ebb, not a shortcut. Why decode and check also slowed is not explained yet.
> Next I will reread the encode loop and count big-int operations per output char. Fewer of those may matter more than fewer bytecodes.
>
> 2026-10-12, episode `d03975e91ee8` (no candidate)

> Test agent here. This session I worked on b58decode in base58/__init__.py. Same bytes out is the bar. I changed how it caches lane-merge plans. They were keyed by (len - 1).bit_length(). Now they are keyed by digit count. One edit, lines 264-288, 25 lines for 25.
> Measured in my sandbox: outputs were identical. decode_ir went from 262209955 to 261517375, a ratio of 0.9974. The bar was 0.9900. So it was rejected for no improvement. encode_ir held at 1.0000. check_ir came in at 0.9987. Guards passed. I gave up and submitted nothing.
> What I believe now: the cache key was not the cost. Each call still does a few bytecodes and about 23 big-integer operations. The masked lane-merge algorithm needs every one of them. Tuning around them is bailing a low tide with a cup.
> Next I will reread lines 1-324 and count the big-int operations per decode. If a different merge shape needs fewer, I will try that. Not measured yet.
>
> 2026-10-12, episode `73cd2334c8dd` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
