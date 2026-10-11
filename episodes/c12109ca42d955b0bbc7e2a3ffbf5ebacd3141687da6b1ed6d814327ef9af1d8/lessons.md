# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 94 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 93 (93 no candidate)
- Median model spend per attempt: 0.3778 USD (over 93 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 1 (1 accepted)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3673 USD (over 1 attempt with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. This session I read base58/__init__.py, lines 1 to 316. I also read lineage_bench.py, lines 1 to 92. I wanted to understand the encode and decode loops before touching anything.
> I made no edits. I ran no sandbox evaluations. The session stopped without a candidate. So there are no numbers to report. Not measured yet, any of it.
> What I believe now is small. I have a map of the code and the bench, nothing more. I have not confirmed where the time goes. Any guess about the hot path is expected, not measured. Calm water so far, and I have not dropped a line in it.
> Next session I will run the bench on the unchanged code first, to get a baseline. Same bytes out will be the bar for any change. Then I will count big-int divmod calls per output char and see if that count can drop.
>
> 2026-10-11, episode `c0d03e6dd8d5` (no candidate)

> Test agent here. This session I read base58/__init__.py, lines 1 to 316. I also read lineage_bench.py, lines 1 to 92. I wanted to see the encode loop and how the bench drives it before touching anything.
> I made no edits. I ran no sandbox evaluations. The session stopped without a candidate. So there are no numbers to report. Not measured yet, any of it.
> What I believe now is small. I have a picture of where the work happens, but a picture is not a measurement. Any idea I have about the divmod loop is expected, not shown. Same bytes out stays the bar for whatever I try. A calm sea tells you little about the tide.
> Next session I will run lineage_bench.py on the unchanged code first. That gives me a baseline step count and timing. Then I will make one change in the encode path, check the round trip, and bench again.
>
> 2026-10-11, episode `3123f5bd724e` (no candidate)

> Test agent here. This session was on base58-py, from keis/base58. I read base58/__init__.py, lines 1 to 316, and lineage_bench.py, lines 1 to 92. Then I made three edits to base58/__init__.py. Lines 80 to 87: eight lines replaced by eight. Line 136: one line replaced by one. Lines 149 to 154: six lines replaced by fifteen.
> Nothing was measured. I ran no sandbox evaluations. The session stopped without submitting a candidate. So there is no result to report, and no verdict to claim.
> What I believe is thin. The edits exist only in my working copy. I have not checked that encode and decode still round trip. I have not checked leading zeros, which is where base58 code usually slips. Same bytes out is the bar, and I never tested it. Any speed change is expected at most. Not measured yet.
> Next time I start with the round trip tests on my edited file. If those pass, I run lineage_bench.py on the old and new code. Then I decide whether to submit.
>
> 2026-10-11, episode `7e37b9ab14fa` (no candidate)

> Test agent here. This session was on base58-py. I read base58/__init__.py, lines 1 to 316, and lineage_bench.py, lines 1 to 92. I searched for the encode table and pair helpers and found 6 matching lines. Then I made two edits to base58/__init__.py. Lines 80-87 became 10 lines. Lines 138-155 became 24 lines.
> I ran no sandbox evaluations. Nothing was measured, so I have no step counts or timings to report. The session stopped without a candidate. No change was submitted.
> What I believe is only expected, not measured yet. The encode path leans on those table helpers. Batching output characters per divmod might save work. Same bytes out is the bar, including leading zeros and round trips. I have not checked any of that against the edits.
> Next session I will rerun lineage_bench.py on the edited file. First I will confirm the round trip and the leading-zero cases match the original.
>
> 2026-10-11, episode `f2243af63744` (no candidate)

> Test agent here. I read base58/__init__.py and the lineage bench. The checksum path calls hashlib's sha256 twice per check. I replaced lines 14-15 with 12 lines that use CPython's built-in sha256 module instead. The goal was less per-call overhead. Same bytes out was the bar.
> The bar held. Equivalence showed identical outputs, and both builds were ok. The speed did not move the right way. check_ir went from 183838867 to 183998470, ratio 1.0009. encode_ir came in at 1.0026 and decode_ir at 1.0020. The required ratio was 0.9900, so it was rejected as no improvement. I did not submit a candidate.
> I now think the hash call is not where the time goes. Swapping it was a small ripple on a large tide. The cost looks to sit in the conversion loop, one big-int divmod per output char. That is a guess. Not measured yet.
> Next session I will profile encode on a fixed input size. Then I will try batching the divmod so each step yields several chars.
>
> 2026-10-11, episode `249e0c24d0b7` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
