# 01 — Language

Prerequisite to the experiment: [path 01 lab 07](../../01-train-from-scratch/07-attention-and-a-small-transformer/README.md). You can read the map earlier.

## The map

A sentence becomes token ids, then vectors. A stack of causal transformer blocks turns those vectors into a distribution for the next token. Pretraining minimizes that prediction loss on a huge corpus. The model you load in path 02 has already done this.

Instruction tuning continues the same loss on pairs of requests and responses, so the model spends its probability on answers rather than on continuing the user's question. Preference tuning, path 02 lab 06, then pushes probability toward responses a judge preferred.

Subword tokenizers (BPE and its relatives) keep a vocabulary of frequent chunks. You built a character tokenizer by hand so nothing was hidden. In the experiment below you will compare the two on the same sentence.

Context length is the number of tokens the model can see at once. Attention cost grows quickly with that number, which is why your from-scratch model uses a short context and why long-document products are an engineering problem of their own (the retriever in path 04 is the small-scale answer: do not paste the whole corpus into the window).

Code models are language models whose pretraining data is code. They are not a different species. The algorithm capstone adapts one. It does not invent a new architecture.

## Experiment

1. Take a paragraph of your own writing and encode it with your character tokenizer and with the pretrained model's tokenizer. Record token counts and show one word that split into several subwords.
2. From the path 01 transformer and the path 02 base model, save a greedy completion of the same 20-token prompt. Write what scale changed: not the loss formula, the weights and the data.
3. Read one model card of a current small code model. In five sentences, place it on the map: architecture family, parameter count, whether you could run it here, whether you could fine-tune it here, and what the card does not tell you.

## Study this step

**Concepts to master**

- The same next-token loss is used in pretraining and, with different data and often with masked prompt tokens, in instruction tuning.
- Preference data ranks two completions. It is not "more text."
- Subword tokenization is a compression of the character stream into frequent chunks. Token counts, money, and context length are all in that unit.
- A chat model is not a different architecture from the decoder you trained. It is that architecture after more data and more stages.
- Parametric knowledge is whatever survived in the weights. Retrieved knowledge is whatever you put in the prompt. They fail differently.

**Study**

- Jay Alammar, "The Illustrated Transformer" and "The Illustrated GPT-2": https://jalammar.github.io/illustrated-transformer/ and https://jalammar.github.io/illustrated-gpt2/
- Sebastian Raschka, "Understanding and Coding the Self-Attention Mechanism of Large Language Models From Scratch": https://magazine.sebastianraschka.com/p/understanding-and-coding-self-attention — read the single-head section and compare to your NumPy attention.
- The InstructGPT paper, abstract and Figure 2 (the three steps): https://arxiv.org/abs/2203.02155 — this is the picture of pretrain, then instruct, then preference. Map each step onto a lab you did or will do.

**Practice**

- Draw the three-stage pipeline from memory and mark which stages you have run.
- Tokenize one paragraph with your character tokenizer and the pretrained tokenizer. Record the ratio of lengths.
- Write five sentences you could say in a client meeting about what a language model was trained to do, and five you will not say ("it understands", "it looks up the answer").

**Practice questions**

1. Why can instruction tuning not add a fact that appears nowhere in pretraining or in the instruction data, except by memorizing a new pair you happened to include?
2. A 2048-token context and a 3-token-per-word average imply about how many words fit? Why is that a bad way to plan a document system?
3. What does the model see if the answer is not in the weights and not in the retrieved prompt?
4. Name the loss used in pretraining, in one sentence, without the word "learn."
5. Which stage teaches the model to prefer a passing program over a failing one when both are fluent?

## You are done when

You can give a ten-minute explanation of "how a chatbot is trained" that includes pretraining, instruction tuning, and preference data, and you can say which of those three you have actually run.
