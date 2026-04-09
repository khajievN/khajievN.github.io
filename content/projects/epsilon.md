---
title: "Epsilon — Privacy-Preserving Research Infrastructure"
date: 2024-07-01
tags: ["TEE", "Privacy", "AWS Nitro", "Python", "React", "Go", "NestJS", "CrewAI", "Transparency Log"]
summary: "A privacy-preserving platform (19 repos, 4 languages, 4 published packages) combining source-side encryption, TEE execution, AI policy checking, per-execution attestation, and an Attestation Transparency Log for sensitive research data."
weight: 1
---

## Overview

Epsilon is a privacy-preserving research infrastructure for Trusted Research Environments (TREs). Researchers run analysis code on sensitive datasets inside AWS Nitro Enclaves, with per-execution attestation binding specific code, data, and output in a single hardware-signed record.

**Scale:** 19 repositories, 4 languages (Python, TypeScript, Go, Bash), 9 live services, 4 published packages.

**Status:** SysTEX 2026 accepted (ACM Artifact: Available, Functional, Reusable). TDSC and Middleware papers in progress.

## Key Innovations

| Innovation | What it solves |
|-----------|---------------|
| **Source-side encryption (epsilon-proxy)** | Data owner encrypts at source; no platform component sees plaintext |
| **Per-execution attestation** | Hardware proof binding YOUR code + YOUR data + THIS time |
| **Attestation Transparency Log (ATL)** | CT-style append-only Merkle tree — suppression/deletion is cryptographically detectable |
| **AI policy agents** | 3-agent pipeline (CrewAI + GPT-4o-mini) reduces manual code review from weeks to 36 seconds |
| **Visual archetypes** | Schema mapping for BYOD — any format to archetype via React + XYFlow |

## Architecture

```
DATA OWNER                          CLOUD (AWS)
──────────                          ───────────
PostgreSQL                          API (NestJS)
    ↓                                   ↓
epsilon-proxy ──attestation verify──→ Coordinator
    ↓ encrypt(pub_key)                  ↓ AI Agents (CrewAI)
    ↓                                   ↓
rathole tunnel ─── Noise protocol ──→ Nitro Enclave (TEE)
                                        ↓ decrypt + execute + attest
                                    ATL Service (Go)
                                        ↓ Merkle tree + COSE_Sign1 receipt
                                    Trust Center (React)
                                        ↓ client-side verification
```

## Source-Side Encryption

The epsilon-proxy is a Go binary data owners install next to their database. The enclave generates an RSA keypair (private key stays inside), the proxy verifies the enclave's attestation document against AWS root certificate, then encrypts query results with the enclave's public key. No platform component — coordinator, API, cloud infrastructure — ever sees plaintext data. Zero inbound ports needed on the data owner's side.

## AI Policy Agents

A 3-agent pipeline combines deterministic AST scanning with LLM reasoning:
- **AST Scanner**: Catches prohibited operations (network, system calls, code injection) in <1ms
- **LLM Agents**: Reason about data privacy violations (PII leakage, k-anonymity, output disclosure)

Evaluated on 121 test submissions (52 curated + 69 real-world OpenSAFELY scripts). F1 = 0.958. The LLM layer improved PII detection from 0% to 83%.

## Attestation Transparency Log (ATL)

A Certificate Transparency-style append-only Merkle tree log for per-execution TEE attestation documents. Makes suppression or deletion of execution records cryptographically detectable. Includes:
- Inclusion proofs (O(log n), portable, offline-verifiable)
- Consistency proofs (append-only guarantee)
- Job Acceptance Commitments (gap detection)
- STH-derived freshness nonces (replay prevention)

Evaluated on 1.54M entries: median submission latency 6.1ms, 0.045% overhead relative to enclave execution.

## Performance

| Operation | Time |
|-----------|------|
| Cold start (EC2 + enclave boot) | ~31s |
| Warm execution (data ingestion + script) | ~18s |
| Script execution inside enclave | ~53ms |
| NSM attestation generation | ~9ms |
| ATL submission (server-side) | ~6.1ms |
| AI policy analysis | ~36s |
| Attestation document size | ~4.8 KB |

## Published Packages

| Package | Registry | Install |
|---------|----------|---------|
| @epsilon-data/nitro-verify | npm | `npm install @epsilon-data/nitro-verify` |
| epsilon-attestation-verifier | PyPI | `pip install epsilon-attestation-verifier` |
| epsilon-sdk | PyPI | `pip install epsilon-sdk` |
| epsilon-proxy | GitHub | `curl -fsSL https://get.epsilon-data.org \| sh` |

## Live Services

| Service | URL |
|---------|-----|
| Main Platform | https://app.epsilon-data.org |
| Analysis Hub | https://analysis.epsilon-data.org |
| Trust Center | https://trust.epsilon-data.org |

## Tech Stack

`Python` `TypeScript` `Go` `React 19` `NestJS 11` `AWS Nitro Enclaves` `CrewAI` `GPT-4o-mini` `Docker` `PostgreSQL` `CBOR` `COSE_Sign1` `Ed25519` `RSA-OAEP` `AES-256-CBC`

## Publications

- **SysTEX 2026** — Tool paper (Accepted, ACM Artifact badges)
- **IEEE TDSC 2026** — ATL transparency log (In progress, June 2026)
- **ACM Middleware 2026** — Full platform (In progress, June 2026)
- **ICSE 2027** — AI agent evaluation (Planned)
- **CHI 2027** — Archetype usability study (Planned)
