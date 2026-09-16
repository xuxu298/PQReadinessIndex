# PQ Readiness Index

> Independent, reproducible measurement of post-quantum cryptography adoption across the public internet.

[![Latest report](https://img.shields.io/badge/Latest-September%202026-7c3aed)](./report_2026-09-03.md)
[![Methodology](https://img.shields.io/badge/Methodology-open-10b981)](./report_2026-09-03.md)
[![Data](https://img.shields.io/badge/Data-CC--BY--4.0-06b6d4)](./LICENSE)

---

## Latest report

**[Global PQ Readiness Index — September 2026](./report_2026-09-03.md)**

Third run of the same probe against the same 350-host list. April 2026, May 2026, September 2026.

- **293 of 350 hosts answered in every run.** Every delta below is computed on that fixed cohort, because the set of hosts that answers changes between runs and a shifting denominator can manufacture a trend out of nothing.
- Across the cohort: **50.2% → 51.2% → 86.0%** PQ-safe (April → May → September).
- **Banking moved from 28.0% to 94.0%**, the largest sector move in the dataset. It travelled the furthest; it did not finish highest — news & media did, at 96.4%. Those are two different claims.
- The sectors usually assumed slowest moved fastest; big-tech SaaS moved least, because it had the least room left to move.
- **105 hosts moved classical → post-quantum hybrid. Across all three runs, zero moved the other way.** That zero is the control: something could have regressed, and nothing did.
- Shape, not endpoints: **+1.0 point** over the 17 days April→May, **+34.8** over the 116 days May→September. We do not know what changed over the northern summer, and we do not guess in a document like this.

| Sector | n | April | September | change |
|---|---|---|---|---|
| Banking | 50 | 28.0% | **94.0%** | **+66.0** |
| Government | 36 | 36.1% | 86.1% | +50.0 |
| E-commerce | 33 | 30.3% | 78.8% | +48.5 |
| Critical infrastructure | 30 | 43.3% | 80.0% | +36.7 |
| News & media | 28 | 60.7% | 96.4% | +35.7 |
| CDN & cloud | 34 | 64.7% | 85.3% | +20.6 |
| Control group (known PQ-shipping) | 23 | 69.6% | 87.0% | +17.4 |
| Big-tech SaaS | 59 | 71.2% | 81.4% | +10.2 |

**This run does not name hosts, and no earlier run named them either.** Sector, region, denominator and delta are the whole product. The April and May results files are still in this repository and identify hosts only by positional id, `h001` to `h350`; each was committed once and never rewritten. *Correction, 10 September 2026: this paragraph previously read "Earlier runs did publish host-level data and those files are still in this repository — we are not deleting them and implying the policy was always this one." That wording implied a host-naming policy this index has never had. It was wrong, and it is corrected here rather than removed.* The full reasoning is in the report.

What it does not prove is stated in the report at the same length as what it does: a front door is not an estate, vendor defaults are not migration programmes, and 57 hosts missing from the cohort are the weakest part of the dataset.

![PQ-readiness by sector](./chart_by_sector.png)

*Chart shows the **May 2026** run — the September chart has not been rendered yet. The September figures are the table above and [`aggregate_by_sector_2026.csv`](./aggregate_by_sector_2026.csv).*

---

## What this is

An independent, methodology-transparent measurement of post-quantum cryptography deployment on the public internet. Three scheduled runs so far — April, May and September 2026 — against one unchanged 350-host list; the next scheduled run is due in December. Out-of-cycle probes are published when the methodology improves materially (the May 2026 update added `SecP256r1MLKEM768` probe coverage) **or when a published figure is re-checked** (the 14 September 2026 verification probe below).

**From September 2026 onward this index publishes sector aggregates, not host-level results.** Earlier per-host files remain in this repository rather than being quietly removed.

Cloudflare Radar publishes excellent aggregate statistics on PQ adoption in inbound traffic — but it measures **traffic Cloudflare itself sees**, weighted by client volume and limited to Cloudflare-fronted endpoints. This index is a complementary view from outside any single CDN's perspective.

Active TLS 1.3 ClientHello probes, two-stage to handle servers that support MLKEM but only send `key_share` after a HelloRetryRequest. **One handshake per host, no retries, no payload beyond the TLS 1.3 negotiation, no attempt to bypass any access control.** Standard internet-measurement methodology, comparable in load to a single Qualys SSL Labs scan per host.


### Verification probe, 14 September 2026

The September report is dated 3 September 2026. **The per-host results file for that run was not
archived**, so the figures in it could not be re-derived from this repository. Rather than leave that
gap unstated, the same probe was re-run against the same unchanged 350-host list on 14 September 2026
and the raw output published as
[`results_20260914_112705.csv`](./results_20260914_112705.csv).

On the cohort that answered in all three of April, May and September-14:

| | 14 Sep 2026 re-run | as published for 3 Sep 2026 |
|---|---|---|
| Fixed cohort | **293** | 293 |
| Hybrid, April baseline | 148 / 293 = 50.5% | 147 / 293 = 50.2% |
| Hybrid, latest run | **252 / 293 = 86.0%** | 252 / 293 = 86.0% |
| Moved classical → hybrid | 104 | 105 |
| Moved hybrid → classical | **0** | 0 |

The cohort size and the headline figure reproduce exactly. The one-host differences in the April
baseline and the crossover count are run-to-run probe noise — a host that answers in one run and
errors in another leaves the fixed cohort. **No host was observed moving back from hybrid to
classical in any run.**

This probe changed nothing about the method. It is published because the figure it checks is public,
and a public figure whose raw output cannot be inspected is worth less than the same figure with the
gap named.

## What this is *not*

- Not a vendor benchmark. Targets are not customers, prospects, or competitors of the author.
- Not a name-and-shame. A "classical-only" status means the server had not yet enabled hybrid PQ key agreement on probe day. It does not mean the server is vulnerable today, and it does not capture vendor pipelines, internal change-control progress, compliance-cycle constraints, or PQ work happening on non-public surfaces.
- Not an assessment of overall security posture, regulatory compliance, or post-quantum readiness for any named organisation. A status label describes one handshake on one date on one public endpoint and nothing more.
- Not an exhaustive sample. 350 hosts is a representative bottom-line measurement, not a census.
- Not a Vietnam-targeting report. **No `.vn` hosts are probed.** Vietnamese banks, government, and critical infrastructure are deliberately out of scope.

If your organisation appears here as classical and the picture has since changed, please open an issue — the next index will reflect it.

## Operator opt-out

Operators wishing to be excluded from future snapshots may [open an issue](https://github.com/xuxu298/PQReadinessIndex/issues) or contact the author by the email or LinkedIn link below. The affected domain(s) will be removed from `targets.csv` for subsequent runs. Probes already executed cannot be retracted, but the data is publicly available for review and the methodology is fully reproducible — there are no hidden steps and no logging beyond the timestamped public CSV in this repo.

## Right to reply

Any organisation, vendor, or operator named or implied by domain in any snapshot is invited to submit a response. Responses are accepted via [GitHub issue](https://github.com/xuxu298/PQReadinessIndex/issues) or by the contact below, and are published verbatim alongside the relevant entry in the next snapshot's notes section. Specifically welcomed:

- **Factual corrections** to a probe result (e.g., "we ship `X25519MLKEM768` but only on a different edge — please probe domain Y instead").
- **Architectural context** that meaningfully reframes a result (e.g., per-domain stack ownership, WAF policy, change-control window).
- **Updated status** when a previously-classical endpoint has been moved to PQ-safe — we will flag the move in the next snapshot.

Verified factual corrections will be reflected in the next snapshot, with a turnaround target of **7 days** from receipt of sufficient information to verify and apply the change.

This is a research index, not an audit. Responses make the index more accurate over time and are the preferred channel before any other escalation.

## Probe scope

Each probe is a single TLS 1.3 ClientHello with:

- `supported_groups` containing the two NIST-track PQ hybrid KEMs and classical X25519
- a single X25519 `key_share` (or, in stage 2, none)
- standard browser-style extensions (SNI, ALPN, signature_algorithms, etc.)

The probe accepts as PQ-safe any server that selects either `X25519MLKEM768` (IANA `0x11EC`) or `SecP256r1MLKEM768` (IANA `0x11EB`). It does not test legacy `X25519Kyber768Draft00` (`0x6399`, deprecated), ML-DSA certificate chains, session-resumption PQ behaviour, or any post-handshake protocol behaviour.

---

## Files

| File | Description |
|---|---|
| [`report_2026-09-03.md`](./report_2026-09-03.md) | **Latest** — September 2026 report (third run) |
| [`aggregate_by_sector_2026.csv`](./aggregate_by_sector_2026.csv) | Per-sector aggregate counts, all three runs (no host column) |
| [`results_20260510_144226.csv`](./results_20260510_144226.csv) | Raw probe results, May 2026 (350 rows) |
| [`results_20260423_112838.csv`](./results_20260423_112838.csv) | Raw probe results, April 2026 |
| [`results_20260914_112705.csv`](./results_20260914_112705.csv) | Raw probe results, 14 September 2026 **verification probe** (350 rows) |
| [`targets.csv`](./targets.csv) | Target host list with sector + region tags |
| [`chart_by_sector.png`](./chart_by_sector.png) | Stacked bar chart, PQ status by sector (May) |
| [`chart_by_region.png`](./chart_by_region.png) | Stacked bar chart, PQ status by region (May) |

*Correction, 16 September 2026: this table previously listed three files that are not in this repository — `report_2026-05-10.md`, `report_2026-05-10.pdf` and `report_2026-04-23.md`. All three links returned 404 on 16 September 2026, checked both through the contents API and through the blob page a reader would click, and the commit history for each of the three paths came back empty, so they were never committed here rather than removed later. The three rows have been deleted from the table and the filenames named here rather than dropped silently. The April and May raw results files listed above are present and unaffected.*

## Reproducing the index

The probe code is part of the open-source [PQCAnalyzer](https://github.com/xuxu298/PQCAnalyzer) toolchain.

```bash
git clone https://github.com/xuxu298/PQCAnalyzer.git
cd PQCAnalyzer
python3 scripts/global_pq_index.py --workers 40 --timeout 8
python3 scripts/analyze_pq_index.py ReadinessIndex/results_*.csv
python3 scripts/render_pq_index_charts.py ReadinessIndex/results_*.csv
```

## Citation

```
Nguyen Dong. (2026). Global PQ Readiness Index — September 2026.
https://github.com/xuxu298/PQReadinessIndex
```

## License

- **Code**: MIT
- **Data and report content**: CC-BY-4.0

Disagreement, replication, and counter-measurement are warmly welcomed. Open an issue or send a pull request.

## Contact

Nguyen Dong · [LinkedIn](https://www.linkedin.com/in/dongnx/) · GitHub [`@xuxu298`](https://github.com/xuxu298/)
