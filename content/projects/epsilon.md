---
title: "Epsilon — Privacy-Preserving Research Infrastructure"
date: 2024-07-01
tags: ["TEE", "ZKP", "Privacy", "AWS Nitro", "Python", "React"]
summary: "A zero-trust platform combining TEE execution, ZKP verification, AI policy checking, and visual schema mapping for sensitive research data."
weight: 1
---

## Overview

Epsilon is a privacy-preserving research infrastructure that provides cryptographic guarantees for every data execution. Researchers can run code on sensitive datasets inside AWS Nitro Enclaves, with Zero-Knowledge Proofs binding the specific code, data, and timestamp.

## Key Features

- **Visual Archetype Editor** — Tree-based schema mapping for data access configuration
- **AI Policy Agent** — Multi-agent system (CrewAI + GPT-4) for automated compliance checking
- **TEE Execution** — AWS Nitro Enclaves with COSE_Sign1 attestation documents
- **ZKP Verification** — Groth16 proofs for per-execution binding
- **Trust Center** — Public verification portal for attestation documents

## Tech Stack

`Python` `FastAPI` `React` `AWS Nitro Enclaves` `CrewAI` `Circom` `SnarkJS` `Docker`

## Publications

- SysTEX 2026 (tool paper, submitted)
- PoPETs 2027 (in preparation)
- CHI 2027 (evaluation paper, in preparation)
