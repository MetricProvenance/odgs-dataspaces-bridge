# Open Data Governance Standard (ODGS) — Reference Architecture & Dataspace Profile (v6.1.0)

`odgs` (`v6.1.0`) is the open-source, Apache-2.0 reference implementation of the **Open Data Governance Standard (ODGS)**. It connects deterministic in-pipeline data-governance enforcement (`ISO/IEC 38505-1:2026`, `ISO/IEC 25012:2008`, `ISO 8000-8`) to open dataspace control planes and conformity assessment vocabularies:

- **ISO/IEC JTC 1/SC 40 & SC 7 (`ISO/IEC 38505-1:2026`, `ISO/IEC TR 38505-2:2018`, `ISO/IEC 25012:2008`, `ISO 8000-8`, `ETSI EN 304 311`)**
- **Eclipse Dataspace Working Group (`CAP v2.2.1`, `DRP`, `DSP`, `DCP`)**
- **Gaia-X (`ODRL-VC Profile` & `Data Transfer Agent (DTA)` Phase 6)**
- **IDSA Rulebook (`2026-3` Decentralized Observability §121)**
- **W3C ODRL 2.2, W3C Verifiable Credentials v2.0 (`cred:Evidence`), W3C PROV-O & NIST OSCAL v1.1.2**

## Why Runtime Enforcement Complements Control-Plane Negotiation

Dataspace control planes (**Eclipse DSP/DCP**, **W3C ODRL 2.2**, **Eclipse DRP**, and the **Gaia-X Data Transfer Agent**) govern **pre-transfer access negotiation and credential verification**. Once the bytes land inside the consumer's data or AI pipeline, post-transfer usage obligations (`odrl:Duty`, `odrl:Prohibition`, `drp:environment.licenses`) and `ISO/IEC 25012:2008` data quality constraints must be enforced at the point of processing, and the consumer must return cryptographic proof of enforcement without exposing confidential data.

`odgs` provides:

1. **ISO-Grounded Contextual Governance (`1_NORMATIVE_SPECIFICATION/schemas/`):** Separates corporate governance of data (`ISO/IEC 38505-1:2026` Evaluate–Direct–Monitor) from operational data management, binds all 60 data quality dimensions to `ISO/IEC 25012:2008` and `ISO 8000-8`, and resolves constraints contextually per business process (`context_bindings.json`).
2. **Deterministic Policy Binding (`odgs.dataspace.odrl_binder`):** Compiles W3C ODRL 2.2 agreements, Gaia-X ODRL-VC constraints, and Eclipse DRP `delegationEvidence` tokens into deterministic boolean rules anchored by a SHA-256 `semantic_hash`.
3. **Proportional In-Pipeline Enforcement (`OdgsInterceptor` & `DataspaceRuntimePEP`):** Evaluates payloads at runtime (`HARD_STOP` Administrative Recusal, `SOFT_STOP` with logged override token, `LOG_ONLY`) and emits a zero-knowledge **Semantic Certificate (`S-Cert`)**.
4. **Standardised Evidence Export (`odgs.dataspace.cap_exporter` & `dta_verifier`):** Formats `S-Cert` receipts as **Eclipse CAP `cred:Evidence`**, verifies **Gaia-X DTA** post-transfer obligations, and exports **W3C PROV-O** and **NIST OSCAL** records.
