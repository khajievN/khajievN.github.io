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

**Trusted Research Environments (TREs)** are the standard infrastructure for working with sensitive health, social, and government data. But they have critical limitations:

| Problem | Current State | Impact |
|---------|--------------|--------|
| **Data sovereignty** | Researchers can't bring their own datasets | Limits research scope |
| **Manual policy enforcement** | Weeks of approval delays for each data request | Slows research by months |
| **No execution verification** | No proof that code ran correctly on the right data | Trust is assumed, not verified |

### My Approach

Epsilon integrates four mechanisms that map to the [Five Safes framework](https://www.ukdataservice.ac.uk/manage-data/legal-ethical/access-control/five-safes):

#### RQ1: Archetypes — Visual Schema Mapping
*How can adaptive schema abstraction support BYOD environments?*

Archetypes are visual, hierarchical schema mappings that let data custodians define exactly what data researchers can access. Instead of flat checkbox lists, custodians build tree structures that make data relationships and sensitivity visible — enabling better data minimization decisions.

**Status:** Completed and integrated.

#### RQ2: AI-Assisted Policy Checking
*How can AI agents reduce manual review bottlenecks?*

A multi-agent system (built with CrewAI + GPT-4) that automatically checks whether a researcher's data request complies with institutional policies. The AI analyzes research purpose, data sensitivity, and proportionality — flagging concerns for human review rather than replacing human judgment.

**Status:** Proof of concept completed.

#### RQ3: Per-Execution Cryptographic Verification
*How can we provide verifiable proof that specific code ran on specific data?*

This is the core technical contribution. Current TEE platforms (Phala, dstack) prove: *"This application runs in a TEE."* Epsilon extends this to: *"Your specific code ran on your specific dataset at this specific time"* — using Zero-Knowledge Proofs (Groth16/BN128) combined with AWS Nitro Enclave attestation.

**Status:** In development (ZKP circuit implementation).

### Architecture

The full pipeline: **Data upload → Archetype configuration → AI policy check → TEE execution → ZKP proof generation → Verifiable output**

Built with:
- **Frontend:** React (visual schema mapper)
- **Backend:** Python, FastAPI
- **AI:** CrewAI framework, OpenAI GPT-4
- **Security:** AWS Nitro Enclaves
- **Cryptography:** Circom, SnarkJS, Groth16/BN128

### What Makes This Different

| Existing Systems | Epsilon |
|-----------------|---------|
| Platform attestation ("app runs in TEE") | **Per-execution** binding (code + data + timestamp) |
| Top-down access control | **Participatory** visual schema mapping |
| Manual policy review (weeks) | **AI-augmented** review (minutes) |
| Trust the institution | **Verify cryptographically** |
