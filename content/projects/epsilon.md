---
title: "Epsilon — Privacy-Preserving Research Infrastructure"
date: 2024-07-01
tags: ["TEE", "ZKP", "Privacy", "AWS Nitro", "Python", "React", "Go", "NestJS"]
summary: "A zero-trust platform (13 repos, 4 languages) combining TEE execution, ZKP verification, AI policy checking, and visual schema mapping for sensitive research data."
weight: 1
---

## Overview

Epsilon is a privacy-preserving research infrastructure that provides cryptographic guarantees for every data execution. Researchers can run code on sensitive datasets inside AWS Nitro Enclaves, with per-execution attestation binding specific code, data, and timestamp.

**Scale:** 13 repositories, 4 languages (Python, TypeScript, Go, Bash), 5 architectural layers.

## Architecture

```
LAYER 1 — RESEARCHER EXPERIENCE
  SDK (Python CLI) + Research Template + Job Submission Web App

LAYER 2 — DATA HUB
  React 19 Frontend (visual schema mapper) + NestJS API

LAYER 3 — DATA & METADATA
  Go Data-Broker (schema crawler) + Apache Atlas + Keycloak + Vault

LAYER 4 — SECURE EXECUTION
  Job Coordinator (4 workers) + Nitro Enclave (TEE)

LAYER 5 — VERIFICATION
  Attestation Verifier (CBOR/COSE) + Trust Center (React)
```

## End-to-End Flow

1. Researcher authenticates via SDK → Keycloak JWT
2. `epsilon init` → downloads archetype, generates models + dummy data
3. Write analysis script, test locally with `epsilon run`
4. Submit job → Lambda starts EC2 → boots Nitro Enclave (4GB, 2 CPU)
5. Coordinator pipeline: **fetch → clone → AI policy check → encrypt → execute in enclave**
6. Enclave: RSA-OAEP decrypt → AES-256-CBC decrypt → execute → NSM attestation
7. Output + COSE_Sign1 attestation document returned
8. Verify: certificate chain → PCR match → output hash binding

## Key Components

| Component | Tech | What It Does |
|-----------|------|-------------|
| **Frontend** | React 19, XYFlow, Vite | Visual archetype schema mapper, dataset browser |
| **API** | NestJS 11, Prisma | Auth, metadata, encryption, job queues |
| **Data-Broker** | Go 1.21 | Crawls DB schemas → Apache Atlas entities |
| **Coordinator** | Python, Docker | 4-worker pipeline (fetch, clone, AI, execute) |
| **Enclave** | Python, VSock | Decrypt → execute → attest inside Nitro TEE |
| **AI Agent** | CrewAI, GPT-4 | Policy/code/governance analysis |
| **Trust Center** | React 19, Express 5 | Public attestation verification portal |
| **SDK** | Python, Typer | CLI for researchers (login, init, run, build) |
| **Verifier** | Python | CBOR/COSE parsing, X.509 chain, PCR validation |

## Security Architecture

```
UNTRUSTED ZONE                    TRUSTED ZONE (Nitro Enclave)
─────────────                     ───────────────────────────
Coordinator containers            Hypervisor-isolated memory
API server, Lambda                No network, no disk, no SSH
ResearchWorkspace                 RSA keys in-memory only
                                  Decryption + execution
Vault: transit encryption         NSM attestation (/dev/nsm)
Keycloak: resource-based authz    Output hash in COSE_Sign1
                                  AWS Root CA trust anchor
```

## Performance

| Operation | Time |
|-----------|------|
| EC2 boot | ~30.6s avg |
| RSA keypair generation | ~50ms |
| Full execution (decrypt + run + attest) | ~265ms |
| NSM attestation | ~9ms |
| Attestation document size | ~4.8 KB |

## Platform Infrastructure

PostgreSQL (x3) · Redis · Keycloak · HashiCorp Vault · Apache Atlas · Elasticsearch · Cassandra · Kafka · Nginx — all orchestrated via Docker Compose.

## Tech Stack

`Python` `TypeScript` `Go` `React 19` `NestJS 11` `AWS Nitro Enclaves` `CrewAI` `GPT-4` `Docker` `Keycloak` `Apache Atlas` `HashiCorp Vault` `Kafka` `Redis` `PostgreSQL`

## Publications

- SysTEX 2026 — Tool paper (submitted)
- PoPETs 2027 — System evaluation (in preparation)
- CHI 2027 — Archetype evaluation (in preparation)
