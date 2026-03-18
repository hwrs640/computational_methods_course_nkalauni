# HWRS640 - Assignment 3: Convolutional Neural Networks for Satellite Image Segmentation

## Due date: Monday, March 30th at 11:59 PM

---

## Background

Semantic segmentation — assigning a class label to every pixel in an image — is one of the most widely used deep learning tasks in Earth observation. Applications include land cover mapping, flood extent delineation, urban growth monitoring, and vegetation change detection. In this assignment you will train a convolutional neural network to segment multi-class satellite imagery and critically evaluate its performance.

The dataset is hosted on HuggingFace and can be loaded with:

```python
from datasets import load_dataset
ds = load_dataset("nikolkoo/SatelliteSegmentation")
```

You may need to install the `datasets` library with `pip install datasets` or `uv sync` if you are using the uv environement. The dataset contains ~1000 RGB satellite images of size 256x256, each paired with a pixel-wise segmentation mask of the same size. There are 5 classes in total, with class codes that are multiples of 10 (e.g., 0, 10, 20, 30, 40). You will need to remap these to contiguous indices before training your model.

The dataset contains RGB satellite images paired with pixel-wise segmentation masks. Each mask pixel carries an integer class code. **Important:** the class codes are not contiguous (they are multiples of 10). You must remap them to contiguous indices $0, 1, \ldots, C-1$ before passing masks to your loss function.

---

## Problem 1: Data exploration and preprocessing (20 points)

1. Load the dataset and report its size and structure (number of samples, image dimensions, number of classes).
2. Visualise at least 5 image–mask pairs. Display the raw satellite image, the segmentation mask, and an overlay of the mask on the image. Use a consistent colormap across all figures so that the same class always appears in the same colour.
3. Compute and plot the **class balance**: what fraction of all pixels belongs to each class? Discuss in 2–3 sentences what implications this has for training a classifier.
4. Build a PyTorch `Dataset` and `DataLoader`. Your `Dataset` must:
   - Return `(image_tensor, mask_tensor)` pairs where the image is a normalised `float32` tensor of shape `(3, H, W)` and the mask is a `int64` tensor of shape `(H, W)` with values in $\{0, \ldots, C-1\}$.
   - Resize all images and masks to a fixed spatial resolution. Choose a size that is divisible by 8 (required for the encoder–decoder pooling path).
   - Use nearest-neighbour interpolation when resizing masks to avoid introducing spurious class values.
   - Split the data into 80% training and 20% validation sets.

---

## Problem 2: Model design and implementation (25 points)

Design and implement a convolutional segmentation model of your choice. Your model must:

- Accept a `(B, 3, H, W)` batch of images and produce a `(B, C, H, W)` tensor of per-pixel class logits.
- Consist entirely of convolutional operations (no fully connected layers that collapse spatial dimensions).

You have freedom in the specific architecture. Two natural options are:

- A **U-Net**-style encoder–decoder with skip connections, which explicitly preserves spatial detail through the network.
- A **ResNet**-style backbone with a lightweight decoder head, which benefits from the residual connections that ease gradient flow in deeper networks.

Whatever you choose, you must justify your architectural decisions in a short written discussion (3–5 sentences): why is this architecture appropriate for a segmentation task compared to a standard classification CNN? What role do skip connections or residual connections play?

Report the total number of trainable parameters in your model.

---

## Problem 3: Training (25 points)

Train your model for at least 20 epochs using the following specifications:

**Loss function:** Use **pixel-wise cross-entropy loss** (`nn.CrossEntropyLoss`), which accepts the raw logit tensor of shape `(B, C, H, W)` and an integer mask of shape `(B, H, W)`. Because the class distribution is unequal, compute **inverse-frequency class weights**:

$$w_c = \frac{\text{total pixels}}{C \times \text{pixels of class } c}$$

and pass them to the `weight` argument of `CrossEntropyLoss`.

**Optimiser:** Use Adam or AdamW with a learning rate of your choice. You may additionally use a learning rate scheduler (e.g., `ReduceLROnPlateau` or cosine annealing).

**Produce the following plots:**
- Training loss and validation loss vs. epoch on the same axes.
- Validation mean IoU vs. epoch.

Discuss in 2–3 sentences: does your model appear to overfit? What evidence supports your conclusion?

---

## Problem 4: Evaluation and interpretation (30 points)

### 4a. Quantitative evaluation

Evaluate your trained model on the validation set and report:

1. The **mean Intersection over Union (mIoU)**, defined as:

$$\text{mIoU} = \frac{1}{C} \sum_{c=0}^{C-1} \frac{\text{TP}_c}{\text{TP}_c + \text{FP}_c + \text{FN}_c}$$

where $\text{TP}_c$, $\text{FP}_c$, $\text{FN}_c$ are the true positives, false positives, and false negatives for class $c$ across the entire validation set.

2. A **per-class IoU bar chart** showing the IoU for each class individually, with a horizontal dashed line marking the mIoU. Label each bar with its class code.

3. Discuss: which classes does your model struggle with most? Does this correlate with the class frequencies you computed in Problem 1? (3–4 sentences)

### 4b. Qualitative evaluation

Visualise predictions for at least 4 validation samples. For each sample show:
- The input satellite image (with normalisation undone for display).
- The ground-truth mask.
- The predicted mask, with the per-sample mIoU printed in the title.

Identify and discuss one example where the model succeeds and one where it fails. What spatial or spectral features might explain the failure? (3–4 sentences)

### 4c. Reflection

Answer the following questions in a short paragraph (4–6 sentences total):

- IoU is preferred over pixel accuracy for segmentation. Explain why, using the class balance you computed in Problem 1 as a concrete example.
- How would you expect model performance to change if you used a pretrained encoder (e.g., a ResNet backbone pretrained on ImageNet) instead of training from scratch? Why might pretrained features transfer usefully from natural images to satellite imagery despite the domain shift?
