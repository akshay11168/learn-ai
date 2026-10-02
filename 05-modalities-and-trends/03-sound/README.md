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

## Study this step

**Concepts to master**

- Sample rate times duration is the number of waveform samples. A spectrogram trades that long axis for a frequency axis times a shorter time axis.
- Mel bins are spaced for perception, not for a linear Hertz scale. You use them because speech and everyday sound concentrate information that way, and because the tensor becomes image-like.
- Whisper-style models map audio features to text tokens. They are sequence models with a large paired corpus. Your classifier maps a spectrogram to a class id. Do not confuse the two tasks.
- Leakage in audio is shared room, shared take, and shared background. The split rule is the concept that makes the score real.

**Study**

- torchaudio feature extraction tutorial: https://pytorch.org/audio/stable/tutorials/audio_feature_extractions_tutorial.html
- The Whisper paper, abstract and the approach figure: https://arxiv.org/abs/2212.04356 — extract the input representation and the output tokens. Note the model sizes and which ones you would attempt on 6 GB (tiny, small) and which you would not (large).
- Path 04 lab 04's notes once they exist. This lab is the write-up of the representation. That lab is the model.

**Practice**

- From one wav, write down sample rate, number of samples, spectrogram shape, and the convnet's first-layer input shape.
- Run Whisper tiny on five sentences if you do the optional inference. Mark insertions and substitutions. That error list is you seeing a speech model as a sequence model, not as a magic ear.
- Sketch the leak: one 10-second take sliced into 10 overlapping windows, randomly split.

**Practice questions**

1. 16,000 samples per second for 2 seconds is how many numbers in the waveform?
2. Why can the same convnet code classify spectrograms and CIFAR images?
3. A random split of windows from one recording scores 99%. What feature other than the spoken word could explain it?
4. Whisper large does not fit your plan. Which resource is the limit?
5. Your classifier outputs a class. Whisper outputs text. What eval changes because the output changed?

## You are done when

You can explain the path from wav file to class id, including the spectrogram shape, and you know the split rule that makes the score believable.
