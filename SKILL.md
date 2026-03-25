---
name: clawra-selfie
description: Generate Clawra-style selfie images with a Hugging Face image model and send them to messaging channels via OpenClaw.
allowed-tools: Bash(npm:*) Bash(npx:*) Bash(openclaw:*) Bash(curl:*) Read Write WebFetch
---

# Clawra Selfie

Generate Clawra-style selfies with a **Hugging Face image model** and optionally probe a **Gemini Flash image model** first (disabled by default), then send the result to OpenClaw messaging channels (Telegram, Discord, WhatsApp, Slack, etc.).

Default current persona target:
- Raya is an 18-year-old Chinese young woman
- use a fixed face-anchor prompt for consistency
- use a fixed negative-anchor prompt to reduce drift toward heavy glam, over-mature, over-filtered, overly childish, or structurally off-model looks

## When to Use

- User says "send a pic", "send me a pic", "send a photo", "send a selfie"
- User says "send a pic of you...", "send a selfie of you..."
- User asks "what are you doing?", "how are you doing?", "where are you?"
- User describes a context: "send a pic wearing...", "send a pic at..."
- User wants Clawra/Raya to appear in a specific outfit, location, or situation

## Required Environment Variables

```bash
HF_TOKEN=your_huggingface_token                     # optional if GEMINI_API_KEY is set
ENABLE_GEMINI=1                                     # set to 1 to enable Gemini probe (default disabled)
GEMINI_API_KEY=your_google_gemini_api_key          # optional probe/fallback path
GEMINI_IMAGE_MODEL=gemini-2.5-flash-image          # optional override
HF_IMAGE_MODEL=black-forest-labs/FLUX.1-schnell    # optional override
HF_API_BASE=https://router.huggingface.co/hf-inference/models
```

Token source:

```text
https://huggingface.co/settings/tokens
```

## Important Limitation

This Hugging Face version is a **free-tier fallback**. Compared with fal.ai or other image-edit backends:

- easier to try for free
- but identity consistency is weaker
- and many free HF models are **text-to-image**, not reliable reference-image editing

So this version should be treated as:

- **good for role-consistent selfie vibes**
- **not guaranteed for exact same face every time**

## Official Face Mechanism

The workspace may store an official face reference under:

- `/home/Jaben/.openclaw/workspace-clawra-bot/references/raya-official-face-current.png`
- `/home/Jaben/.openclaw/workspace-clawra-bot/references/raya-official-face-current.jpg`
- `/home/Jaben/.openclaw/workspace-clawra-bot/references/raya-official-face-v1.png`
- `/home/Jaben/.openclaw/workspace-clawra-bot/references/raya-official-face-v1.jpg`

Behavior:

- if a file exists, treat it as Raya's official face anchor
- current HF free backend still uses prompt-first generation, so this is a **soft anchor**, not hard face lock
- when the backend is upgraded later to reference-image editing or local ComfyUI, reuse the same file path


### Mirror mode
Best for outfit / mirror-area / half-body or full-body style.
Does **not** require holding a phone by default.

### Direct mode
Best for close-up selfie / current state / location vibe.

## Primary Script

```bash
HF_TOKEN=your_token \
/home/Jaben/.openclaw/skills/clawra-selfie/scripts/clawra-selfie.sh \
  "her desk late at night, still replying to messages" \
  "telegram" \
  "direct" \
  "Raya 的自拍 ✨"
```

Arguments:

1. `user_context` — what she should be doing / wearing / where she is
2. `channel` — target channel/provider name for OpenClaw send
3. `mode` — optional: `mirror`, `direct`, or `auto`
4. `caption` — optional text caption

## Notes

- Default model is `black-forest-labs/FLUX.1-schnell` because it is relatively easy to test on HF
- If you find a better free HF model, set `HF_IMAGE_MODEL` instead of rewriting the whole skill
- If HF returns JSON/text instead of image bytes, surface the raw error clearly
- This version is intentionally simpler and more robust than the Gemini/fal attempts
