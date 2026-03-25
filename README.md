# Clawra Selfie

A lightweight **Clawra-inspired selfie/image skill** for OpenClaw.

This project was inspired by [SumeLabs/clawra](https://github.com/SumeLabs/clawra), but adapted for a different practical goal:

- keep the **Clawra / Raya-style roleplay image experience**
- avoid paid image backends as the default path
- use a **free Hugging Face generation route** as the current working backend
- preserve a future upgrade path for **reference-image editing**, **paid APIs**, or **LoRA-based consistency**

## Why this exists

The original Clawra flow is inspiring, but some of its model/provider choices are paid.
For this project, the goal was to build a version that:

- works inside an OpenClaw agent workflow
- can generate selfie-like / life-state images on demand
- can send results directly to messaging channels
- can stay usable on a free stack for experimentation

In short:

> **Clawra vibe, OpenClaw-native workflow, Hugging Face free-tier fallback.**

---

## Features

- **OpenClaw skill-style structure**
- **Prompt-driven image generation** for selfie / daily-life / scenario images
- **Direct mode** for close-up current-state selfie style
- **Mirror mode** for half-body / full-body / mirror-area compositions
- **Soft official-face mechanism** via a workspace reference image
- **Fixed anchor prompts** for better identity consistency
- **Negative anchor prompts** to reduce drift
- **Telegram / channel send workflow support** through OpenClaw tooling
- **Gemini probe path exists but is disabled by default**
- **Hugging Face remains the default active backend**

---

## Current backend strategy

### Active default
- **Hugging Face**
- Current default model:
  - `black-forest-labs/FLUX.1-schnell`

### Optional but currently disabled
- **Gemini image probe**
- Disabled by default because current testing showed:
  - image-generation free-tier quota may be unavailable (`limit: 0`)
  - not reliable enough as the default path

Gemini can be re-enabled later if billing or quotas change.

---

## Identity consistency approach

This project currently uses **soft consistency**, not true face-locking.

That means it combines:

- a fixed **FACE_ANCHOR**
- a fixed **NEGATIVE_ANCHOR**
- an **official face reference file path**
- careful prompt shaping for scenario-specific generation

### Official face reference

The script checks the following files in the workspace:

- `references/raya-official-face-current.png`
- `references/raya-official-face-current.jpg`
- `references/raya-official-face-current.jpeg`
- `references/raya-official-face-v1.png`
- `references/raya-official-face-v1.jpg`
- `references/raya-official-face-v1.jpeg`

If present, the file is used as a **soft identity anchor**.

### Important limitation

Because the current Hugging Face free backend is still mostly **text-to-image**, identity consistency is only approximate.
It is good for:

- stable vibe
- similar facial structure direction
- repeated role-consistent images

It is **not** a guaranteed exact same face every time.

---

## Modes

### Direct mode
Best for:
- close-up selfie
- current mood / status
- casual candid images
- location vibe

### Mirror mode
Best for:
- mirror-area shots
- outfit photos
- half-body / full-body framing
- gym / fashion / campus / indoor scene compositions

Mirror mode in this project **does not require holding a phone by default**.

---

## Project structure

```text
clawra-selfie/
├─ assets/
│  └─ clawra.png
├─ scripts/
│  ├─ clawra-selfie.sh
│  └─ clawra-selfie.ts
└─ SKILL.md
```

### Key files

- `scripts/clawra-selfie.sh`
  - main generation script
  - backend routing
  - anchor prompt assembly
  - output file writing

- `scripts/clawra-selfie.ts`
  - small Node wrapper for script execution

- `SKILL.md`
  - OpenClaw skill-facing usage instructions

---

## Requirements

### Required
- `bash`
- `curl`
- `jq`
- an OpenClaw environment
- a Hugging Face token

### Environment variables

```bash
HF_TOKEN=your_huggingface_token
HF_IMAGE_MODEL=black-forest-labs/FLUX.1-schnell
HF_API_BASE=https://router.huggingface.co/hf-inference/models
```

### Optional Gemini variables

Gemini is currently disabled by default.
Only enable it if you intentionally want to probe Gemini again.

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
/home/Jaben/.openclaw/skills/clawra-selfie/scripts/clawra-selfie.sh \
  "at the gym, post-workout mirror-area fitness snapshot" \
  "telegram" \
  "mirror" \
  "健身房状态 ✨"
```

### Node wrapper

```bash
node /home/Jaben/.openclaw/skills/clawra-selfie/scripts/clawra-selfie.ts \
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
- running on the field after class

---

## Known limitations

- Hugging Face free generation is **not true reference-image editing**
- identity consistency is weaker than paid or local advanced pipelines
- exact same face cannot be guaranteed every run
- free backends can change behavior or rate limits over time

---

## Future upgrade path

This project is intentionally structured so it can later upgrade to:

- stronger paid image APIs
- true reference-image editing backends
- local ComfyUI pipelines
- LoRA-based identity consistency
- curated dataset → training workflow

The current implementation is the **practical free version**, not the final ceiling.

---

## Inspiration

- Original inspiration: [SumeLabs/clawra](https://github.com/SumeLabs/clawra)
- Adaptation goal: make a usable, OpenClaw-native, lower-cost version for everyday image-roleplay workflows

---

## Status

This repository reflects a working in-use version with:

- Hugging Face default generation
- Gemini temporarily disabled
- official-face soft anchor support
- prompt-anchored Raya consistency workflow

If you want the stronger version later, the next step is not more prompt tuning — it is upgrading the backend.
