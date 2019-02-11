
`ImageDataBunch` has convenient factory methods for loading your data. For weird datasets and edge cases, use `ImageItemList`.

`ImageItemList` class provides a more fine-graind way to define your data.

In fact, `ImageDataBunch` [calls](https://github.com/fastai/fastai/blob/c5e9bace1202ae1c2f50623b921ad52b1da8c1ed/fastai/vision/data.py#L114) `ImageItemList` under the hood.

[Data block API docs](https://docs.fast.ai/data_block.html)

---

Note on transformations (augmentation):
- set `flip_vert` to `True`. It's normal for satellites to see things upside down.
- set `max_warp` to `0`. Warp changes the perspective of the camera. Satellite images usually look straight down.

---

`metrics` shows how your model is doing. It doesn't affect training. You can have one, or a list of metrics: `metrics=[...]`

`f_score` is used by kaggle. It takes false-positives & false-negatives into consideration.

---

After training and unfreezing, if you run `lr_find()`, the loss plot can look like this:

![Imgur](https://i.imgur.com/nlMC4wv.png)

It's hard to find a spot where the loss definitely declines. Instead, take the value at the elbow (just before where it shoots up), and go back by 10x for the left part of the slice. The the right part, take the original learning rate and divide by 5 or 10.

---

Beyond finetuning

Start with a smaller image size. Train, unfreeze, and finetune. Then use the same model *freeze it*, and train it again on a larger image size. Then unfreeze it and finetune it again.

When starting small, don't go smaller than 64x64.

> yo dawg, i herd u like finetuning. so i finetuned your finetuned model...

---

Image segmentation is like a magnified version of classification: you're trying to classify every single pixel in an image.

---

use an unet to do segmentation

---

