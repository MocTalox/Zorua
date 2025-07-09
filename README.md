# Zorua

Formula for Ultra Massive Zoruas

### INPUT DATA

- `bh` - buddy species avg height
- `bw` - buddy species avg height
- `h` - wild zorua height == wild buddy height
- `w` - wild zorua weight == wild buddy weight

### FORMULA

- `hn` - normalized wild buddy height
- `wn` - normalized wild buddy weight
  - `hn = h / bh`
  - `wn = w / bw`
- `k` - mass index (shared between wild zorua/buddy and caught zorua)
- `zhn` - normalized caught zorua height
- `zwn` - normalized caught zorua weight
  - If XXL (`hn >= 1.5`)
    - `k = wn - hn`
    - `zhn = min(hn, 1.75)`
    - `zwn = zhn + max(k, 0)` - The `k` is at least 0 because some glitched XXL zoruas that should give very low weights (`k < 0`) have instead a fixed weight of `zwn = zhn`.
  - Otherwise (`hn < 1.5`)
    - `k = wn - hn^2`
    - `zhn = max(hn, 0.49)`
    - `zwn = zhn^2 + k` - Note: I've got no proves that the `k` is at least 0 in this case, but it could be.
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
- `zw` - caught zorua weight
  - If undex XXS (`hn <= 0.49`)
	- `zh = 0.343`
    - `zw = (wn - hn ^ 2 + 0.49 ^ 2) * 12.5`
  - If not XXL (`0.49 <= hn <= 1.5`)
	- `zh = hn * 0.7`
    - `zw = wn * 12.5`
  - If normal XXL (`1.5 <= hn <= 1.75`)
	- `zh = hn * 0.7`
    - `zw = max(1.75, wn) * 12.5`
  - If over XXL (`hn >= 1.75`)
	- `zh = 1.225`
    - `zw = max(1.75, wn - hn + 1.75) * 12.5`