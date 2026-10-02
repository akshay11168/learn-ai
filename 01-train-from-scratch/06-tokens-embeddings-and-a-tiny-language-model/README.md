# 06 — Tokens, embeddings, and a tiny language model

Prerequisite: [04 The training loop](../04-autograd-and-the-training-loop/README.md).

## Purpose

Language models in this course predict the next token. To do that, text has to become integers, integers have to become vectors, and a loss has to score a distribution over the vocabulary. You will train a tiny model that does this on a small text file and sample from it.

## Explain before you code

1. Why is "the next character" a classification problem with as many classes as there are characters?
2. A one-hot vector of length 30 for the letter "q" is almost all zeros. An embedding of size 16 is a learned lookup. What does the row for "q" come to represent?
3. If the model is trained on one short story, what will a sample reveal besides English spelling?

## Concepts

**Tokenization.** Character-level: each character is a token, vocabulary is small (under 100), sequences are long, and the model can learn spelling from nothing. Word-level: vocabulary is huge and unknown words break. Subwords (BPE) are the compromise production models use. You will implement character-level yourself, then run a pretrained tokenizer in path 02 and see subwords.

**Embedding.** `nn.Embedding(vocab, n)` is a matrix you index by token id. Row 12 is the vector for token 12. Those rows are weights. Training moves them. Tokens that behave alike get nearby rows, though "nearby" on a tiny corpus is a weak effect. Do not expect word2vec-quality analogies from a short file.

**The task.** Given characters `c0, c1, ..., c_{t-1}`, predict a distribution for `c_t`. Cross-entropy against the true next character. Averaged over positions and batches, that is the training loss. Perplexity is `exp(mean cross-entropy)` and reads as "effective number of choices the model is unsure among." You can report it once you have the loss.

**A first architecture, before attention.** Embed each character, mean-pool the last `block_size` embeddings (or flatten and use a linear layer), and project to vocabulary logits. This model sees only a fixed window and has no notion of order if you only average. Prefer flattening the window so order is visible to the linear layer: input shape `(batch, block_size * n_embed)`. It is a bigram-to-n-gram model, and it is enough to watch loss fall and samples become less random. Lab 07 replaces this trunk with attention. Building the weak model first gives you a baseline.

**Sampling.** Take the logits for the last position, divide by a temperature, softmax, and draw one token. Append it and repeat. Temperature near 0 becomes almost deterministic. Temperature above 1 becomes chaotic. Show both.

## Build

Training file: a plain text you care about, at least a few hundred KB if you can (a public-domain book from Project Gutenberg is a good size). Save it under `D:\learn-ai-data\text\`, not in git if the file is large. A few dozen KB still trains; the samples will be more repetitive.

1. Build `stoi` and `itos` maps from the characters in the file. Encode the whole file to a `LongTensor`.
2. Split by character position: first 90% train, last 10% validation. Splitting by shuffling windows leaks the story across the split. A cut in time is cleaner.
3. Implement the windowed embedding model above. `block_size` 32 or 64, embedding size 32, Adam `1e-3`, batch size 64. Train on GPU until validation loss clearly drops, then keep an eye on it rising again.
4. Sample 500 characters at temperatures 0.5, 1.0, and 1.2. Save them in `notes.md`.
5. Baseline: a bigram model you can implement as counts, `P(next | current)`. Compare its validation loss to the neural model's. The neural model should win because it sees a wider window.

## Verify

Validation loss beats the bigram baseline. Samples at low temperature look locally like the training text. You can point at a repeated phrase and call it memorization.

## Stretch

Plot a few embedding rows' pairwise dot products for vowels versus consonants. On a small file the pattern may be faint. Report what you actually see.

## You are done when

You can describe training as "classify the next token," and you can change temperature and predict how the sample will change.
