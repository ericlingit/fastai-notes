
## lesson3-planet.ipynb

Multi-label image classification is similar to what we did in lessons 1 and 2. Instead of having 1 label per image, we have many.

The workflow is largely the same:
1. load data
1. train
3. finetune

What's different here, is how we load data.


## [Data block API](https://docs.fast.ai/data_block.html)

We previously used [`ImageDataBunch`](https://docs.fast.ai/vision.data.html#ImageDataBunch). It has many convenient factory methods for loading image data. For datasets that can't be loaded with factory methods, use [`ItemList`](https://docs.fast.ai/data_block.html#ItemList).

[`ImageItemList`](https://docs.fast.ai/vision.data.html#ImageItemList) class (inherits `ItemList`) provides a more fine-graind way to define your image data.

In fact, `ImageDataBunch` [calls](https://github.com/fastai/fastai/blob/c5e9bace1202ae1c2f50623b921ad52b1da8c1ed/fastai/vision/data.py#L114) `ImageItemList` under the hood.

The general workflow for creating a `DataBunch` object with `ItemList` is as follows:
1. [Provide inputs](https://docs.fast.ai/data_block.html#Step-1:-Provide-inputs)
1. [Split the data into a training and validation sets](https://docs.fast.ai/data_block.html#Step-2:-Split-the-data-between-the-training-and-the-validation-set)
1. [Label the inputs](https://docs.fast.ai/data_block.html#Step-3:-Label-the-inputs)
1. [Optional: apply transformations](https://docs.fast.ai/data_block.html#Add-transforms)
1. [Optional: add a test set](https://docs.fast.ai/data_block.html#Add-a-test-set)
1. [Wrap in dataloaders and create a DataBunch](https://docs.fast.ai/data_block.html#Step-4:-convert-to-a-DataBunch)


#### [Transformations](https://docs.fast.ai/vision.transform.html) / augmentation

- set `flip_vert` to `True`. It's normal for satellites to see things upside down.
- set `max_warp` to `0`. Warp changes the perspective of the camera. Satellite images usually look straight down.


## Defining [metrics](https://docs.fast.ai/metrics.html)

`metrics` shows how your model is doing. It doesn't affect training. You can have one, or a list of metrics: `metrics=[...]`

[`f_score`](https://docs.fast.ai/metrics.html#fbeta) is used by kaggle. It takes false-positives & false-negatives into consideration.

---

After training and unfreezing, if you run `lr_find()`, the loss plot can look like this:

![Imgur](https://i.imgur.com/nlMC4wv.png)

It's hard to find a spot where the loss definitely declines. Instead, take the value at the elbow (just before where it shoots up), and go back by 10x for the left part of the slice. The the right part, take the original learning rate and divide by 5 or 10.

---

## Beyond finetuning

> yo dawg, i heard u like finetuning. so i finetuned your finetuned model!

Start training your model with a smaller image size: train, unfreeze, and finetune. Then *freeze* that model. Next, train it again with a larger image size. Then unfreeze it and finetune it again.

When starting small, don't go smaller than 64x64.

---

Image segmentation is like a magnified version of classification: you're trying to classify every single pixel in an image.

---

use an unet to do segmentation

---

