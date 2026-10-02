# 04 — Recognize short sounds

Prerequisite: [path 01 lab 04](../../01-train-from-scratch/04-autograd-and-the-training-loop/README.md). Read [sound, in path 05](../../05-modalities-and-trends/03-sound/README.md) first so the spectrogram is a defined object.

## Purpose

Train a classifier on short audio clips you record: a handful of spoken words, or a handful of household sounds. The point is the representation (a waveform becomes an image-like spectrogram) and an honest split (the same recording session must not sit in train and test).

## Explain before you code

1. Why is a raw waveform at 16,000 samples per second an awkward input for a small network, and what does a mel spectrogram compress?
2. Why does splitting random one-second slices of one long take leak the room tone and your voice into every split?
3. What is the baseline for five balanced classes?

## Build

Record with the Windows Voice Recorder or any tool that writes `.wav`. Five classes, at least 20 clips each, about one second, same sample rate (16 kHz mono is a good target; resample in code if needed). Classes could be five words you say ("yes", "no", "start", "stop", "go") or five sounds. Put them in `D:\learn-ai-data\audio\<class>\`.

Hold out entire takes for validation and test. If you record 20 separate takes, assign takes to splits, then slice.

1. Load a wav (the stdlib `wave` module is enough), plot one waveform and save it.
2. Compute a mel spectrogram. `torchaudio` transforms are the straightforward tool once torchvision/torch are installed. Print the shape. It should look like `(n_mels, time)`.
3. Baseline: average energy of the clip, plus a threshold you pick on the training set, for a two-class subset if five-way energy cannot separate them. For five-way, a baseline of "always the most common class" still gets recorded, and you can add "nearest class centroid of the mean spectrogram" as a stronger simple baseline.
4. A small convnet on the spectrogram, the same training loop as CIFAR, far fewer epochs. GPU.
5. `python predict.py clip.wav` prints the class.
6. Record 5 new clips after training, in a different room if you can. Those are the test. The interesting failure is a class that only worked because the microphone was in the same place.

## Study this step

**Concepts to master**

- A waveform is pressure samples at a fixed rate. A mel spectrogram is energy across frequency bands across short frames. The convnet sees a 2D tensor, not "sound" as a vague object.
- The split unit is the take or the session. Slices of one take share room tone, microphone, and your voice that day. Random slices leak that signature.
- Class centroids of mean spectrograms are a serious baseline. A convnet that cannot beat them has not used its capacity.
- Domain shift is a new room or a new distance to the microphone. Report it separately from the in-room validation score.

**Study**

- torchaudio tutorial "Audio feature extractions" / mel spectrogram tutorial: https://pytorch.org/audio/stable/tutorials/audio_feature_extractions_tutorial.html — compute one spectrogram and match the shape to the docs.
- *Speech and Language Processing* (Jurafsky and Martin), the short section on mel spectrograms in the speech chapter, if you want the hearing motivation: https://web.stanford.edu/~jurafsky/slp3/ — read only the spectrogram subsection.
- Path 05 lab 03, which is the concept page for this use case. Read it before you record.

**Practice**

- Plot waveform and mel spectrogram of one clip of "yes" and one of background noise. Label the axes.
- Put two slices of the same take on opposite sides of a throwaway split, train for a minute, and watch the flattering accuracy. Then throw that split away.
- Record the five new-room clips last, after the model is frozen.

**Practice questions**

1. 16 kHz for 1 second is how many samples? If you feed that vector straight to a linear layer, how many input weights does the first layer have per unit?
2. Why is a random split of one clap recorded for 10 seconds not 10 independent examples?
3. The in-room validation score is 95% and the other-room score is 40% with five classes. What did the model likely use as a feature?
4. Majority baseline for five balanced classes is what accuracy, and what does a 30% model mean?
5. You normalize each spectrogram by the max of the whole dataset before splitting. Where is the leak?

## You are done when

The convnet beats the centroid or majority baseline on validation, the script classifies a new file, and your notes say whether a new room hurt.
