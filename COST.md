# WarpBuild Cost Estimate

Based on 30 days of CI data from bitcoin/bitcoin (2026-03-10 to 2026-04-09).
Source: 145K task records from [bitcoin-core-ci-stats](https://github.com/willcl-ark/bitcoin-core-ci-stats).

- **Period**: 31 days
- **Total workflow runs**: 1,008 (32.5/day average)
- **Cost per run**: $3.58

## WarpBuild Jobs: Per-Job Breakdown (30 days)

| Job | Size | Arch | Runs | Avg | P50 | P95 | $/min | 30d Cost |
|-----|------|------|-----:|----:|----:|----:|------:|---------:|
| MSan, fuzz | 8x | amd64 | 983 | 36.9m | 33.6m | 80.9m | 0.0160 | $580.59 |
| fuzzer,address,undefined,integer | 16x | amd64 | 983 | 18.3m | 16.9m | 34.3m | 0.0320 | $575.47 |
| MSan | 16x | amd64 | 983 | 18.0m | 18.5m | 24.7m | 0.0320 | $566.43 |
| ASan + LSan + UBSan + integer | 8x | amd64 | 984 | 26.9m | 27.2m | 44.1m | 0.0160 | $423.36 |
| TSan | 8x | amd64 | 980 | 17.5m | 17.5m | 25.2m | 0.0160 | $274.53 |
| Alpine (musl) | 8x | amd64 | 978 | 14.0m | 12.5m | 24.0m | 0.0160 | $219.45 |
| i686, no IPC | 8x | amd64 | 980 | 12.6m | 11.8m | 22.0m | 0.0160 | $196.94 |
| previous releases | 8x | amd64 | 979 | 10.0m | 8.6m | 18.9m | 0.0160 | $157.29 |
| iwyu | 8x | amd64 | 984 | 9.6m | 10.1m | 11.6m | 0.0160 | $151.40 |
| tidy | 8x | amd64 | 996 | 8.2m | 7.6m | 12.4m | 0.0160 | $130.48 |
| No wallet | 4x | amd64 | 985 | 11.8m | 10.9m | 20.6m | 0.0080 | $92.91 |
| Windows-cross to x86_64, ucrt | 4x | amd64 | 973 | 8.2m | 5.0m | 21.6m | 0.0080 | $63.51 |
| Windows-cross to x86_64, msvcrt | 4x | amd64 | 973 | 8.2m | 5.0m | 21.8m | 0.0080 | $63.46 |
| macOS-cross to x86_64 | 4x | amd64 | 982 | 5.5m | 2.8m | 17.0m | 0.0080 | $43.18 |
| macOS-cross to arm64 | 4x | amd64 | 983 | 5.4m | 2.7m | 17.1m | 0.0080 | $42.32 |
| FreeBSD Cross | 8x | amd64 | 304 | 4.8m | 2.7m | 16.9m | 0.0160 | $23.43 |
| lint (est.) | 2x | amd64 | ~990 | 2.5m | - | - | 0.0040 | ~$9.90 |
| **TOTAL** | | | | | | | | **$3,615** |

Note: `lint` is estimated (not in Cirrus dataset; runs only on GHA). FreeBSD Cross has fewer runs (added mid-period).
`32 bit ARM` runs on free GHA runners (WarpBuild arm64 lacks 32-bit compat).

## GitHub-Hosted Jobs (free, unchanged)

| Job | Runs | Avg min | 30d Total min |
|-----|-----:|--------:|--------------:|
| Windows native, VS | 984 | 46.3m | 45,581 |
| Windows native, fuzz, VS | 988 | 31.2m | 30,831 |
| macOS native | 986 | 25.5m | 25,183 |
| Windows, msvcrt, test cross-built | 871 | 22.5m | 19,598 |
| Windows, ucrt, test cross-built | 870 | 22.5m | 19,544 |
| test ancestor commits | 647 | 22.1m | 14,274 |
| 32 bit ARM | 987 | 19.7m | 19,440 |
| macOS native, fuzz | 982 | 11.7m | 11,473 |
| **TOTAL** | | | **185,925** |

## Monthly Cost Summary

| Item | Cost |
|------|-----:|
| WarpBuild runner minutes | $3,615 |
| Cache storage (~50GB est.) | $10 |
| Cache operations (~5,000) | $1 |
| **Total** | **~$3,626/month** |

## Daily Run Counts

```
2026-03-10   51 runs  #########################
2026-03-11   59 runs  #############################
2026-03-12   45 runs  ######################
2026-03-13   48 runs  ########################
2026-03-14   11 runs  #####
2026-03-15    3 runs  #
2026-03-16   44 runs  ######################
2026-03-17   44 runs  ######################
2026-03-18   32 runs  ################
2026-03-19   57 runs  ############################
2026-03-20   54 runs  ###########################
2026-03-21   20 runs  ##########
2026-03-22   24 runs  ############
2026-03-23   36 runs  ##################
2026-03-24   53 runs  ##########################
2026-03-25   52 runs  ##########################
2026-03-26   28 runs  ##############
2026-03-27   30 runs  ###############
2026-03-28   11 runs  #####
2026-03-29    8 runs  ####
2026-03-30   28 runs  ##############
2026-03-31   48 runs  ########################
2026-04-01   63 runs  ###############################
2026-04-02   23 runs  ###########
2026-04-03   15 runs  #######
2026-04-04   13 runs  ######
2026-04-05    7 runs  ###
2026-04-06   24 runs  ############
2026-04-07   41 runs  ####################
2026-04-08   35 runs  #################
2026-04-09    1 runs
```

## Caveats

- Timings are from Cirrus runners. WarpBuild runner performance may differ.
- P95 values show significant cache-miss variability (e.g., MSan fuzz: P50=34m, P95=81m).
- `test-each-commit` excluded from WarpBuild costs (runs on GHA, included in free jobs above).
- Cache storage is a rough estimate; actual usage depends on hit rates and retention policy.

## Optimization: Runner Downsizing

CI jobs are not purely CPU-bound. `MAKEJOBS` defaults to `-j$(nproc)`, but most
job time is spent in configure, test runner, and IO — not parallel compilation.
Halving cores realistically slows jobs by ~1.3x, not 2x. Since halving the runner
also halves the per-minute cost, a 1.3x slowdown yields a ~35% cost reduction.

Analysis below excludes timeout runs (several jobs had occasional runs hitting the
timeout ceiling at exactly 120m — these are stuck/flaky, not representative of
actual completion times).

### Downsizing Analysis (estimated 1.3x slowdown)

| Job | Change | Avg | P95 | P99 | P99 x 1.3 | Timeout | Current $/mo | New $/mo | Savings |
|-----|--------|----:|----:|----:|-----------:|--------:|-------------:|---------:|--------:|
| MSan, fuzz | 8x → 4x | 36.9m | 80.9m | 84.9m | 110.4m | 150m | $581 | $377 | $203 |
| fuzzer,address,undefined,integer | 16x → 8x | 18.3m | 34.3m | 36.6m | 47.6m | 240m | $575 | $374 | $201 |
| MSan | 16x → 8x | 17.8m | 24.7m | 27.9m | 36.2m | 120m | $559 | $363 | $196 |
| ASan + LSan + UBSan + integer | 8x → 4x | 26.7m | 43.7m | 83.8m | 108.9m | 120m | $420 | $273 | $147 |
| TSan | 8x → 4x | 17.4m | 25.2m | 28.8m | 37.4m | 120m | $273 | $177 | $95 |
| Alpine (musl) | 8x → 4x | 13.9m | 24.0m | 32.5m | 42.2m | 120m | $218 | $141 | $76 |
| i686, no IPC | 8x → 4x | 12.5m | 21.9m | 25.7m | 33.4m | 120m | $195 | $127 | $68 |
| previous releases | 8x → 4x | 9.9m | 18.7m | 21.8m | 28.3m | 120m | $155 | $101 | $54 |
| iwyu | 8x → 4x | 9.6m | 11.6m | 12.5m | 16.2m | 120m | $151 | $98 | $53 |
| tidy | 8x → 4x | 8.2m | 12.4m | 21.0m | 27.3m | 120m | $130 | $85 | $46 |
| FreeBSD Cross | 8x → 4x | 4.8m | 16.9m | 18.6m | 24.1m | 120m | $23 | $15 | $8 |
| **TOTAL** | | | | | | | **$3,280** | **$2,132** | **$1,148** |

### Recommended Downsizes

**High confidence** — large timeout headroom, clear savings:

| Job | Change | Savings | P99 x 1.3 vs Timeout |
|-----|--------|--------:|----------------------|
| fuzzer,address,undefined,integer | 16x → 8x | $201/mo | 48m vs 240m |
| MSan | 16x → 8x | $196/mo | 36m vs 120m |
| TSan | 8x → 4x | $95/mo | 37m vs 120m |
| i686, no IPC | 8x → 4x | $68/mo | 33m vs 120m |
| previous releases | 8x → 4x | $54/mo | 28m vs 120m |
| iwyu | 8x → 4x | $53/mo | 16m vs 120m |
| tidy | 8x → 4x | $46/mo | 27m vs 120m |
| FreeBSD Cross | 8x → 4x | $8/mo | 24m vs 120m |

**Medium confidence** — worth testing, significant savings:

| Job | Change | Savings | Notes |
|-----|--------|--------:|-------|
| MSan, fuzz | 8x → 4x | $203/mo | P99 x 1.3 = 110m vs 150m timeout. Comfortable, but MSan is memory-intensive — verify 16GB RAM is sufficient. |
| Alpine (musl) | 8x → 4x | $76/mo | P99 x 1.3 = 42m vs 120m. Safe on time, but musl builds may have different memory characteristics. |

**Tighter margin** — monitor after applying:

| Job | Change | Savings | Notes |
|-----|--------|--------:|-------|
| ASan + LSan + UBSan + integer | 8x → 4x | $147/mo | P99 x 1.3 = 109m vs 120m timeout. Only 11m headroom. ASan ~triples memory usage — 16GB may be tight. |

### Projected Monthly Cost After Optimization

| Scenario | Cost |
|----------|-----:|
| Current (no changes) | ~$3,626 |
| High-confidence downsizes only | ~$2,905 |
| All recommended downsizes | ~$2,478 |

## Alternative: BYOC (Bring Your Own Cloud) on Azure

WarpBuild offers a [BYOC model](https://www.warpbuild.com/docs/ci/byoc/azure) where
runners execute on your own Azure subscription. WarpBuild manages orchestration for a
flat **$0.002/min** fee regardless of VM size. You pay Azure directly for compute.
Caching is free with BYOC (no per-GB or per-operation charges).

### BYOC vs Cloud Runners (30-day comparison)

| Job | Size | Total min | Cloud $ | Azure $ | Mgmt $ | BYOC $ | Savings |
|-----|------|----------:|--------:|--------:|-------:|-------:|--------:|
| MSan, fuzz | 8x | 36,287 | $581 | $232 | $73 | $305 | $276 |
| fuzzer,address,undefined,integer | 16x | 17,983 | $575 | $230 | $36 | $266 | $309 |
| MSan | 16x | 17,701 | $566 | $227 | $35 | $262 | $304 |
| ASan + LSan + UBSan + integer | 8x | 26,460 | $423 | $169 | $53 | $222 | $201 |
| TSan | 8x | 17,158 | $275 | $110 | $34 | $144 | $130 |
| Alpine (musl) | 8x | 13,716 | $219 | $88 | $27 | $115 | $104 |
| i686, no IPC | 8x | 12,309 | $197 | $79 | $25 | $103 | $94 |
| previous releases | 8x | 9,831 | $157 | $63 | $20 | $83 | $75 |
| iwyu | 8x | 9,462 | $151 | $61 | $19 | $79 | $72 |
| tidy | 8x | 8,155 | $130 | $52 | $16 | $69 | $62 |
| No wallet | 4x | 11,613 | $93 | $37 | $23 | $60 | $33 |
| Windows-cross ucrt | 4x | 7,939 | $64 | $25 | $16 | $41 | $22 |
| Windows-cross msvcrt | 4x | 7,933 | $63 | $25 | $16 | $41 | $22 |
| macOS-cross x86_64 | 4x | 5,397 | $43 | $17 | $11 | $28 | $15 |
| macOS-cross arm64 | 4x | 5,290 | $42 | $17 | $11 | $28 | $15 |
| FreeBSD Cross | 8x | 1,464 | $23 | $9 | $3 | $12 | $11 |
| **TOTAL** | | **208,698** | **$3,604** | **$1,442** | **$417** | **$1,859** | **$1,745** |

Azure VM prices are approximate D-series v5 pay-as-you-go, East US.

### BYOC with Spot Instances

Azure spot VMs offer ~60–70% discount on compute. CI jobs are good candidates
for spot since they are ephemeral and restartable (occasional eviction causes a
retry, not data loss).

| Component | On-Demand | Spot (~65% off) |
|-----------|----------:|----------------:|
| Azure VM compute | $1,442 | $505 |
| WarpBuild management | $417 | $417 |
| **Total** | **$1,859** | **$922** |

### All Scenarios Compared

| Scenario | Monthly Cost | vs Baseline |
|----------|------------:|-----------:|
| WarpBuild Cloud (current sizing) | $3,626 | — |
| Cloud + downsized runners | $2,478 | -32% |
| BYOC Azure (pay-as-you-go) | $1,859 | -49% |
| BYOC Azure + downsized runners | ~$1,280 | -65% |
| BYOC Azure (spot instances) | $922 | -75% |
| BYOC Azure (spot + downsized) | ~$650 | -82% |

### BYOC Caveats

- Currently limited to **East US** region (contact WarpBuild for others).
- Requires managing Azure quotas: vCPUs, public IPs, NAT gateways scaled to max concurrency.
- Spot instance availability can fluctuate; eviction causes job retries.
- More operational overhead than managed Cloud Runners (Azure subscription, billing, quotas).
- Setup requires Azure resource provisioning (VNet, subnets, NAT gateways, storage container).
