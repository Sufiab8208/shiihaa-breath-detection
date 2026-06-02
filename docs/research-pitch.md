# Research pitch

Felix Zeller · April 2026 · ~2 minute read

## The one-sentence version

We validate a novel smartphone-based breathing-phase detector against clinical ground truth — and use the same study to establish it as a qualified label source for a follow-up ML project.

## Why this is scientifically novel

Existing microphone-based breath-detection systems share two hard limits:

1. **Only three classes** — inhale / exhale / silence. A breath-hold and a between-breath pause are acoustically near-identical, so they aren't distinguished.
2. **Either wearable-dependent or weak** — in-ear microphones reach F1 > 95%, but smartphone-only systems drop to around 69% balanced accuracy in real-world conditions.

The detector combines five cooperating pillars, two of which are, to our knowledge, new in the literature:

- **Protocol-Synchronized Prep Anchor** — a ~3 s scripted exhale prompt before free breathing, which measures the user's individual spectral signature. This per-session calibration replaces population training and makes label polarity deterministic.
- **Technique-Aware Ratio Matching** — the known target ratio of a technique (e.g. 1:1.75:2 for 4-7-8) resolves the hold/pause ambiguity deterministically.

Alongside these: an adaptive amplitude-based state machine, multi-signal spectral classification (peak + centroid), a Valley Gate (3-AND), and a six-rule transient-artifact rejection.

## The two-phase story (the actual point)

**Phase 1 — detector validation (this paper).** A prospective single-arm study, N ≈ 30, in Switzerland (ethics application in preparation, expected HRA ClinO category A). Ground truth: Polar H10 + Vernier Go Direct Respiration Belt at 50 Hz, with dual-rater segmentation (κ ≥ 0.80). Three nested settings (quiet / normal / challenging, the last with scripted acoustic challenges). Primary endpoint: balanced accuracy, non-inferiority against Breeze 2 (69%, state-of-the-art smartphone-only).

**Phase 2 — substrate qualification (the novel part).** The same study also yields a label-quality measure: Cohen's κ between detector labels and the chest-belt reference, per technique × setting cell. Cells whose lower-bound 95% CI(κ) ≥ 0.85 are declared "ML-ready" and released as the training basis for a follow-up ML paper. Cells that miss the cutoff are explicitly excluded.

Why this is the clou: mobile-health ML usually fails on "unvalidated labels in, unvalidated model out." We invert the order — quantify label quality first, then train. To our knowledge no one has formalised this in the mobile-audio context.

## Two papers from one study

| Paper | Focus | Data | Horizon |
| --- | --- | --- | --- |
| Paper 1 (now) | Validated rule-based detector + substrate qualification | N ≈ 30 study | 6–9 months |
| Paper 2 (follow-up) | Personalised self-supervised ML on qualified labels | The dataset released from Paper 1 + ongoing app users | 12–18 months |

Paper 1 automatically produces the de-identified qualified-labels dataset (YAMNet embedding, detector label, chest-belt label), published as an OSF benchmark — not only for us, but as a resource for the community.

## Why this is ready now

The algorithm has been running in production since April 2026 (shii·haa v1.7.4, iOS + Android). The label-collection infrastructure is already live (milestone M1): confirmed (waveform, phase-label) pairs are collected per user on-device, and no raw audio leaves the device. Once the validation study runs, it delivers both the accuracy evidence for Paper 1 and the qualified training set for Paper 2 — with no additional recruitment.

## Contact & collaboration

We're looking for clinical co-authors, ethics-committee sparring, and feedback from methods peers (sample-size strategy, target-journal fit, registration strategy). The full methods draft (v2.1) exists as a supplement and is shared on request.

Felix Zeller · felix@shiihaa.app · shiihaa.app
Intensive care physician, internist & emergency physician — now in private practice as a general practitioner. Founder of shii·haa.
