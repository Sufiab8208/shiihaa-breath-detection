# Privacy and design constraints

These are constraints we hold ourselves to, not marketing. They shape what the system is allowed to do.

## Audio and privacy

- **No raw audio upload.** Microphone audio is analysed on-device. The raw audio stream does not leave the phone.
- **No speech analysis.** The pipeline operates on the energy envelope and spectral shape of breathing. It is not built to recognise, transcribe, or interpret speech.
- **On-device processing.** Breath detection runs locally. What's collected for model improvement is quality-checked (waveform, phase-label) material kept on-device until a user explicitly confirms it, not a continuous recording.
- **Browser demo without an account.** The browser biofeedback demo runs the live breath detection without creating an account. The native app still uses an account for personal features.

## Hardware

- **No wearable required for basic biofeedback.** The microphone alone drives the live experience.
- **Optional chest strap.** A respiration belt or heart-rate strap can be added; it's used for validation and as an optional extra signal, never a prerequisite.

## Design

- **Feedback, not scoring.** The interface reflects your breathing back to you. It is not a score, a streak, or a game, and it isn't designed to pull you back in.
- **Wellness, not medical.** This is a wellness and lifestyle tool for self-awareness. It is not a medical device and is not validated for clinical use.
