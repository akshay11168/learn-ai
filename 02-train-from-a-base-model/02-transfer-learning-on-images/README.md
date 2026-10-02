# 02 — Transfer learning on images

Prerequisite: [path 01 lab 05](../../01-train-from-scratch/05-images-and-convolutions/README.md).

## Purpose

Take a convnet trained on a large public image set, freeze its body, and train a new classification head on a small set of your own photos. Then unfreeze a few layers and see what changes. This is the image version of "train from another model."

## Explain before you code

1. Why should the new head see features computed by a frozen net, rather than raw pixels, when you only have a few dozen photos per class?
2. What can go wrong if the photos in validation were taken in the same burst as the photos in training?
3. Batch-norm layers in a frozen backbone were fit on the original dataset. What habit does `model.eval()` protect when those layers stay frozen?

## Build

Collect a tiny dataset of your own: 3 or 4 classes, at least 30 photos each if you can, phone photos are fine. Examples: mugs versus books versus plants, or rooms in your place. Put files in `D:\learn-ai-data\photos\<class>\`. Split by photo session, not by a random shuffle of near-duplicate frames. Hold out a test folder you do not open while tuning.

Use `torchvision.models.resnet18` with `weights=ResNet18_Weights.DEFAULT`. Replace the final linear layer with one that has your number of classes.

Experiment A: freeze every parameter except the new head (`requires_grad = False`), train the head with Adam and a small learning rate, batch size that fits, images resized to 224 and normalized with the mean and std the pretrained weights expect. Those numbers are in the torchvision weights meta. Using the wrong normalization silently hurts. Record validation accuracy.

Experiment B: train the same head on the same photos but with the backbone randomly initialized (no pretrained weights), same epoch budget. This is the from-scratch control.

Experiment C: unfreeze the last residual stage as well as the head, use a smaller learning rate, train a few more epochs.

Log all three in the experiment format from path 06. Plot the curves.

Error analysis on the validation photos: which class pairs fail, and whether the failures are bad labels, similar objects, or a dark photo the pretraining set rarely contained.

After the choice between A, B, and C is made, run the test folder once.

This lab is the technique behind [the photo use case](../../04-small-use-cases/02-classify-your-own-photos/README.md). Build the model here. Package it there when the numbers are honest.

## Verify

You can state which experiment won on validation and whether the pretrained body beat the random body by enough to matter on this set. If it did not, your notes say what was wrong with the data or the training, based on the error review.

## Stretch

Train with the backbone frozen for a few epochs, then unfreeze. Compare to unfreezing immediately. Write what the head is learning while the body is still fixed.

## Study this step

**Concepts to master**

- Early conv layers detect generic structure (edges, color, texture). Later layers become specific to the classes they were trained on. Transfer learning keeps the generic part and replaces the specific head.
- Freezing means `requires_grad=False` and not passing those parameters to the optimizer. Forgetting either one wastes memory or silently trains the backbone.
- Pretrained weights assume a preprocessing contract: resize, crop, and the channel mean and std used in the original training. Breaking the contract throws away the transfer.
- A random backbone with the same architecture is the control that tells you the data helped, rather than the architecture alone.
- Batch-norm layers contain running statistics. A frozen backbone should stay in `eval()` so those statistics are not recomputed on your tiny batches.
- Near-duplicate photos across the split inflate accuracy. The split unit is the capture session, not the file.

**Study**

- CS231n transfer-learning notes: https://cs231n.github.io/transfer-learning/ — their three regimes (tiny, medium, and large target data) map directly onto how much you unfreeze. Write which regime your photo set is.
- Sebastian Ruder, "Transfer Learning - Machine Learning's Next Frontier": https://www.ruder.io/transfer-learning/ — read the taxonomy section. Place this lab under inductive transfer from a supervised image net.
- torchvision ResNet18 weights documentation, including the preprocessing transforms in the weights enum: https://pytorch.org/vision/stable/models/generated/torchvision.models.resnet18.html — copy the mean and std from the page into your code and cite the page in `notes.md`.

**Practice**

- Print how many parameters require grad in experiments A, B, and C. The frozen run should show only the new head.
- Deliberately use mean `0` and std `1` instead of the official normalization for one epoch. Compare validation accuracy to the correct preprocessing. That gap is the contract.
- Find two photos that are almost the same frame. Force one into train and one into validation in a throwaway split, score it, and then restore the honest split. Write how much the score moved.

**Practice questions**

1. You replace the final layer of ResNet-18. The old layer mapped 512 features to 1000 classes. Your new layer maps which size to how many classes?
2. The backbone is frozen but the loss still takes a long time and the GPU memory looks like full training. What did you forget?
3. Why can unfreezing all layers with learning rate `1e-3` destroy features the pretraining spent a large compute budget to learn?
4. Experiment B (random init) beats experiment A (pretrained) on your set. Give two data-side explanations you would check before you conclude pretraining does not work.
5. `model.train()` on a frozen batch-norm network changes which numbers? Why is that harmful on a batch of 4?
6. A test photo of a plant on a desk is classified as "book" with probability 0.51. What does the lab's later threshold idea need to do with this output, and why is 0.51 not "a book"?

## You are done when

A new photo, taken after the split, can be classified by a script that loads your checkpoint, and you know which experiment that checkpoint came from.
