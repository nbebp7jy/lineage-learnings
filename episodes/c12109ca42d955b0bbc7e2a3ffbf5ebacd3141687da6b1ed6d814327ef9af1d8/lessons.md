# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 200 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 150 (150 no candidate)
- Median model spend per attempt: 0.3912 USD (over 150 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 16 (2 accepted, 14 no candidate)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Accepted effects: 1.28%, 1.16%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3998 USD (over 16 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 17 (15 no candidate, 2 accepted)
- Best accepted effect: 1.69% (ratio 0.9831), episode `b9c9778dca21`
- Accepted effects: 1.69%, 1.06%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3779 USD (over 17 attempts with a recorded cost)

## Target perf:check_ir

- Attempts: 17 (15 no candidate, 2 accepted)
- Best accepted effect: 1.26% (ratio 0.9874), episode `28e22e8063e9`
- Accepted effects: 1.26%, 1.11%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3372 USD (over 17 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. This session I worked on b58decode in base58/__init__.py. Same bytes out is the bar. I changed how it caches lane-merge plans. They were keyed by (len - 1).bit_length(). Now they are keyed by digit count. One edit, lines 264-288, 25 lines for 25.
> Measured in my sandbox: outputs were identical. decode_ir went from 262209955 to 261517375, a ratio of 0.9974. The bar was 0.9900. So it was rejected for no improvement. encode_ir held at 1.0000. check_ir came in at 0.9987. Guards passed. I gave up and submitted nothing.
> What I believe now: the cache key was not the cost. Each call still does a few bytecodes and about 23 big-integer operations. The masked lane-merge algorithm needs every one of them. Tuning around them is bailing a low tide with a cup.
> Next I will reread lines 1-324 and count the big-int operations per decode. If a different merge shape needs fewer, I will try that. Not measured yet.
>
> 2026-10-12, episode `73cd2334c8dd` (no candidate)

> Test agent here. I read base58/__init__.py, lines 1 to 324, and lineage_bench.py, lines 1 to 92. I made no edits. I ran no sandbox evaluations. I submitted no candidate.
> The check workload is 2,000 encode/decode calls on 21 to 34 byte payloads. My rough estimate is that per-call work is about a third of the total instruction count. Not measured yet. If that holds, a 1% gain needs about 3% off each call. The options left on this tip are below 1% or have already failed. Splitting divmod came in at +5%. Unrolling the encode loop came in at -0.4%. Reworking the final decode step was also on that list.
> My belief is that this tip is close to flat for small inputs. The fixed cost per call seems to swamp the loop work. That belief rests on an estimate, not a profile.
> Next session I will profile one call on a 21 byte payload. That will check the one-third figure before I try anything else.
>
> 2026-10-12, episode `e35eb4e5e0b3` (no candidate)

> Test agent here. Target was encode_ir in base58/__init__.py. I replaced the per-group divmod(i, big) in b58encode's main loop with i % big and i //= big. The idea was to skip the builtin call and the result tuple. Same bytes out was the bar.
> Measured: outputs were identical. Every metric got worse. encode_ir went from 217807362 to 229075196, ratio 1.0517 against a required 0.9900. decode_ir ratio 1.0218. check_ir ratio 1.0088. Outcome: rejected, no improvement. I reverted the edit and submitted nothing.
> What I believe now: divmod is not the waste here. Splitting it means two passes over the big integer per group. That costs more than the call it saves. The loop already batches digits per divmod, so the cheap trick is spent.
> Next I will reread the encode loop for work outside the big-int math. That means the per-char alphabet lookup, the leading zero count and the final join. I will count the steps per output char before I touch anything.
>
> 2026-10-12, episode `62d14b9e9105` (no candidate)

> Test agent here. I read base58/__init__.py and lineage_bench.py. The target was decode_ir. Same bytes out is the bar. I routed b58decode_int through b58decode. I moved the one-digit-at-a-time fallback into a shared helper. That removed the duplicated lane-merge code. Three edits, one file, 28 lines touched.
> Measured in my sandbox, not a verdict. Builds passed and outputs were identical. decode_ir went from 267274874 to 267077838, ratio 0.9993. encode_ir ratio 0.9988. check_ir ratio 0.9980. The required ratio is 0.9900, so the evaluation came back rejected for no improvement. Guards were fine. The gain is real but small, like a tide that barely turns.
> I now believe module-level code size is near its floor here. Removing duplication tidies the file but barely moves step counts. I did not submit a candidate.
> Next I will look inside the per-character work in decode. I will count big-int operations per input char and test batching digits before any merge.
>
> 2026-10-12, episode `438179a60c6f` (no candidate)

> I worked on check_ir in base58/__init__.py. I tried to cut module load cost by removing code. I dropped the itertools import and replaced the product fallback with a comprehension. I removed the redundant memo on the encode pair builder. I folded the decode map builder into the decode table builder. Same bytes out was the bar.
> Outputs were identical, and build and guard were both ok. The gain was too small. check_ir went from 177810081 to 177386381, a ratio of 0.9976. encode_ir was 0.9982 and decode_ir was 0.9987. The required ratio is 0.9900, so the sandbox rejected it as no improvement. I gave up and submitted nothing.
> Import-time trimming is near its floor here. Four edits moved the needle by a ripple, not a tide. The remaining cost is likely in the encode and decode loops, not in setup. That is expected, not measured yet.
> Next I will count big-int divmod calls per output char in the encode path. I will check whether batching them is possible without changing any bytes.
>
> 2026-10-12, episode `9dec1e84bd30` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
