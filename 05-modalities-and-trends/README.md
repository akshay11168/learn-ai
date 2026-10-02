# 05 — Modalities and trends

Text, images, and sound look like different subjects. In this course they are one training loop pointed at different tensors. This path is where you study the representation and the current standard architecture for each, and where you build the habit of updating that map after the course is over.

Do the reading when you reach the matching build lab. Do the experiment in each modality lab once the prerequisite model runs.

| Lab | Pair it with |
|---|---|
| [01 Language](01-language/README.md) | Path 01 labs 06–07, path 02 |
| [02 Images](02-images/README.md) | Path 01 lab 05, path 02 lab 02 |
| [03 Sound](03-sound/README.md) | Path 04 lab 04 |
| [04 How to follow the field](04-how-to-follow-the-field/README.md) | Ongoing, one hour at the end of each month |

## What "latest" means in a study repo

A list of model names goes stale. The lab on following the field teaches you to read a model card, a dataset card, and one paper section, and to write five sentences that would still be useful if the model name changed. The modality labs name the standing ideas as of 2026 so you have a starting map:

- Language: decoder-only transformers, subword tokenization, next-token pretraining, then instruction tuning and preference tuning.
- Images: convnets still as feature bodies you fine-tune; attention-based vision models as the other backbone family; diffusion and related generative models for synthesis, with LoRA as the adaptation method that fits a 6 GB GPU (Stable Diffusion 1.5, not a large SDXL full fine-tune).
- Sound: waveforms to spectrograms for small classifiers; sequence models in the Whisper family for speech-to-text, adapted carefully because a full Whisper-large fine-tune is tighter than this GPU likes. Whisper tiny or small is the local size.

When a newer family replaces one of those, lab 04 is how you notice and how you decide whether the course map needs a paragraph, not a new religion.

## Done with this path when

You have one experiment written up in each modality lab, and one "field note" in lab 04 that compares a new model card to the map above.
