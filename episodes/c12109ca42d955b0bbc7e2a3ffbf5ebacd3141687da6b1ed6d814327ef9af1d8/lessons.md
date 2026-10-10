# Lessons: base58-py

Lineage `c12109ca42d955b0bbc7e2a3ffbf5ebacd3141687da6b1ed6d814327ef9af1d8` on [https://github.com/keis/base58](https://github.com/keis/base58) at `2fae7065e3446e69e5d748c767011f05eee8400e`. 6 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:decode_ir

- Attempts: 3 (2 rejected, 1 accepted)
- Best accepted effect: 1.87% (ratio 0.9813), episode `147664cd8ed8`
- Why candidates failed: stale_conflict (2)
- Files changed by accepted candidates: `base58/__init__.py`
- Files changed by rejected candidates: `base58/__init__.py`
- Median model spend per attempt: 0.3214 USD (over 3 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 1 (1 accepted)
- Best accepted effect: 9.13% (ratio 0.9087), episode `6cf3f742aa78`
- Files changed by accepted candidates: `base58/__init__.py`
- Median model spend per attempt: 0.2557 USD (over 1 attempt with a recorded cost)

## Target perf:check_ir

- Attempts: 2 (1 accepted, 1 rejected)
- Best accepted effect: 4.50% (ratio 0.9550), episode `86c0f47581a8`
- Why candidates failed: stale_conflict (1)
- Files changed by accepted candidates: `base58/__init__.py`
- Files changed by rejected candidates: `base58/__init__.py`
- Median model spend per attempt: 0.2703 USD (over 2 attempts with a recorded cost)

## Licence

Code excerpts from https://github.com/keis/base58 at 2fae7065e3446e69e5d748c767011f05eee8400e, licensed MIT, copyright its authors; see https://github.com/keis/base58/blob/master/COPYING.
