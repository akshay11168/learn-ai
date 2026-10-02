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

## Study this step

**Concepts to master**

- A tokenizer is a function from text to integer ids and back. Character-level ids are a vocabulary you can print. Subword ids are a learned vocabulary you must inspect with `convert_ids_to_tokens`.
- An embedding is a matrix of shape `(vocab, n_embed)`. Looking up id `i` is selecting row `i`. Those rows are trained by gradient descent like any other weight.
- Next-token prediction is multi-class classification at every position. The target at position `t` is the id at position `t+1`. The loss is cross-entropy, averaged over positions you choose to score.
- A fixed window flattened into a linear layer can use order. A mean of the embeddings cannot, because means are commutative.
- Temperature divides logits before softmax. Below 1 sharpens. Above 1 flattens. Greedy decoding is the limit of temperature going to 0, implemented as `argmax`, not as a tiny temperature that can still tie.
- Perplexity `exp(mean loss)` is an effective branching factor. It is comparable only between models with the same tokenizer and the same validation text.

**Study**

- Jay Alammar, "The Illustrated Word2vec": https://jalammar.github.io/illustrated-word2vec/ — read part 1, the embedding lookup and the prediction task. You are doing the character version of that picture.
- Karpathy, "Let's build GPT", the bigram and loss sections only, before attention: https://www.youtube.com/watch?v=kCc8FmEb1nY — code a bigram count model while you watch that segment. Stop the video when he starts the self-attention block. That block is the next lab.
- *Speech and Language Processing* (Jurafsky and Martin), the chapter on n-gram language models, sections on the chain rule and perplexity: https://web.stanford.edu/~jurafsky/slp3/ — read the n-gram chapter's opening. A neural window model is a smoothed, learned n-gram.

**Practice**

- Print every character id in one sentence you know by heart, then decode it back. A mismatch means your `stoi`/`itos` are not inverses.
- Train the bigram counts and the neural window model on the same split. Put both validation losses in one table.
- Sample the same prompt at temperatures 0.2, 1, and 2. Mark repeated phrases that appear verbatim in the training file.

**Practice questions**

1. Vocabulary size 40, embedding size 16, window 32, linear head from the flattened window to 40 logits. How many parameters are in the embedding, and how many are in that linear layer, ignoring bias?
2. The target for the window ending at index `i` is which id, and what do you do at the last character of the file?
3. Why does shuffling individual windows across the whole book leak the validation text?
4. Two models both have validation loss `2.3`, one character-level and one subword. Can you say they are equally good? What else must match?
5. Temperature 0.3 produced a loop of the same five words. What is the model doing, and which control do you change first, temperature or the data?
6. A one-hot vector of length `V` dotted with a matrix `(V, d)` equals one row of that matrix. How is `nn.Embedding` the same operation with less arithmetic?

## You are done when

You can describe training as "classify the next token," and you can change temperature and predict how the sample will change.
