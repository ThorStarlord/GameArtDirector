# Z-Image Turbo Technical Notes

Model-specific behavior that affects prompt strategy for background generation.

## Key facts

- **Architecture:** S3-DiT (6B single-stream diffusion transformer) — text and image tokens processed together
- **Distillation:** Few-step Turbo model (~8 effective diffusion steps)
- **Training:** Bilingual (English + Chinese), strong instruction-following
- **URLs:** [Hugging Face](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo) | [GitHub](https://github.com/Tongyi-MAI/Z-Image)

## Critical: no negative prompts

The official pipeline **does not use classifier-free guidance at inference**:
- `guidance_scale` = 0.0 (best quality)
- `negative_prompt` is **ignored** by the base model
- Third-party UIs may add custom CFG hacks, but assume the negative prompt box does nothing

**Implication:** All constraints must go in the positive prompt. Use in-prompt exclusion phrasing ("no text, no UI, no logos, no furniture, no people, no workstations").

## Parameters for our use case

| Parameter | Value | Notes |
|-----------|-------|-------|
| Steps | 8-12 | Turbo designed for few steps; higher rarely helps |
| Resolution | 1024×1024 (native) | Can draft at 768 or 512 |
| Guidance scale | 0.0 | Official default; ignore CFG sliders |
| Seed | Fixed during iteration | Randomize for variety |

## Prompt structure that works

Long, detailed, structured prompts outperform short ones. Recommended scaffold for backgrounds:

> **[Environment type] + [Architectural structure] + [Materials] + [Infrastructure/hardware] + [Lighting] + [Composition/view] + [Constraints/exclusions]**

Sweet spot: **80-250 words**. Longer is fine if precise. Avoid poetic/novelistic prose — the model follows instruction better than suggestion.

Token limit: 512 default, can raise to 1024 in official pipeline.

## What the model responds to (from our 8-test experiment)

| Signal type | Reliability | Style |
|-------------|-------------|-------|
| Visible infrastructure (cameras, turnstiles, signs) | Strongest | Renders immediately |
| Physical structures (doors, readers, panels) | Strong | Repetition amplifies identity |
| Specific materials (matte charcoal, brushed metal, slate-blue) | Strong | Surface qualifiers matter |
| Identity constraints (what the space IS NOT) | Moderate | Suppresses drift |
| Architectural lighting | Weak | Often interpreted as office |
| Abstract narrative purpose | Weak | Mostly ignored |

## Key insight from experiments

> The model responds to things it can draw, not things it has to infer.

Translate every abstract concept into visible architectural consequences before writing it in the prompt. See `principles.md` (P1) and `experiment-log.md` for the full evidence.

## References

- Official prompting guide: [Hugging Face discussion](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo/discussions/8)
- Best practices article: [zimageturbo.org](https://zimageturbo.org/z-image-best-practice)
- Full prompt guide with safety templates (portrait-focused): linked in parent skill docs
