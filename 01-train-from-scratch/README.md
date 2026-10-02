# 01 — Train from scratch

This path is the core of the course. You will write models whose first predictions are nonsense, measure the nonsense, and change the weights until the predictions improve. At the start you will do the weight change yourself. By the end PyTorch will do it, and you will know what it is doing because you already did it on a small net.

"From scratch" here means random initial weights and a training loop you understand. It does not mean reimplementing CUDA. It also does not mean pretraining a 7B coder on this laptop. The last lab draws that line with arithmetic, after you have trained a real, small model.

## The idea every lab repeats

1. Choose a function with unknown numbers in it (the weights).
2. Run inputs through it to get predictions.
3. Score the predictions against known answers (the loss).
4. Compute how the loss would change if each weight moved a little (the gradient).
5. Move each weight a small step against its gradient.
6. Repeat on the training split. Judge progress on the validation split.

Labs 01–03 do steps 1–5 in NumPy, including a gradient you check by nudging the weight. Lab 04 hands steps 4–5 to autograd and makes you write the loop that real training uses: batches, epochs, validation, a saved checkpoint. Labs 05–08 change the function (convolutions, embeddings, attention) and keep the loop.

## Labs

| Lab | Function you train | What is new |
|---|---|---|
| [01 Fit a line](01-fit-a-line/README.md) | `y = wx + b` | The loop in its smallest form |
| [02 One neuron](02-one-neuron-and-logistic-regression/README.md) | Sigmoid of a dot product | Classification, log loss, a baseline to beat |
| [03 Backprop in NumPy](03-a-network-and-backprop-in-numpy/README.md) | Two-layer network | The chain rule across layers, checked numerically |
| [04 The training loop](04-autograd-and-the-training-loop/README.md) | The same network in PyTorch | Batches, epochs, device, checkpoint |
| [05 Convolutions](05-images-and-convolutions/README.md) | A small convnet on CIFAR-10 | Weight sharing, channels, overfitting a vision model |
| [06 A tiny language model](06-tokens-embeddings-and-a-tiny-language-model/README.md) | Next-character prediction | Tokens, embeddings, a model that samples text |
| [07 A small transformer](07-attention-and-a-small-transformer/README.md) | The same data with attention | Queries, keys, values, and why the MLP lab still matters |
| [08 Capstone](08-capstone-pretrain-a-tiny-model/README.md) | A tiny model on one programming language | What pretraining is, and what hardware the next size up needs |

## How deep to go

- Do not import `sklearn.LinearRegression` or `nn.Linear` until you have a version that learns without it. Then import it and check that the two agree.
- When a loss curve falls, write one sentence on which split it fell on. A falling training loss is the optimizer working. A falling validation loss is the model becoming useful.
- Keep the numerical gradient check from lab 03. You will be tempted to skip it. It is the lab that makes backpropagation a calculation.
- Read [05 Modalities, language](../05-modalities-and-trends/01-language/README.md) while you are in labs 06 and 07. The history is short and it explains why this architecture is the one the field standardized on.

## Done with this path when

- You can derive the gradient of your two-layer net for one input and match PyTorch's autograd within a small tolerance.
- A convnet you trained beats a linear baseline on CIFAR, and you have an error analysis of one class it still misses.
- A tiny transformer samples text or code in the style of its training file, and you can say what it has memorized.
- You can explain, with bytes of memory, why this GPU trains the capstone and why it does not train a 7B model from random weights.

Path 02 starts from the checkpoint habit in lab 04. You may open [02 lab 01](../02-train-from-a-base-model/01-load-a-model-and-run-it/README.md) after lab 04 if you want to see a large model run, then come back and finish 05–08 before you adapt one.
