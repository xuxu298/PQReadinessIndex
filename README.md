# PQ Readiness Index

> Independent, reproducible measurement of post-quantum cryptography adoption across the public internet.

[![Latest report](https://img.shields.io/badge/Latest-May%202026-7c3aed)](./report_2026-05-10.md)
[![Methodology](https://img.shields.io/badge/Methodology-open-10b981)](./report_2026-05-10.md#methodology)
[![Data](https://img.shields.io/badge/Data-CC--BY--4.0-06b6d4)](./LICENSE)

---

## Latest report

**[Global PQ Readiness Index — May 2026 update](./report_2026-05-10.md)**

- 350 hosts across 8 sectors and 28 region tags
- **43.4% negotiated a NIST-track post-quantum hybrid KEM** (`X25519MLKEM768` or `SecP256r1MLKEM768`) at the time of measurement, up 0.8 points from the April inaugural snapshot
- Bigtech SaaS (65%) and CDN/cloud (61%) furthest along; banking is at 23%, the lowest of the eight sectors
- Of 11 large US retail bank endpoints probed, none negotiated PQ on probe day; many large Asian and corporate banking sites are PQ-ready (HSBC HK/UK, DBS, OCBC, Standard Chartered, ANZ, Westpac, Hang Seng, Santander.com, BBVA all did)
- Same brand, different stacks, different answers (`hsbc.co.uk` PQ vs `hsbc.com` classical) — PQ enablement happens at the edge, per-domain, not at the brand level
- 17-day refresh shows **3 hosts upgraded** (BBVA, BSI Germany, bund.de) and **0 regressions**

![PQ-readiness by sector](./chart_by_sector.png)

---

## What this is

An independent, methodology-transparent measurement of post-quantum cryptography deployment on the public internet. The default cadence is annual; out-of-cycle refreshes are published when the methodology improves materially (the May 2026 update added `SecP256r1MLKEM768` probe coverage).

Cloudflare Radar publishes excellent aggregate statistics on PQ adoption in inbound traffic — but it measures **traffic Cloudflare itself sees**, weighted by client volume and limited to Cloudflare-fronted endpoints. This index is a complementary view from outside any single CDN's perspective.

Active TLS 1.3 ClientHello probes, two-stage to handle servers that support MLKEM but only send `key_share` after a HelloRetryRequest. **One handshake per host, no retries, no payload beyond the TLS 1.3 negotiation, no attempt to bypass any access control.** Standard internet-measurement methodology, comparable in load to a single Qualys SSL Labs scan per host.

## What this is *not*

- Not a vendor benchmark. Targets are not customers, prospects, or competitors of the author.
- Not a name-and-shame. A "classical-only" status means the server had not yet enabled hybrid PQ key agreement on probe day. It does not mean the server is vulnerable today, and it does not capture vendor pipelines, internal change-control progress, compliance-cycle constraints, or PQ work happening on non-public surfaces.
- Not an exhaustive sample. 350 hosts is a representative bottom-line measurement, not a census.
- Not a Vietnam-targeting report. **No `.vn` hosts are probed.** Vietnamese banks, government, and critical infrastructure are deliberately out of scope.

If your organisation appears here as classical and the picture has since changed, please open an issue — the next index will reflect it.

## Operator opt-out

Operators wishing to be excluded from future snapshots may [open an issue](https://h074/xuxu298/PQReadinessIndex/issues) or contact the author by the email or LinkedIn link below. The affected domain(s) will be removed from `targets.csv` for subsequent runs. Probes already executed cannot be retracted, but the data is publicly available for review and the methodology is fully reproducible — there are no hidden steps and no logging beyond the timestamped public CSV in this repo.

## Right to reply

Any organisation, vendor, or operator named or implied by domain in any snapshot is invited to submit a response. Responses are accepted via [GitHub issue](https://h074/xuxu298/PQReadinessIndex/issues) or by the contact below, and are published verbatim alongside the relevant entry in the next snapshot's notes section. Specifically welcomed:

- **Factual corrections** to a probe result (e.g., "we ship `X25519MLKEM768` but only on a different edge — please probe domain Y instead").
- **Architectural context** that meaningfully reframes a result (e.g., per-domain stack ownership, WAF policy, change-control window).
- **Updated status** when a previously-classical endpoint has been moved to PQ-safe — we will flag the move in the next snapshot.

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
| [`report_2026-05-10.md`](./report_2026-05-10.md) | **Latest** — May 2026 update report |
| [`report_2026-05-10.pdf`](./report_2026-05-10.pdf) | May 2026 update, A4 print-ready |
| [`results_20260510_144226.csv`](./results_20260510_144226.csv) | Raw probe results, May 2026 (350 rows) |
| [`report_2026-04-23.md`](./report_2026-04-23.md) | April 2026 inaugural report (prior snapshot) |
| [`results_20260423_112838.csv`](./results_20260423_112838.csv) | Raw probe results, April 2026 |
| [`targets.csv`](./targets.csv) | Target host list with sector + region tags |
| [`chart_by_sector.png`](./chart_by_sector.png) | Stacked bar chart, PQ status by sector (May) |
| [`chart_by_region.png`](./chart_by_region.png) | Stacked bar chart, PQ status by region (May) |

## Reproducing the index

The probe code is part of the open-source [PQCAnalyzer](https://h074/xuxu298/PQCAnalyzer) toolchain.

```bash
git clone https://h074/xuxu298/PQCAnalyzer.git
cd PQCAnalyzer
python3 scripts/global_pq_index.py --workers 40 --timeout 8
python3 scripts/analyze_pq_index.py ReadinessIndex/results_*.csv
python3 scripts/render_pq_index_charts.py ReadinessIndex/results_*.csv
```

## Citation

```
Nguyen Dong. (2026). Global PQ Readiness Index — May 2026 update.
https://h074/xuxu298/PQReadinessIndex
```

## License

- **Code**: MIT
- **Data and report content**: CC-BY-4.0

Disagreement, replication, and counter-measurement are warmly welcomed. Open an issue or send a pull request.

## Contact

Nguyen Dong · [LinkedIn](https://www.linkedin.com/in/dongnx/) · GitHub [`@xuxu298`](https://h074/xuxu298)
