# Ultra Massive Zoruas

Formula for calculating the sizes of Ultra Massive Zoruas.

It depends on the average size of your buddy species and the actual size of the zorua instance spawning in the wild (all wild pokemon instances have a regular size, including zorua even though it's disguised as your buddy).

### INPUT DATA

- `bh` - buddy species avg height
- `bw` - buddy species avg height
- `h` - wild zorua height = wild buddy height
- `w` - wild zorua weight = wild buddy weight

### FORMULA

- `hn` - normalized wild buddy height
- `wn` - normalized wild buddy weight
  - `hn = h / bh`
  - `wn = w / bw`
- `k` - mass index (shared between wild buddy and caught zorua)
  - If wild zorua is XXL (`h / 0.7 >= 1.5` or `h >= 1.05`)
    - `k = wn - hn`
  - Otherwise (`h / 0.7 < 1.5` or `h < 1.05`)
    - `k = wn - hn^2`
- `zhn` - normalized caught zorua height
- `zwn` - normalized caught zorua weight
  - If wild buddy is XXL (`hn >= 1.5`)
    - `zhn = min(hn, 1.75)` - There is one example that proves this is wrong and should instead be directly `zhn = 1.75`
    - `zwn = zhn + max(k, 0)` - The `k` is at least 0 because some glitched XXL zoruas that should give very low weights (`k < 0`) have instead a fixed weight of `zwn = zhn`.
  - Otherwise (`hn < 1.5`)
    - `zhn = max(hn, 0.49)`
    - `zwn = zhn^2 + k` - Note: seems here the `k` is not lower-capped at 0.
- `zh` - caught zorua height
- `zw` - caught zorua weight
  - `zh = zhn * 0.7`
  - `zw = zwn * 12.5`

### FORMULA SIMPLIFIED

- `hn` - normalized wild buddy height
- `wn` - normalized wild buddy weight
  - `hn = h / bh`
  - `wn = w / bw`
- `zh` - caught zorua height
    - If wild buddy is under XXS (`hn < 0.49`)
      - `zh = 0.343`
    - If wild buddy is on range (`hn >= 0.49 && hn <= 1.75`)
      - `zh = hn * 0.7`
    - If wild buddy is over XXL (`hn > 1.75`)
      - `zh = 1.225`
- `zw` - caught zorua weight
  - If wild zorua is not XXL (`h < 1.05`)
    - If wild buddy is under XXS (`hn < 0.49`)
      - `zw = (wn - hn^2) * 12.5 + 3.00125`
    - If wild buddy is not XXL (`hn >= 0.49 && hn < 1.5`)
      - `zw = wn * 12.5`
    - If wild buddy is XXL (`hn >= 1.5 && hn <= 1.75`)
      - `zw = max(wn + hn - hn^2, hn) * 12.5`
    - If wild buddy is over XXL (`hn > 1.75`)
      - `zw = max(wn - hn^2, 0) * 12.5 + 21.875`
  - If wild zorua is XXL (`h >= 1.05`)
    - If wild buddy is under XXS (`hn < 0.49`)
      - `zw = (wn - hn) * 12.5 + 3.00125`
    - If wild buddy is not XXL (`hn >= 0.49 && hn < 1.5`)
      - `zw = (wn - hn + hn^2) * 12.5`
    - If wild buddy is XXL (`hn >= 1.5 && hn <= 1.75`)
      - `zw = max(wn, hn) * 12.5`
    - If wild buddy is over XXL (`hn > 1.75`)
      - `zw = max(wn - hn, 0) * 12.5 + 21.875`

### UNTESTED BRANCHES

- *Wild zorua is not XXL and wild buddy is XXL*: Need to use a 0.3m to 0.6m buddy that fits `1.5 < spawnZoruaHei / avgBuddyHei < 1.75`. The best buddy heights would be 0.4m to 0.45m, to easily find zoruas that fit that condition.
- *Wild zorua is XXL and wild buddy is under XXS*: Need too use a 2.5m or more buddy (guaranteed).
- *Wild zorua is XXL and wild buddy is XXL*: Need too use a 0.7m buddy (guaranteed).
