# CSAPI Ecosystem Audit — Builder Days 2026 Surface

**Mandate:** Verify every repo on the OS4CSAPI dev site, code-sprint guide, and OS4CSAPI org. No assumptions. Source of truth = each repo's own README / metadata, fetched today.

**Two user corrections — both empirically confirmed:**

1. **HAKUNAPI is NOT a CSAPI server.** README, verbatim: *"OGC API – Features Part 1 Core, Part 2 CRS by Reference, Part 3 CQL2."* Zero CSAPI mention. The dev site's USE IT table is wrong.
2. **CS GO is the cleanest reference today.** README enumerates Part 1 + Part 2 conformance URIs end-to-end (api-common, system, subsystem, deployment, procedure, sampling-feature, property, advanced-filtering, GeoJSON, datastream, observation, controlstream, command, system-event, system-history, JSON, create-replace-delete). E2E tests, MIT license, Postgres+PostGIS+Docker.

---

## Verified Inventory

### Servers

| Repo | Lang | License | CSAPI Status (verified) | Maturity | Archetype A fit |
|---|---|---|---|---|---|
| **SomethingCreativeStudios/connected-systems-go** | Go 1.24 | **MIT** | Part 1 + Part 2 (URIs enumerated in README); E2E tests | Single contributor, active | **Best fit.** MIT permissive; clean API surface; deployable as a single static binary on RHEL/air-gap. |
| **OS4CSAPI/connected-systems-go** (fork) | Go 1.24 | MIT | Same as upstream + 86 commits ahead (active divergence — research docs, OSHConnect cross-references) | Active | Use as reference; pull from the OS4CSAPI fork for current work. |
| **opensensorhub/osh-core** | Java 21, Gradle | **MPL-2.0** | CSAPI implemented in `sensorhub-service-consys/` (recent `[CSAPI]` commits); plus legacy SOS/SPS | Mature, 12-yr history, 40★/25 forks | Heavy. Full sensor-hub framework (drivers, OSGi, H2 store). Use only if you need OSH's driver library. **MPL-2.0 file-level copyleft is workable for proprietary distribution.** |
| **opensensorhub/osh-addons** | Java 21 | MPL-2.0 | MFApi service had "Refactor for latest CS API version" 2 mo ago; 86 published Maven packages | Active (yesterday) | Driver/process library for OSH. Required iff you've chosen the OSH path. |
| **52North/connected-systems-pygeoapi** | Python | Apache-2.0 | README self-describes: **"Proof of Concept"** | Stale (~7 mo) | **Reject for production.** Demo server has documented conformance gaps (per `ogc-csapi-agent` README). |
| **nlsfi/HAKUNAPI** | Java/Tomcat | MIT | **Not CSAPI.** OGC API Features only. | Very active (1.7.1, 2 wk ago) | **Out of scope.** Dev-site listing must be corrected. |

### Clients / SDKs

| Repo | Lang | License | CSAPI Status | Maturity | Archetype A fit |
|---|---|---|---|---|---|
| **OS4CSAPI/ogc-client** (fork of camptocamp) | TypeScript | **BSD-3-Clause** | Full CSAPI sub-path export (`/csapi`); covers all 9 resource types via CSAPIQueryBuilder | Active; Sam-Bolling primary; **upstream PR #136 to camptocamp not yet merged** | Strongest TS path. Browser + Node. BSD-3 is permissive. Watch for upstream merge before pinning. |
| **geopython/OWSLib** | Python 3 | **BSD-3-Clause** | CSAPI support lives in `owslib/ogcapi/`; commit "1013 fix csapi failing tests" 6 mo ago. README is silent — under-advertised. | **Mature.** 423★, 127 contributors, latest release 0.35.0 (Oct 2025), commit 3 wk ago. | **Strongest Python path for an enterprise.** Multi-contributor, governance via geopython, well-known. Not single-author risk. |
| **OS4CSAPI/OSHConnect-Python** (fork of Botts) | Python 3.12 | MPL-2.0 | "Python client for CSAPI" — generic CSAPI surface plus 9-service publisher fleet (NWS/NDBC/CO-OPS/AviationWX/OpenSky/USGS×3/ISS) | **126 commits ahead of upstream**, 21 behind. Sam-Bolling primary. | Useful as a CSAPI-publisher reference; library API still single-author and thinly documented. MPL-2.0 mostly OK. |
| **Botts-Innovative-Research/OSHConnect-Python** | Python 3.12 | MPL-2.0 | Same library codebase, less divergence | Single primary contributor (`tipatterson-dev`) | Lower velocity than the OS4CSAPI fork. |
| **OS4CSAPI/ogc-csapi-explorer** | Vue 3 / Vite | BSD-3-Clause | Full UI over all 9 resource types; OL map; CRUD; OSH/52°N preconfigured proxies | Active; current branch `demo/acoustic-cuas-targeting` (telling) | Demo / explorer, not a product. Useful as a smoke-test client and architectural example. |
| **Botts-Innovative-Research/osh-viewer** | JS/Vue | **NO LICENSE** | Depends on `osh-js`; **OSH-specific, not generic CSAPI** | 1.0.0 Mar 2026 | **Reject** for generic CSAPI use. License missing = no rights to redistribute. |
| **Botts-Innovative-Research/oscar-viewer** | Next.js / TS | MPL-2.0 | Description: *"Web App for OpenSensorHub"* — OSH-specific | Last commit 2 wk ago | **Reject** for generic CSAPI use. |
| **Botts-Innovative-Research/ConnectedSystemsAPI-CPP** | C++ / vcpkg | MPL-2.0 | Skeletal — README is just the title; two folders (lib + test) | Very early; last commit 2 mo ago | Watch only. Not usable. |
| **byteroad/qgis-oacs-plugin** | Python | **GPL-3.0** | Pure Python QGIS plugin, lists 4 demo CSAPI servers | v0.0.4 (Feb 2026) | **GPL-3.0 = hard blocker for proprietary distribution.** Standalone QGIS extension only — never link or vendor into product. |
| **nsnarayanam/ogc-csapi-agent** | Python + Node | NO LICENSE | Gemini ADK agent wrapping `@camptocamp/ogc-client/csapi`; FastAPI mock CSAPI server included | Brand new (~1 wk) | **Diagnostic value only.** README enumerates 4 real interop bugs found against OSH demo: csapiCollections null-deref, `rel:"data"` vs `rel:"collections"`, parseCollections only flags `ogc-cs:*`, getCollectionDocument blind self-link follow. Treat as evidence the ecosystem has interop friction *right now*. |

### Tooling / Test / Aux

| Repo | Lang | License | Purpose | Maturity | Archetype A fit |
|---|---|---|---|---|---|
| **Botts/ets-ogcapi-connectedsystems10** | Java 17 / Maven / TestNG | **Apache-2.0** | OGC TeamEngine 5.6.1 ETS — all 14 Part 1 conformance classes. **Part 2 explicitly out of scope.** Dockerized. | Very active (commits hours ago) | **Critical.** This is the artifact for the *"deliver a conformance report"* contract requirement. Pair with ogc-cite/teamengine. Note: Part 2 gap means you cannot certify Part 2 via this ETS yet. Sibling `csapi_compliance` (frozen Next.js dev pre-flight) supplements. |
| **tipatterson-dev/OCSASim** | Python 3.12 / uv / Docker | (not shown) | Two-container OSH+sim stack; counter & line-of-bearing sims; uses `oshconnect` from test.pypi.org | Single contributor, very recent | Targets `osh-addons@mqtt-csapi-pt3-update` branch — **CSAPI Part 3 (Pub/Sub) work**. Watch as Part 3 evidence. |
| **tipatterson-dev/SWECommonBinaryEncodings** | Cap'n Proto + FlatBuffers + Protobuf | MIT | Schema-only; covers OGC 24-014 SWE Common 3.0 in three binary formats | Brand new (1 wk); single author; **AI-generated, explicitly not authoritative** | Useful starting point for binary payload work, but README warns: *"neither the code nor the docs should be treated as authoritative without independent verification against OGC 24-014."* Do not ship without your own review. |
| **OS4CSAPI/osh-node-dev-template** (fork) | Java 17 / Gradle | (OSH = MPL-2.0) | Template for OSH driver/process modules | Stale (~last year) | "I have my own sensor → OSH driver" path. Only relevant if OSH is selected. |

### OS4CSAPI Org — Stale / WIP (do not use)

`ogc-client-CSAPI_2`, `ogc-client-CSAPI`, `ogc-client-2026-homework`, `ogc-client-first-attempt`, `ogc-client-homework` — all earlier development phases of what became `OS4CSAPI/ogc-client`. Skip.

`os4csapi-meta`, `.github` — coordination/profile only.

---

## What This Tells Us About the Ecosystem (Bedrock Facts)

1. **Server reference cleanliness ranks: CS GO (MIT, full Part 1+2) > OSH-core (MPL-2.0, mature but heavyweight) > 52°N pygeoapi (PoC, do not field).** HAKUNAPI is not a CSAPI server.

2. **Part 2 conformance has no certification path yet.** ETS covers Part 1 only. CS GO claims Part 2 conformance URIs but there is no OGC test suite to verify.

3. **Part 3 (Pub/Sub) is in active early experimentation** on `osh-addons@mqtt-csapi-pt3-update` with OCSASim as the test rig. Nothing certified.

4. **The two strongest non-OSH client paths are:**
   - **TypeScript/JavaScript**: `OS4CSAPI/ogc-client` — CSAPI sub-path is real but upstream PR #136 not merged. Single primary contributor.
   - **Python**: `geopython/OWSLib` — multi-contributor, BSD-3, mature, but CSAPI support is **under-advertised** (README never mentions it; lives in `owslib/ogcapi/`). This is probably the lowest-risk enterprise Python path and is **not promoted on the dev site**.

5. **OSH coupling is everywhere in the Botts surface.** osh-viewer, oscar-viewer, ConnectedSystemsAPI-CPP, OSHConnect-Python all assume OSH semantics or osh-js. None can be lifted as a generic CSAPI viewer without rework.

6. **The ecosystem has documented live interop bugs.** `ogc-csapi-agent` is itself evidence: a working CSAPI agent had to discover and document 4 distinct bugs to make ogc-client talk to OSH. Plus the 52°N demo's `/conformance` is missing CSAPI URIs. **This contradicts the dev-site's "USE IT" framing of these as production-ready.**

7. **License blockers identified:**
   - `qgis-oacs-plugin` = GPL-3.0 → standalone tool only, never vendor.
   - `osh-viewer` = no LICENSE file → no rights to redistribute or modify.
   - `ogc-csapi-agent` = no LICENSE file → diagnostic reference only.

8. **CS GO README cites Part 2 as "OGC 24-008".** The spec is OGC 23-002. Likely typo in CS GO README. Worth filing.

---

## Required Microsite Corrections

**Dev site `ogc-csapi-website`, USE IT table:**
- Remove HAKUNAPI row, or move to a separate "OGC API – Features (not CSAPI)" section. README is unambiguous.
- Consider adding a maturity column: PoC / Active-WIP / Reference / OSH-coupled.
- OWSLib should be added as a CSAPI Python client (currently absent or underweighted).

**Code-sprint participation guide `code-sprint-guide`:**
- Remove HAKUNAPI from the Servers list under builder-days-2026, or annotate "OGC API Features only — not CSAPI".
- Consider noting which servers have known conformance gaps (52°N demo missing CSAPI URIs per ogc-csapi-agent docs).

I have not edited either microsite. Proposed changes pending your review.

---

## Gaps (No Existing Project Fits an Archetype A NN Requirement)

Pre-stating what we already know is missing, *before* we draft NNs:

- **No certified Part 2 conformance test.** Customer needs proof of Part 2 compliance → must build / contribute.
- **No FIPS-validated TLS toolkit** in any of these. Every server uses default crypto (Go stdlib, Java JSSE, Python ssl). Defense FIPS deployment requires extra layer.
- **No air-gap distribution story.** All projects assume internet-pulled deps (Maven Central, GitHub Packages, npm, PyPI, vcpkg). No vendored / mirrored bundle exists.
- **No SensorML-free CSAPI-only pathway documented.** OSH and SWECommonBinaryEncodings assume SensorML literacy. CS GO's README marks SensorML "planned" — minimal-SensorML production posture is not yet illustrated.
- **No Part 3 production reference.** Part 3 work is at OCSASim-experiment level.
- **No proprietary-metadata extension pattern documented.** All examples use canonical SOSA/SSN.

---

## Files Read (Primary Evidence Sources)

Servers: `SomethingCreativeStudios/connected-systems-go` (README + IMPLEMENTATION.md), `OS4CSAPI/connected-systems-go`, `opensensorhub/osh-core`, `opensensorhub/osh-addons`, `52North/connected-systems-pygeoapi`, `nlsfi/hakunapi`.

Clients: `OS4CSAPI/ogc-client`, `geopython/OWSLib` (root + `owslib/ogcapi/`), `OS4CSAPI/OSHConnect-Python`, `Botts-Innovative-Research/OSHConnect-Python`, `OS4CSAPI/ogc-csapi-explorer`, `Botts-Innovative-Research/osh-viewer`, `Botts-Innovative-Research/oscar-viewer`, `Botts-Innovative-Research/ConnectedSystemsAPI-CPP`, `byteroad/qgis-oacs-plugin`, `nsnarayanam/ogc-csapi-agent`.

Tooling: `Botts-Innovative-Research/ets-ogcapi-connectedsystems10`, `tipatterson-dev/OCSASim`, `tipatterson-dev/SWECommonBinaryEncodings`, `OS4CSAPI/osh-node-dev-template`.

Microsites + org: `os4csapi.github.io/ogc-csapi-website/`, `os4csapi.github.io/code-sprint-guide/builder-days-2026/`, `github.com/orgs/OS4CSAPI/repositories`.
