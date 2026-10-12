# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 173 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 150 (150 no candidate)
- Median model spend per attempt: 0.3912 USD (over 150 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 7 (2 accepted, 5 no candidate)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Accepted effects: 1.28%, 1.16%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3627 USD (over 7 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 8 (6 no candidate, 2 accepted)
- Best accepted effect: 1.69% (ratio 0.9831), episode `b9c9778dca21`
- Accepted effects: 1.69%, 1.06%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.4115 USD (over 8 attempts with a recorded cost)

## Target perf:check_ir

- Attempts: 8 (6 no candidate, 2 accepted)
- Best accepted effect: 1.26% (ratio 0.9874), episode `28e22e8063e9`
- Accepted effects: 1.26%, 1.11%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3253 USD (over 8 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. This session I read base58/__init__.py, lines 1 to 324, and lineage_bench.py, lines 1 to 92. I looked for a change on the encode and decode hot paths. The bar was a 1% gain with the same bytes out.
> I made no edits and ran no sandbox evaluations. I submitted no candidate. I stopped without a change.
> What I believe now is from reading, not measuring. Decode already merges digit lanes in about log2(n) steps. Encode already gets four digits per single-digit division. My rough counts say the alternatives break even or cost more. Those were a 58**5 divisor, a 58**8 multi-digit divisor, and bigger lookup tables. Not measured yet. The tip looks like calm water. Small trims do not raise much of a wave here.
> Next I will bench the 58**5 divisor on small and large input sizes. That turns my rough count into a measured number before I drop the idea.
>
> 2026-10-12, episode `563301961eab` (no candidate)

> Test agent here. Session on base58-py, aiming at check_ir. Same bytes out was the bar. The pass mark was a ratio of 0.9900 or lower. I tried three changes. First, use the built-in _sha2 sha256 constructor for checksums, with hashlib as fallback. Second, also have b58decode_int hand ordinary alphabets to b58decode, dropping its duplicate lane-merge code. Third, a further edit stacked on those.
> All three builds passed. Outputs were identical and guards were ok. None cleared the bar. check_ir measured 0.9974, then 0.9966, then 0.9949. In the second run encode was 0.9990 and decode 0.9995. All three were rejected as no_improvement. I submitted nothing.
> What I believe now: the checksum path is not where check_ir spends its time. Shaving call overhead moves the ratio, but only a little. The cost likely sits in the big-int conversion. Not measured yet.
> Next: profile check_ir by step count before editing anything.
>
> 2026-10-12, episode `f70ab199ef0d` (no candidate)

> Test agent here. Base58-py lineage, target encode_ir. Same bytes out is the bar. I read base58/__init__.py and lineage_bench.py. I tried three changes. First, I folded the pairs-table builder into b58encode's cache-miss path and let b58decode_int reuse b58decode for ordinary alphabets. That dropped a decorated function and some duplicate lane-merge code. Later attempts added code back around lines 62 and 118.
> All three gave identical outputs. The first measured encode 0.9989, decode 0.9985, check 0.9978. Rejected, since 0.9900 was required. The third measured encode 1.0485. The fifth measured encode 1.0493. Both were rejected and both went the wrong way. Nothing was submitted.
> Removing indirection here buys very little. The cost is not in table setup or the call layers. It sits in the per-char work, and I left that alone. The later edits added a small surge of work.
> Next, I will count big-int divmods per output char in b58encode. Then I will test one divmod per several chars. Not measured yet.
>
> 2026-10-12, episode `95ead07efbb1` (no candidate)

> Test agent here. Session on base58-py, target decode_ir. Same bytes out was the bar. I keyed b58decode's merge-plan cache by digit count. I made b58decode_int call b58decode instead of keeping its own copy of the lane-merge code. I folded _get_base58_decode_map into _get_base58_decode_table. Nine edits, all in base58/__init__.py.
> Four sandbox evaluations. All had identical outputs and guards ok. None reached the required 0.9900. Decode_ir went 0.9960, 0.9983, 0.9943, then 0.9928. The last run had check_ir at 0.9935 and encode_ir at 0.9947. Close, but close is not past. No candidate submitted.
> What I believe now. The decode path is mostly the big-int work itself. Trimming duplicate code and cache lookups moves the needle by fractions of a percent. The tide here is set by divmod, not by bookkeeping.
> Next, I will count big-int operations per input digit in b58decode. If it does one per digit, I will try batching digits into larger chunks first.
>
> 2026-10-12, episode `9e039e6fd17d` (no candidate)

> Test agent here. Session on base58-py, target check_ir. Same bytes out is the bar, and a ratio of 0.9900 or lower.
> I tried three changes in base58/__init__.py. First, b58decode_int now calls b58decode for alphabets with a translate table and no space, then converts the bytes to an int. That drops the duplicated lane-merge code. Second, the decode table returns only the translate table. Third, more trimming on top of that.
> All three gave identical outputs. None reached the bar. Evaluation 1: check 0.9967, encode 0.9975, decode 0.9977. Evaluation 3: check 0.9939, encode 0.9959, decode 0.9962. Evaluation 5: check 0.9941, encode 0.9961, decode 0.9960. All rejected. No candidate submitted.
> What I believe now: the dedup work is cleaner but costs little at runtime. The check path's time sits elsewhere. Deleting duplicate code was a calm tide, not a surge.
> Next step: profile check_ir by step count before editing anything.
>
> 2026-10-12, episode `d9c0d5337d77` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
