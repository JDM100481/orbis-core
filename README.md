# Orbis  
[![Offline-First](https://img.shields.io/badge/offline-first-green.svg)]() [![Privacy by Design](https://img.shields.io/badge/privacy-by--design-orange.svg)]() [![Local AI](https://img.shields.io/badge/llm-local-blue.svg)]()

**Orbis** (Offline Reasoning and Bound iNTELLIGENT System) is a self-hosted, offline-first AI environment that understands you, builds with you, and evolves beside you.

Orbis is designed to act like a personal operating system for your thinking, negotiations, obligations, and health.  
It captures context, reasons locally, helps you make decisions, and can generate/execute useful functionality — without sending your data to anyone.

> “Jarvis, but sovereign.”

---

## TL;DR

Orbis = Offline-first AI + Private Memory + Self-expanding Builder.

- Runs fully on your own hardware (Proxmox VM, no cloud dependency)
- Captures voice, agreements, risks, promises, and stress signals
- Gives you continuity: “What did I commit to, to whom, and what’s the risk?”
- Can extend itself by generating new modules, scripts, and tooling
- Cannot talk to the internet unless you explicitly allow it, one-time only

This is not SaaS. This is yours.

---

## ✨ Core Principles

### 🔒 1. Offline-First
- Default: no outbound network access.
- No silent telemetry, no “phone home.”
- You physically own the machine that runs Orbis.

When Orbis needs outside info (ex: new policy, law, or news), it must:
1. Ask you for permission
2. Temporarily open outbound network for that one request
3. Log what it did
4. Lock itself offline again

### 🧠 2. Understands You
Orbis tracks:
- what you said you’d deliver,
- who expects what from you,
- what pressure you’re under,
- where the real risks are.

That memory is stored in your private vault. Not in someone else’s server.

### 🛠 3. Builds With You
This is not just “chat.”

You can say:
- “Orbis, draft a script to log uptime of my Proxmox VMs.”
- “Orbis, summarize what I promised to SK Federation and flag the deadlines.”
- “Orbis, generate a kiosk rollout plan and list all political risks.”

The vision: Orbis can generate new modules, then execute them in a controlled sandbox.

### 🧩 4. Modular
Orbis is Lego.

You start with:
- Core reasoning (local LLM via Ollama)
- Voice interface (Whisper.cpp + TTS)
- Memory / evidence vault
- Privacy firewall

Then you can add:
- Smart glasses capture (field audio/video for proof-of-agreement)
- Smartwatch biometrics (stress / fatigue awareness)
- Smart mirror / home console
- Vehicle / GPS context
- IoT / kiosk nodes

Core still works without any of those. Ikaw pa rin ang may control.

---

## 🏗 System Overview

Orbis is split into three major layers:

### 1. Orbis Core (Brain)
Runs in a VM (Ubuntu Server) under Proxmox.

Components:
- **Local LLM Engine (`core/llm/`)**  
  Uses Ollama to run local models (Mistral, Phi, etc.) for reasoning and planning.
- **Speech Stack (`core/voice/`)**  
  Whisper.cpp for offline STT (speech → text).  
  Local TTS for response audio.
- **Memory Engine (`core/memory/`)**  
  SQLite + Markdown logs of: meetings, commitments, risks, stress notes, negotiations.
- **Privacy Layer (`core/privacy/`)**  
  Firewall guard. Default: deny all outbound traffic.
- **Executor Layer (`core/executor/`) [future]**  
  Controlled sandbox where Orbis can generate small scripts/automations and run them locally.

### 2. Interface Layer (`interfaces/`)
How you talk to Orbis.

- **Thin Client / Console**  
  e.g. an all-in-one PC acting like your “Jarvis console.”
- Planned:  
  - Local web dashboard  
  - Voice console with wake word (“Hey Orbis”)  
  - Physical mute / camera shutter switches

### 3. Data Layer (`data/`)
Your captured world.

- `data/vault/`  
  Raw audio, screenshots, photos of signed agreements, video snippets.
- `data/vault/processed/` (future)  
  OCR text, transcripts, summaries.
- `data/affine_workspace/`  
  Human-readable journal + commitments + risk ledger.

Why this matters:
- You can prove what was agreed.
- You can track exposure (political, financial, legal).
- You can hand context to a future COO / successor without giving them your brain.

---

## 📂 Repository Structure

```text
orbis-core/
├─ core/
│   ├─ voice/          # Voice in/out (Whisper.cpp, TTS)
│   ├─ llm/            # Local reasoning via Ollama
│   ├─ memory/         # Journaling, long-term memory
│   ├─ executor/       # (future) script/automation runner
│   └─ privacy/        # Firewall guard, mic/cam kill logic
│
├─ interfaces/
│   ├─ web/            # Local-only dashboard
│   ├─ voice_console/  # Wake word / push-to-talk loop
│   └─ api/            # LAN-local API surface
│
├─ data/
│   ├─ vault/              # Raw media (audio, images, scans)
│   ├─ affine_workspace/   # Commitments, risks, daily briefs
│   └─ cache/
│
├─ bootstrap/
│   ├─ start_core.sh       # Bring Orbis Core up in offline mode
│   ├─ start_interface.sh  # Start dashboard / console
│   └─ start_secure.sh     # Enforce privacy lock + audit
│
├─ config/
│   ├─ privacy_policy.yaml   # offline rules, one-time web rule
│   ├─ model_config.yaml     # which LLM/STT/TTS engines run
│   └─ orbis_identity.yaml   # Orbis "personality"/stance
│
├─ docs/
│   ├─ architecture.txt      # notes for system diagram
│   └─ banner.txt            # branding tagline, visual direction
│
└─ README.md                 # you are here
