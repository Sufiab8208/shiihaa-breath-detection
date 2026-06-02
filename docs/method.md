# Method note

How shii·haa detects breathing from a phone microphone. This is a description of the approach, not a line-by-line spec, and it deliberately separates what the live pipeline does from what is part of the ongoing validation work.

## The signal we start from

The input is the raw microphone stream from a phone. We slice it into short, overlapping analysis windows. For each window we compute:

- an **amplitude / energy** measure (how loud the window is, after accounting for the running noise floor), and
- a few **spectral features** — notably where the spectral energy is concentrated and where its peaks are.

The physical intuition: inhalation through the nose or mouth is more turbulent and tends to push energy higher in the spectrum; exhalation is smoother and lower. This is a tendency, not a rule, and on any single window the features overlap heavily with noise. The signal only becomes interpretable across a sequence of windows.

## Phases, not just loudness

We care about more than "is there breath sound now." The target phases are **inhale**, **exhale**, and the **transitions** between them — including the quiet stretches that a loudness-only system would lump together. A hold and a pause can be acoustically near-identical (both are roughly silent), which is one of the central difficulties: simple energy detection cannot tell a deliberate breath-hold from the gap between breaths.

## The breathing state machine

Phase is tracked by a small **state machine** rather than decided independently per window. The machine holds a current phase and only allows plausible transitions out of it. Its decision thresholds are **adaptive and amplitude-based**: they track the running noise floor and the recent breath dynamic range, so the same person in a quiet room and in a noisy one is handled by the same logic with shifted thresholds, instead of by hard-coded constants.

This is what gives the system temporal coherence. A momentary spike inside an exhale doesn't get reclassified as an inhale just because one window crossed a threshold; the transition has to be consistent with where the machine already is.

## Spectral classification

Where amplitude alone is ambiguous, the **spectral features** are used to discriminate phase — combining more than one signal (for example a peak-frequency cue together with the spectral centroid) rather than relying on a single number. Using two cooperating spectral measures is more robust to the broadband noise that a single feature gets fooled by.

## The quality layer

Not every window is allowed to influence the output. A **data-quality layer** sits in front of the decision and rejects windows that are too noisy, too quiet, or acoustically ambiguous. The design preference is explicit: it is better to emit "uncertain" briefly than to emit a confident, wrong phase. In practice this means the system will occasionally go quiet rather than fabricate a transition — which, for biofeedback, is the right failure mode, because a wrong phase is something the user can immediately feel.

Part of this layer is **transient-artifact rejection** — a set of checks that catch short, sharp events (a tap, a door, a consonant burst) that look like breath onsets to an energy detector but don't have the temporal shape of one.

## Machine learning, scoped

ML is used to **sharpen feedback and improve the model over time** from windows that have passed the quality layer and been confirmed. It is not the thing the live detection hangs on: the rule-based pipeline above is what runs the experience. Keeping ML downstream of a validated, quality-checked label source is a deliberate choice — see the research pitch for why label quality is treated as a precondition rather than an afterthought.

## Platform quirks

A large share of the engineering is in handling real mobile audio:

- **Device and placement variation** — phone in hand vs. flat on a table vs. on fabric changes the spectrum substantially.
- **Automatic gain control** on the OS audio path can compress exactly the dynamics we rely on.
- **Microphone hardware differences** across phone models shift the spectral baseline.
- **Ambient drift** — fans, traffic, HVAC — requires the noise floor to be re-estimated continuously rather than calibrated once.

## What is current vs. under validation

The pieces above — windowed signal processing, the adaptive amplitude-based state machine, multi-signal spectral classification, the quality layer with transient-artifact rejection, and bounded downstream ML — describe the live approach.

Beyond that, the validation study is evaluating additional protocol-level ideas, including:

- a **Protocol-Synchronized Prep Anchor** (a short scripted exhale prompt before free breathing, used to measure a per-session spectral signature instead of relying on population training), and
- **Technique-Aware Ratio Matching** (using the known target ratio of a breathing technique to resolve the hold/pause ambiguity).

These are described in the research pitch as part of the method under validation. We are not claiming validated accuracy numbers here; the study exists precisely to produce them.

## Limitations

- Microphone-only breath detection in uncontrolled conditions is hard, and smartphone-only systems in the literature sit well below wearable-based ones.
- Silence is inherently ambiguous: distinguishing a hold from a between-breath pause from quiet shallow breathing is the hardest case, and the protocol-level ideas above exist specifically to attack it.
- Heavy background noise, very shallow breathing, and aggressive device-side audio processing all degrade detection.
- This is a wellness and self-awareness tool. It is not a medical device and is not validated for any clinical use.
