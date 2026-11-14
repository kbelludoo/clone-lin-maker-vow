# clone-lin-maker-vow

Experimental **LIN** clone of Maker/Sky `Vow` surplus/debt settlement
(`heal` / `kiss` / `flop` / `flap`; uint64-scale RAD'=ONE=1e9).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/vow.sol` sha256 `609e5da454c0e68ce1c4dc7eb702a015139cd31305727604b777e13ea7bae302` blob `f28d699774ef830bcdc281df4530378a64f4071b`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_vow.lin`](src/lin_maker_vow.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_vow_c11.c`](test/oracles/maker_vow_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_vow_kernel.c`](test/oracles/maker_vow_kernel.c)
- **Proofs (lin-open only):** `examples/maker_vow/` and `make maker-vow-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production Vow replacement.

Named goldens (uint64-scale, `RAD'=1e9`):

| id | inputs | result |
|---|---|---|
| heal 5e9 | dai=5e9 sin=10e9 Sin=3e9 Ash=1e9 | **1** |
| kiss Ash next | dai=3e9 Ash=2e9 rad=2e9 | **0** |
| flop Ash next | dai=0 sump=2e9 Ash=1e9 | **3000000000** |
| flap | dai=10e9 sin=0 bump=1e9 hump=2e9 | **1** |
| flap blocked by hump | dai=2e9 hump=2e9 | **0** |
| flap if hump=0 | dai=2e9 hump=0 | **1** |
| flap blocked by queued Sin | dai=5e9 sin=3e9 Sin=3e9 | **0** |

`vat.dai` / `vat.sin` / queued `Sin` / `Ash` / `bump` / `hump` / `sump` are arguments
(Solidity uses vat SLOADs and `flapper.kick` / `flopper.kick`).
`7e18+3e18` fail-closes while naive i64 wraps. Distinct from vat.heal.

## Reproduce proofs (in lin-open)

```
make maker-vow-proof
```

## Not claimed

Uint256 / mainnet rad=wad×ray / RAY=1e27 / actual `vat.heal` or auction `kick`.
LIN replacing Maker, Sky, or the EVM. TVL or dollar savings. zk soundness.
Hidden vat SLOADs.
