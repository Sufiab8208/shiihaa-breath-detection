# Show HN draft

Working title and first comment for posting this. Internal note, not part of the method.

## Title

Show HN: Live breath detection from a phone microphone, on-device

## First comment

I'm a physician, and I kept running into the same thing with breathing apps: they're supposed to calm you down but mostly compete for your attention. I wanted to try the opposite — have the phone listen to how you actually breathe and reflect it back, with no wearable and no score.

The detection turned out to be the whole problem. A phone mic in a normal room is a mess — room tone, traffic, the phone resting on fabric, automatic gain control fighting you. Recovering inhale/exhale/holds out of that, on-device, without analysing speech or uploading audio, is harder than the "mindfulness" framing suggests. Holds and pauses are nearly identical acoustically, which is the part that breaks naive energy detectors.

This repo is the method write-up rather than the app source: how the signal-processing + state-machine + quality-rejection pipeline is put together, where ML fits (downstream, on quality-checked labels), and a research pitch for the validation study we're running against a chest belt and Polar H10. I tried to be honest about the limits — smartphone-only breath detection sits well below wearable systems, and the study exists to find out how far below.

Happy to go into the signal processing, the failure modes, or the study design.
