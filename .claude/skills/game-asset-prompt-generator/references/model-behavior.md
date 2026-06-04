# Model Behavior Comparison

Empirical observations from generating identical prompts across different models. This file captures which models respond to which prompt signal types.

## Z-Image Turbo vs Anima (same prompt)

Based on the full VN corridor prompt (Test 9) run on both models.

| Signal type | Z-Image Turbo | Anima |
|-------------|---------------|-------|
| Architectural repetition | Strong — precise, clean | Medium — less precise, more interpretive |
| Material language | Strong — surface qualifiers respected | Medium — materials present but looser |
| Security infrastructure | Strong — hardware renders reliably | Medium — may introduce artifacts |
| Perspective / composition | Strong — clean centered VP, good readability | Medium — more dramatic but less controlled |
| VN usability (clear floor) | Strong — open space respected | Medium — may populate with objects |
| Behavioral lighting | Weak — light pools partially followed, darkness becomes flat wall | Strong — pools, shadows, darkness obeyed aggressively |
| Atmospheric darkness | Weak — flattens to mid-gray | Strong — creates genuine deep shadow |
| Environmental mood | Weak — feels procedural | Strong — dramatic, mysterious |
| Depth / recession | Medium — functional but flat | Strong — eye pulled into depth |
| Architectural precision | Strong — clean geometry, realistic | Medium — more stylized, less realistic |

## Key takeaway

The models have complementary strengths. The optimal strategy may be to emphasize different signal types depending on the target model rather than using one prompt for all.

**Z-Image Turbo excels at:** things the model can reconstruct from photographic training data — buildings, materials, hardware, perspective.

**Anima excels at:** things requiring interpretive rendering — atmosphere, darkness, light behavior, mood.

## Implications for prompt strategy

| If targeting | Emphasize | De-emphasize |
|-------------|-----------|-------------|
| Z-Image Turbo | Architecture, materials, infrastructure, perspective constraints | Abstract mood, atmospheric darkness, lighting behavior |
| Anima | Lighting behavior, atmosphere, depth, shadow structure | Architectural precision, strict perspective, hardware specificity |

## Future entries

When testing a new model, run the same canonical test set prompts and compare against these baselines. Add new rows for each model tested.
