# 03 — Sound

Prerequisite to the experiment: [path 04 lab 04](../../04-small-use-cases/04-recognize-short-sounds/README.md). Read this before you record the dataset, so the takes are split correctly.

## The map

A microphone records air pressure over time, a one-dimensional sequence at thousands of samples per second. A second of 16 kHz audio is 16,000 numbers. A small network can consume that, and it will spend capacity learning the windowing a spectrogram would have given it for free.

A mel spectrogram measures energy in frequency bands that are spaced more like human hearing than like linear Hertz, over short time frames. The result is a 2D array. Your convnet from the image lab applies with almost no conceptual change. That reuse is the point of the map.

Speech-to-text models in the Whisper family are sequence models: audio features in, text tokens out, trained on a large paired corpus. Fine-tuning Whisper tiny or small on a narrow domain (a short command set, or your own accent on a fixed phrase list) can fit on this GPU if the batch and the audio length stay small. Whisper large does not belong in the plan. Fine-tuning speech models is easy to fool with a leaked split: the same sentence recorded twice, one copy in each split, measures whether you memorized the take.

Music and general audio generation exist and move quickly. Treat them as field-note material in lab 04 unless a small open model documents a fine-tune that fits in 6 GB. Do not block the course on them.

## Experiment

The short-sound classifier is the required experiment. In this folder, write the representation notes that the use-case folder does not need to carry:

1. One waveform plot and one spectrogram plot of the same clip, with axes labeled in time and in frequency bins.
2. The shape of the spectrogram tensor and how it entered the convnet.
3. The new-room or new-take test result copied from the use case, plus a sentence on what leaked if you had split slices of one recording at random.
4. Optional: run Whisper tiny on five clips of you speaking a sentence, and note errors. This is inference, not training. It places the speech-to-text family on your map with a personal example.

## You are done when

You can explain the path from wav file to class id, including the spectrogram shape, and you know the split rule that makes the score believable.
