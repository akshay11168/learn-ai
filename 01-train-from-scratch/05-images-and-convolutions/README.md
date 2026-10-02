# 05 — Images and convolutions

Prerequisite: [04 The training loop](../04-autograd-and-the-training-loop/README.md) and [06 Evaluation, metrics](../../06-evaluation-and-depth/01-metrics-baselines-and-error-analysis/README.md). Start the metrics lab alongside this one.

## Purpose

Images are tensors shaped `(batch, channels, height, width)`. A convolution reuses one small filter across the whole image, because a useful visual pattern (an edge, a corner) can appear anywhere. You will train a small convnet on CIFAR-10 and compare it to a linear model on the same pixels.

## Explain before you code

1. CIFAR-10 images are 32×32×3. How many inputs does a linear layer see if you flatten one image?
2. A 3×3 convolution with 8 filters on a 3-channel image has how many weights? Compare that count to the linear layer's first matrix.
3. Why does flipping an image left-right (sometimes) make a legal extra training example for "horse" and an illegal one for "letter b"?

## Concepts

A filter is a small `W` that you slide. At each location the filter's weights dot the local patch. Weight sharing means the edge detector learned in the top-left is the same detector in the bottom-right. Channels in the output are different filters. Stacking conv, ReLU, and pool builds larger patterns out of smaller ones: edges, then corners, then parts.

Padding keeps the spatial size. Stride 2 downsamples. Max-pool of 2×2 keeps the strongest response in each neighborhood and halves the map. After a few of these, flatten and use a linear layer to produce 10 logits. The loss is cross-entropy, the multi-class loss from the neuron lab's stretch.

CIFAR-10 has 50,000 training images and 10,000 test images, 10 classes, 32×32 color. It is the right size for this GPU: a small convnet trains in minutes.

Augmentation (random crop, horizontal flip) invents nearby inputs so the model cannot memorize exact pixels. It is a regularizer. Use it after you have a baseline without it, so you can see what it changed.

## Build

Download CIFAR-10 with `torchvision.datasets.CIFAR10` into `D:\learn-ai-data\cifar10`, not into the repo.

1. Baseline: flatten each image to 3072 numbers, train a linear classifier (`nn.Linear(3072, 10)`) for a few epochs. Record validation accuracy. This is the number to beat. Also record the most-common-class baseline, which is 10% because the set is balanced.
2. A convnet you specify in `notes.md` before coding. A sane start: conv 3→16, ReLU, pool, conv 16→32, ReLU, pool, flatten, linear to 10. Train with Adam, `1e-3`, batch size 128, about 10 epochs. Use the GPU.
3. Plot train and validation loss. If the gap widens, the model is memorizing. Add flip augmentation and train again. Record both validation accuracies.
4. Error analysis: from the validation or test set, save a grid of 16 images the model got wrong, with true label and predicted label in the title. Group them. Write which confusions dominate (cats and dogs are the classic pair).
5. Touch the official test set only after the architecture and augmentation choices are frozen. One number. Write it in `notes.md` and stop tuning.

Expect the linear model somewhere near 30–40% and a small convnet well above that, often in the 60% range with this tiny architecture. The exact number is less important than the gap over the linear baseline and the error grid.

## Verify

The convnet beats the linear baseline on validation. You can compute the parameter count of the first conv layer by hand and match `sum(p.numel() for p in model.parameters())` for that layer. The wrong-image grid exists and your notes name a pattern in the mistakes.

## Stretch

Replace the convnet with the same depth of linear layers on flattened pixels, similar parameter count, and compare validation accuracy. The convnet should win because of weight sharing and locality. Write that sentence only after you see the numbers.

## Study this step

**Concepts to master**

- An image batch is `(N, C, H, W)`. A 3×3 convolution with `C_in` input channels and `C_out` filters has `C_out * C_in * 3 * 3` weights, plus `C_out` biases if bias is on. Those weights are reused at every spatial position.
- Padding, stride, and kernel size determine the output map size. For stride 1 and kernel 3, padding 1 keeps height and width.
- Max-pooling keeps the strongest activation in a window and downsamples. It does not add parameters.
- Stacking conv + ReLU + pool builds a hierarchy: local contrast, then patterns of those, then a linear head that sees a small grid.
- Translation equivariance is the inductive bias you are buying. A fully connected layer on pixels does not have it, which is why the parameter count explodes and the sample efficiency drops.
- Data augmentation invents inputs you claim are the same class. The claim is false for some labels (flipping a digit 6, flipping text).

**Study**

- CS231n convolutional notes, through "Pooling": https://cs231n.github.io/convolutional-networks/ — compute their parameter-count examples by hand before reading the answers.
- Chris Olah, "Conv Nets: A Modular Perspective": https://colah.github.io/posts/2014-07-Conv-Nets-Modular/ — the picture of a conv layer as a bank of dot products.
- *Dive into Deep Learning*, chapter on convolutional neural networks, the sections "Convolutions for Images" and "Padding and Stride": https://d2l.ai/chapter_convolutional-neural-networks/conv-layer.html and https://d2l.ai/chapter_convolutional-neural-networks/padding-and-strides.html — run their shape calculations on your layer.

**Practice**

- For your exact architecture, compute each layer's output shape on a 32×32 input and the parameter count. Then print `p.numel()` and reconcile.
- Train the linear baseline and the convnet for the same wall-clock budget, not the same epoch count, if one step is much heavier. Say which comparison you used.
- Visualize one first-layer filter as a 3×3×3 tensor rescaled to an image. It will not look like a zebra. It should have some spatial structure. If it is noise, training did not move that layer.

**Practice questions**

1. Input `(8, 3, 32, 32)`, conv `kernel_size=3`, `padding=1`, `stride=1`, 16 filters. Output shape, and number of weights excluding bias?
2. Same layer with `stride=2` and `padding=1`. What happens to height? Use the formula `floor((H + 2p - k) / s) + 1`.
3. Why does tying the same 3×3 filter across the image use less data than a separate detector for each pixel?
4. You flip every CIFAR image left-right, including the validation set, as augmentation applied after the split, using a random flip at eval time too. What did you make incomparable?
5. The confusion matrix shows cats predicted as dogs more than any other pair. Name two data-side reasons and one model-side reason. Which would you test first?
6. A convnet and an MLP have the same parameter count. The convnet wins on CIFAR. What assumption did the convnet bake in that the MLP has to learn from pixels?

## You are done when

You can explain a convolution as a shared dot product on patches, and you can show one class pair your model still mixes up.
