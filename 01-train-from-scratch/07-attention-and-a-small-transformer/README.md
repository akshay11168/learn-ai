# 07 — Attention and a small transformer

Prerequisite: [06 A tiny language model](../06-tokens-embeddings-and-a-tiny-language-model/README.md).

## Purpose

Implement scaled dot-product attention, first as a few matrix multiplies you can read, then as a small decoder-only transformer trained on the same text as lab 06. The lab 06 model is the baseline. Attention has to beat it.

## Explain before you code

1. If each token is a vector, how can a weighted average of earlier tokens represent "what this position should look at"?
2. Why scale the query-key dot products by `sqrt(d)` before the softmax?
3. What does a causal mask do to the attention weights of future tokens, and why would leaving it off make the next-token task dishonest?

## Concepts

One attention head, for a sequence of vectors `X` shaped `(T, d)`:

```text
Q = X @ Wq
K = X @ Wk
V = X @ Wv
scores = (Q @ K.T) / sqrt(d)
scores = scores + mask          # 0 on and below the diagonal, -inf above
weights = softmax(scores, dim=-1)
out = weights @ V
```

Each row of `weights` sums to 1 and says how much this position reads from each earlier position, including itself. `Wq`, `Wk`, and `Wv` are learned. Different heads use different projections, then their outputs are concatenated and mixed with a linear layer.

A transformer block is: layer norm, attention, residual add, layer norm, a two-layer MLP, residual add. Residuals let the gradient travel backward as an identity path, which is why the network from lab 03 could be stacked many times only after this trick (and normalization) became standard. The MLP inside the block is the same kind of feature mixer you already trained. Attention moves information between positions. The MLP changes it at a position.

A decoder-only language model is a stack of those blocks, plus token embeddings and a final linear map to vocabulary logits. Position matters: add learned position embeddings or a fixed sinusoidal pattern so the model can tell token 3 from token 30.

This is the architecture family behind the code models in path 02. The ones you load there are this, scaled up, trained longer, on more data, with extra training stages (instruction tuning, preference tuning).

## Build

1. `attention_numpy.py`: one head, random `X` of shape `(4, 8)`, causal mask, printed weights. Confirm each row sums to 1 and that positions do not attend to the future (those weights are 0).
2. Numerical or shape check: `out` has the same shape as `X`.
3. A PyTorch `Transformer` you write yourself, not `nn.Transformer`. Suggested size for this GPU and a single book: embedding 64, 2 heads, 2 blocks, MLP hidden 256, context 64. That is well under a million to a few million parameters depending on vocabulary. Train on the lab 06 data and split.
4. Use the same validation cut as lab 06 so the loss comparison is fair. Log both losses in `notes.md`.
5. Sample at the same temperatures as lab 06. Compare a paragraph side by side with the windowed model.
6. Overfit a tiny slice on purpose: 200 characters, a model large enough to memorize them, until training loss is near zero. Sample with temperature 0 and confirm it reproduces the slice. Then sample after training on the full file and write the difference. This is the cleanest picture you will get of capacity versus data.

Watch the loss. If it is `NaN`, the learning rate is too high or the attention scores overflowed. Scaling by `sqrt(d)` and a learning rate around `3e-4` are the usual fixes. Record the failure if you hit it; path 06 lab 02 is about exactly this.

## Verify

Masked attention rows sum to 1 and put 0 weight on future tokens. The transformer's validation loss beats lab 06's model on the same split. The memorization slice reproduces at temperature 0.

## Stretch

Read one attention map for a prompt that contains a repeated rare word, and see whether a later occurrence attends back to the earlier one. On a tiny model this sometimes shows up and sometimes does not. Report the map, not the hope.

## You are done when

You can implement a causal attention head from the four-line definition without looking it up, and you can say what the residual MLP is still doing inside the block.
