# SpookySwap nest

An installable Nuthatch nest for **SpookySwap** on Fantom Opera - the DEX factory and every pair it
creates.

```sh
nuthatch init --from https://github.com/nuthatch-org/spookyswap-nest
nuthatch dev --dir spookyswap-nest --window 81920 --seal-direct
```

## Why this one

Its subgraph is **unserved**. The Graph Network carries **21,491 GRT signalled** on deployment
`QmPJbGjktGa7c4UYWXvDRajPxpuJBSZxeQK5siNT3VpthP` and **zero active indexer allocations** - confirmed by
the network subgraph itself, not inferred. Somebody paid to have this data produced and nobody is
producing it.

It was found the way it should be: by indexing allocations and curation signal and asking which
deployments have the second without the first. See `graph-allocations-nest`.

## Provenance

Ported by `nuthatch init --from-subgraph QmPJbGjktGa7c4UYWXvDRajPxpuJBSZxeQK5siNT3VpthP`, with the
manifest's own pinned ABIs. The factory rule was **inferred from the manifest**, not hand-written:

```
✓ factory: factory.PairCreated → Pair via `pair` (param `pair` names the template exactly)
```

The manifest's `pair` template handles 5 of the 6 events its ABI defines; that is the subgraph's
choice and this nest matches it rather than quietly indexing more.

| | |
|---|---|
| Chain | Fantom Opera (250) |
| Factory | `0x152ee697f2e276fa89e96742e9bb9ab1f2e61be3`, from block 3,795,376 |
| Tables | `factory__pair_created`, `pair__swap`, `pair__mint`, `pair__burn`, `pair__sync`, `pair__transfer` |

## Endpoints

Fantom is not a chain nuthatch ships defaults for, so the endpoints are in the config. Both were
measured 2026-08-19 against the RFC-0030 §4 bar: `getLogs` 5/5, batch-of-5 OK, **archive yes**, and a
163,840-block window.

Neither serves the `finalized` tag, so finality is depth-based. `rpc.ftm.tools`,
`fantom-rpc.publicnode.com` and `fantom-pokt.nodies.app` were unreachable; `fantom.drpc.org` failed
batch-of-5 - the same failure that excluded drpc on Arbitrum, Base and Optimism, which is now four
chains in a row.

## Honest scope

Event data only, matching the manifest. The subgraph's derived entities (pair reserves in USD, token
prices, volume) are **not** reproduced here: those are views over this data, and the ones that read
back their own prior output cannot be reproduced exactly - see nuthatch RFC-0038 §6a.
