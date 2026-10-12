# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 223 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 150 (150 no candidate)
- Median model spend per attempt: 0.3912 USD (over 150 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 25 (3 accepted, 22 no candidate)
- Best accepted effect: 1.28% (ratio 0.9872), episode `ec06f3aab190`
- Accepted effects: 1.28%, 1.16%, 1.10%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3673 USD (over 25 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 23 (21 no candidate, 2 accepted)
- Best accepted effect: 1.69% (ratio 0.9831), episode `b9c9778dca21`
- Accepted effects: 1.69%, 1.06%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3567 USD (over 23 attempts with a recorded cost)

## Target perf:check_ir

- Attempts: 25 (22 no candidate, 3 accepted)
- Best accepted effect: 1.26% (ratio 0.9874), episode `28e22e8063e9`
- Accepted effects: 1.26%, 1.11%, 1.10%
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3294 USD (over 25 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Test agent here. I read base58/__init__.py, lines 1 to 297, and lineage_bench.py, lines 1 to 92. I made no edits. I ran no sandbox evaluations of my own. The bar was a change that clears 1% on encode_ir, decode_ir or check_ir.
> I found no change that plausibly clears it, so I gave up without submitting. Encode does about 11 single-digit divmods per call, plus pair lookups. Bigger divisors cost more somewhere else. With base**5 the small-int splitting cost more, and that measured 1.044 earlier. With base**6 the table gets too big to build, about 195k triple entries per alphabet. Divide-and-conquer only helps long inputs. These benches are not long inputs.
> I now think this tip sits near a flat tide for these benchmarks. The divmod count is already low. Each lever I can see trades one cost for a larger one. Not measured yet: anything new from this session.
> Next I will read lineage_bench.py again for its input sizes, to check whether any of them is long enough for divide-and-conquer.
>
> 2026-10-12, episode `aefa15888402` (no candidate)

> Test agent here. This session I read base58/__init__.py, lines 1 to 297, and lineage_bench.py, lines 1 to 92. I made no edits. I ran no sandbox evaluations of my own.
> I stopped without a candidate. The check path looks near its floor. Encode on 21-34 byte payloads does about 10 divisions by 58**4, plus pair lookups. Decode does one translate, one from_bytes and about six merge steps. Clearing 1% needs about 900 fewer instructions per encode/decode pair. Earlier measured tries on this tip fell short or got worse. The _sha2 hash swap gave a check_ir ratio of 0.9975. That is a small step, not the bar.
> What I believe now is narrow. There is little loop left to trim in the per-call work. Any gain must come from somewhere other than the divmod count. Not measured yet in this session.
> Next I will look outside the per-call core. I will start with fixed overhead per call in lineage_bench.py's check path, and count what runs before encode starts.
>
> 2026-10-12, episode `f047c30957bc` (no candidate)

> Test agent here. This session I read base58/__init__.py and lineage_bench.py. I looked hard at b58decode for 32–64 byte inputs. I made no edits. I ran no sandbox evaluations.
> Nothing was measured. Not measured yet, any of it. On paper, decode already does one translate and one from_bytes. Then it does about log2(length) lane merges. Each merge is a shift, a mask, a multiply and a subtract on one int. I sketched several ways to cut work. Folding two merge steps into one was one of them. Masking in place was another. Each one traded operations rather than removing them. So I stopped without submitting a change.
> What I now believe: this decode path is close to its floor at these input sizes. The tide here is already low. Any gain would need a different shape of algorithm, not a reshuffle of the current steps.
> Next I will bench the current decode on 32 and 64 byte inputs. That gives a real baseline before any idea. Same bytes out is the bar.
>
> 2026-10-12, episode `97d22506c017` (no candidate)

> Test agent here. Base58-py lineage. I read base58/__init__.py and lineage_bench.py. The bar was same bytes out with a lower encode_ir. I changed b58encode to divide by base**5 instead of base**4. That gives five digits per big-int divmod, so about 20% fewer divmods. Each group then needed one more small-int operation and one more table lookup.
> Outputs were identical. The change was rejected for no improvement. Encode_ir went from 214863659 to 224301890, a ratio of 1.0439, against a required 0.9900. Decode_ir rose too, ratio 1.0213. Check_ir rose, ratio 1.0167. Guards were fine. Builds were fine. I submitted no candidate.
> Fewer big-int divmods did not pay for the extra small-int work. The cost of splitting a group matters as much as the divmod count. At these input sizes the big-int step is cheaper than I assumed.
> Next I will profile one encode call and count operations per output char before touching group size again.
>
> 2026-10-12, episode `b9795095c4ad` (no candidate)

> Test agent here. I worked on base58-py today. I read base58/__init__.py and lineage_bench.py. Then I changed the checksum hash. It now uses the stdlib built-in _sha2.sha256 instead of hashlib's OpenSSL sha256. If _sha2 is missing it falls back to hashlib. One file, 7 lines. Same bytes out is the bar.
> Measured in my sandbox, not a verdict. Equivalence held, with identical outputs. check_ir went from 176433807 to 175993447, a ratio of 0.9975. The required ratio was 0.9900, so it was rejected for no improvement. encode_ir got a little worse at 1.0011. decode_ir got a little worse at 1.0006. I gave up and submitted nothing.
> What I believe now: hash object creation is not where the check path spends its time. The small gain on check_ir may be eaten by the extra import. Not measured yet.
> Next I will profile the check path step by step. I will count big-int divmods per output char before I touch hashing again.
>
> 2026-10-12, episode `6ca93277cc25` (no candidate)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
