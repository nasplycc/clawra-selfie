# Clawra Selfie

> Clawra-inspired selfie generation for OpenClaw, powered by a practical **Hugging Face free-tier fallback**.

![Clawra](assets/clawra.png)

A lightweight OpenClaw image skill inspired by [SumeLabs/clawra](https://github.com/SumeLabs/clawra), adapted for a more practical everyday setup:

- keep the **Clawra / Raya-style image roleplay experience**
- avoid making paid image backends the default requirement
- use a **working free Hugging Face route** as the active backend
- keep a clean upgrade path for **reference-image editing**, **paid APIs**, and **LoRA-based identity consistency**

## Overview

This project exists because the original Clawra idea is genuinely good, but some of the most powerful model routes are paid.

So this version takes a different path:

- build a **usable OpenClaw-native workflow first**
- keep the experience good enough for day-to-day use
- preserve future compatibility with stronger backends later

In short:

> **Clawra vibe, OpenClaw-native workflow, free-first backend strategy.**

---

## Features

- **OpenClaw-friendly skill structure**
- **Prompt-driven selfie / daily-life image generation**
- **Direct mode** for close-up selfie / current-state shots
- **Mirror mode** for half-body / full-body / gym / outfit scenes
- **Soft official-face mechanism** via workspace reference image
- **Fixed face anchor + negative anchor** for better consistency
- **Channel-send compatible workflow** for OpenClaw messaging
- **Gemini probe path implemented but disabled by default**
- **Hugging Face remains the stable active default**

---

## Backend Strategy

### Active backend
- **Hugging Face**
- Default model:
  - `black-forest-labs/FLUX.1-schnell`

### Disabled by default
- **Gemini image probe**

Gemini support exists in the script, but is currently disabled by default because testing showed free-tier image generation was not reliably usable in this setup.

To re-enable it later:

```bash
ENABLE_GEMINI=1
GEMINI_API_KEY=your_google_gemini_api_key
```

---

## Identity Consistency Strategy

This project currently uses **soft identity consistency**, not hard face locking.

It combines:

- a fixed **FACE_ANCHOR**
- a fixed **NEGATIVE_ANCHOR**
- an **official face reference file path**
- scenario-specific prompt shaping

### Official face files

The script looks for these files inside the workspace:

- `references/raya-official-face-current.png`
- `references/raya-official-face-current.jpg`
- `references/raya-official-face-current.jpeg`
- `references/raya-official-face-v1.png`
- `references/raya-official-face-v1.jpg`
- `references/raya-official-face-v1.jpeg`

If one exists, it is treated as the current official face anchor.

### Limitation

Because the current free Hugging Face path is still mostly **text-to-image**, this project can only provide:

- stable visual vibe
- approximate facial-structure consistency
- repeatable character direction

It **cannot guarantee identical face output every time**.

---

## Modes

### Direct mode
Best for:
- close-up selfie
- current mood / state
- casual candid shots
- daily-life moments

### Mirror mode
Best for:
- mirror-area photos
- outfit shots
- half-body / full-body framing
- gym / campus / indoor compositions

Mirror mode here **does not require holding a phone by default**.

---

## Project Structure

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
  - main backend logic
  - prompt assembly
  - fallback flow
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

## Example Scenarios

- morning selfie
- breakfast shop full-body shot
- campus classroom candid
- post-workout gym state
- city street snapshot
- coffee shop side profile
- running on the field after class

See also: [`examples/README.md`](examples/README.md)

---

## Known Limitations

- Hugging Face free generation is **not true reference-image editing**
- identity consistency is weaker than paid or local advanced pipelines
- exact same face cannot be guaranteed every run
- free backends may change behavior, limits, or availability over time

---

## Future Upgrade Path

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
- Adaptation goal: make a usable, OpenClaw-native, lower-cost version for daily image-roleplay workflows

---

## Status

This repository currently reflects a working private-repo version with:

- Hugging Face default generation
- Gemini temporarily disabled by default
- official-face soft anchor support
- prompt-anchored Raya consistency workflow

If stronger identity consistency is needed later, the real upgrade is not more prompt decoration — it is a better backend.
