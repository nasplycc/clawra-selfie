# Clawra Selfie

<p align="center">
  <img src="assets/clawra.png" alt="Clawra" width="220" />
</p>

<p align="center">
  <strong>Clawra-inspired selfie generation for OpenClaw</strong><br/>
  A practical, free-first image workflow using Hugging Face as the active backend.
</p>

<p align="center">
  <img alt="status" src="https://img.shields.io/badge/status-working-3fb950">
  <img alt="backend" src="https://img.shields.io/badge/backend-HuggingFace-fcc72b">
  <img alt="gemini" src="https://img.shields.io/badge/Gemini-disabled_by_default-6f42c1">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-blue">
</p>

---

## What this is

This project is inspired by [SumeLabs/clawra](https://github.com/SumeLabs/clawra), but adapted for a different practical goal:

- keep the **Clawra / Raya-style image roleplay feeling**
- integrate naturally into an **OpenClaw-native workflow**
- avoid making **paid image backends** the default requirement
- use a **working free Hugging Face route** today
- preserve a clean future path toward **stronger face consistency** later

In short:

> **Clawra vibe, OpenClaw-native workflow, free-first backend strategy.**

---

## Why this exists

The original Clawra concept is genuinely compelling, but some of the more powerful model routes require paid usage.

This version takes the more practical route:

1. make it **work reliably first**
2. keep the experience **good enough for daily use**
3. structure it so stronger backends can be swapped in later

That means the current version optimizes for:

- usability
- simplicity
- low cost
- future upgradeability

---

## Current capabilities

- **Prompt-driven selfie / daily-life image generation**
- **Direct mode** for close-up selfie or current-state shots
- **Mirror mode** for gym / outfit / half-body / full-body compositions
- **Soft official-face mechanism** using a workspace reference image
- **Fixed face anchor + negative anchor** for better consistency
- **OpenClaw channel-send compatible workflow**
- **Gemini image probe path exists but is disabled by default**
- **Hugging Face remains the active stable backend**

---

## Backend strategy

### Active default
- **Hugging Face**
- default model: `black-forest-labs/FLUX.1-schnell`

### Present but disabled by default
- **Gemini image probe**

Gemini support exists in the script, but current testing showed the image-generation route was not reliably usable as a free default path, so it is intentionally disabled unless explicitly re-enabled.

```bash
ENABLE_GEMINI=1
GEMINI_API_KEY=your_google_gemini_api_key
```

---

## Identity consistency strategy

This project currently uses **soft identity consistency**, not hard face lock.

It combines:

- a fixed **FACE_ANCHOR**
- a fixed **NEGATIVE_ANCHOR**
- an **official face reference file path**
- scenario-aware prompt shaping

### Official face files

The script checks these workspace paths:

- `references/raya-official-face-current.png`
- `references/raya-official-face-current.jpg`
- `references/raya-official-face-current.jpeg`
- `references/raya-official-face-v1.png`
- `references/raya-official-face-v1.jpg`
- `references/raya-official-face-v1.jpeg`

If one exists, it becomes the current soft identity anchor.

### Important limitation

Because the active Hugging Face free route is still mostly **text-to-image**, this project can currently offer:

- stable character vibe
- approximate facial structure consistency
- repeatable role direction

It **cannot guarantee an identical face every single run**.

If stronger identity consistency is needed later, the real upgrade path is a better backend — not endless prompt decoration.

---

## Modes

### Direct mode
Best for:
- close-up selfie
- current mood / status
- casual candid shots
- daily-life moments

### Mirror mode
Best for:
- mirror-area shots
- outfit photos
- half-body / full-body framing
- gym / campus / indoor scenes

Mirror mode in this project **does not require holding a phone by default**.

---

## Project structure

```text
clawra-selfie/
├─ assets/
│  └─ clawra.png
├─ examples/
│  └─ README.md
├─ scripts/
│  ├─ clawra-selfie.sh
│  └─ clawra-selfie.ts
├─ .gitignore
├─ CHANGELOG.md
├─ LICENSE
├─ README.md
└─ SKILL.md
```

### Key files

- `scripts/clawra-selfie.sh`
  - main backend flow
  - prompt assembly
  - fallback behavior
  - output writing

- `scripts/clawra-selfie.ts`
  - lightweight Node wrapper

- `SKILL.md`
  - OpenClaw skill-facing usage guidance

- `README.md`
  - repository-facing project overview

---

## Requirements

### Required
- `bash`
- `curl`
- `jq`
- OpenClaw environment
- Hugging Face token

### Environment variables

```bash
HF_TOKEN=your_huggingface_token
HF_IMAGE_MODEL=black-forest-labs/FLUX.1-schnell
HF_API_BASE=https://router.huggingface.co/hf-inference/models
```

### Optional Gemini variables

```bash
ENABLE_GEMINI=1
GEMINI_API_KEY=your_google_gemini_api_key
GEMINI_IMAGE_MODEL=gemini-2.5-flash-image
```

---

## Usage

### Shell

```bash
HF_TOKEN=your_token \
./scripts/clawra-selfie.sh \
  "at the gym, post-workout mirror-area fitness snapshot" \
  "telegram" \
  "mirror" \
  "健身房状态 ✨"
```

### Node wrapper

```bash
node ./scripts/clawra-selfie.ts \
  "in class, side profile from deskmate perspective" \
  "telegram" \
  "mirror" \
  "上课时的侧颜 ✨"
```

Arguments:

1. `user_context` — what she is doing / wearing / where she is
2. `channel` — target provider name
3. `mode` — `direct`, `mirror`, or `auto`
4. `caption` — output caption

---

## Example scenarios

- morning selfie
- breakfast shop full-body shot
- campus classroom candid
- post-workout gym state
- city street snapshot
- coffee shop side profile
- after-class running scene

More examples: [`examples/README.md`](examples/README.md)

---

## Known limitations

- Hugging Face free generation is **not true reference-image editing**
- identity consistency is weaker than paid or local advanced pipelines
- exact same face cannot be guaranteed every run
- free backends may change behavior, availability, or limits over time

---

## Roadmap

- [x] working Hugging Face image generation path
- [x] direct / mirror mode split
- [x] official-face soft anchor mechanism
- [x] GitHub-ready project packaging
- [ ] stronger reference-image editing backend
- [ ] configurable workspace / path abstraction
- [ ] more polished example gallery
- [ ] optional dataset curation flow docs
- [ ] future LoRA-ready workflow integration

---

## Inspiration

- Original inspiration: [SumeLabs/clawra](https://github.com/SumeLabs/clawra)
- Adaptation goal: make a usable, OpenClaw-native, lower-cost version for daily image-roleplay workflows

---

## Status

This repository currently reflects a working private-repo version with:

- Hugging Face default generation
- Gemini disabled by default
- official-face soft anchor support
- prompt-anchored Raya consistency workflow

It is already usable now, while still leaving room for a much stronger second stage later.
