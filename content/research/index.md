---
title: "Research"
layout: "single"
url: "/research/"
summary: "My research on privacy-preserving research infrastructure"
ShowToc: true
---

## Epsilon: Privacy-Preserving Research Infrastructure

My PhD research addresses three fundamental problems with how sensitive data is handled in research environments today.

### The Problem

**Trusted Research Environments (TREs)** are the standard infrastructure for working with sensitive health, social, and government data. The UK alone operates over 30 TREs serving thousands of researchers. But they have critical limitations:

| Problem | Current State | Impact |
|---------|--------------|--------|
| **Data sovereignty** | Researchers can't bring their own datasets | Limits cross-institutional analysis |
| **Manual policy enforcement** | Weeks of approval delays for each code submission | Slows research by months |
| **No execution verification** | No proof that code ran correctly on the right data | Trust is assumed, not verified |
| **No accountability** | Operator controls the audit trail | Suppression/deletion is undetectable |

### My Approach

Epsilon integrates four mechanisms that map to the [Five Safes framework](https://www.ukdataservice.ac.uk/manage-data/legal-ethical/access-control/five-safes):

#### RQ1: Archetypes — Visual Schema Mapping
*How can adaptive schema abstraction support BYOD environments?*

Archetypes are visual, hierarchical schema mappings that let data custodians define exactly what data researchers can access. Built with React and XYFlow, they enable data minimization decisions through visual hierarchy rather than flat checkbox lists.

**Status:** Completed and integrated. Targeting CHI 2027 for usability evaluation.

#### RQ2: AI-Assisted Policy Checking
*How can AI agents reduce manual review bottlenecks?*

A 3-agent pipeline (CrewAI + GPT-4o-mini) combining deterministic AST scanning with LLM reasoning. The AST scanner catches prohibited operations in <1ms; the LLM layer reasons about data privacy violations (PII leakage, k-anonymity, output disclosure).

Evaluated on 121 test submissions including 69 real-world scripts from OpenSAFELY. F1 = 0.958. PII detection improved from 0% (deterministic only) to 83% (with LLM reasoning). All agents operate outside the TCB — they see code (already on GitHub) but never patient data.

**Status:** Completed. Targeting ICSE 2027 with TREBench benchmark.

#### RQ3: Per-Execution Attestation + Transparency Log
*How can we provide verifiable, accountable proof of execution?*

Two complementary mechanisms:

1. **Per-execution TEE attestation**: Each computation inside a Nitro Enclave produces a hardware-signed attestation document binding the script hash, dataset hash, and output hash. Unlike platform-level attestation (Phala, dstack), this proves *"your specific code ran on your specific dataset at this specific time."*

2. **Attestation Transparency Log (ATL)**: A Certificate Transparency-style append-only Merkle tree that records every attestation document. Makes suppression or deletion cryptographically detectable by any party with an inclusion receipt. Includes Job Acceptance Commitments for gap detection, STH-derived freshness nonces for replay prevention, and context commitments for metadata integrity.

The ATL design is TEE-agnostic: any platform with hardware-signed attestation and an application-defined data field (Nitro user_data, SGX report_data, TDX, SEV-SNP, ARM CCA) can serve as the Attester.

**Status:** Implemented (Go, 1,800 LOC). SysTEX 2026 accepted. TDSC 2026 in progress (deadline June 1).

### What Makes This Different

| Existing Systems | Epsilon |
|-----------------|---------|
| Platform attestation ("app runs in TEE") | **Per-execution** binding (code + data + output) |
| Operator controls the audit trail | **Append-only Merkle tree** — suppression is detectable |
| Data uploaded to platform in plaintext | **Source-side encryption** — proxy encrypts at source |
| Manual code review (weeks) | **AI agents** reduce to seconds (F1 = 0.958) |
| Top-down access control | **Visual archetypes** for participatory data governance |

### Publication Strategy

| Paper | Venue | Status |
|-------|-------|--------|
| SysTEX 2026 | EuroSys workshop | **Accepted** (Artifact: Available, Functional, Reusable) |
| TDSC 2026 | IEEE Transactions (IF 7.94) | In progress — ATL transparency log |
| Middleware 2026 | ACM Middleware (CORE A*) | In progress — full platform |
| ICSE 2027 | SE research track | Planned — AI agent benchmark (TREBench) |
| CHI 2027 | HCI research track | Planned — archetype usability study |
