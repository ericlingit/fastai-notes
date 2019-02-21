
## lesson3-planet.ipynb

14:49 Multi-label image classification is similar to what we did in lessons 1 and 2. Instead of having 1 label per image, we have many.

The workflow is largely the same:
1. load data
1. train
3. finetune

What's different here, is how we load data.


## [Data block API](https://docs.fast.ai/data_block.html)

16:05 We previously used [`ImageDataBunch`](https://docs.fast.ai/vision.data.html#ImageDataBunch). It has many convenient factory methods for loading image data. For datasets that can't be loaded with factory methods, use [`ItemList`](https://docs.fast.ai/data_block.html#ItemList).

[`ImageItemList`](https://docs.fast.ai/vision.data.html#ImageItemList) class (inherits `ItemList`) provides a more fine-graind way to define your image data.

In fact, `ImageDataBunch` [calls](https://github.com/fastai/fastai/blob/c5e9bace1202ae1c2f50623b921ad52b1da8c1ed/fastai/vision/data.py#L114) `ImageItemList` under the hood.

18:10 `DataBunch` class itself is implements pytorch's `Dataset` abstract class, and `DataLoader` class. The `DataLoader` class loads a batch of data and places them into the GPU for training.

24:25 The general workflow for creating a `DataBunch` object with `ItemList` is as follows:
1. [Provide inputs](https://docs.fast.ai/data_block.html#Step-1:-Provide-inputs)
1. [Split the data into a training and validation sets](https://docs.fast.ai/data_block.html#Step-2:-Split-the-data-between-the-training-and-the-validation-set)
1. [Label the inputs](https://docs.fast.ai/data_block.html#Step-3:-Label-the-inputs)
1. [Optional: apply transformations](https://docs.fast.ai/data_block.html#Add-transforms)
1. [Optional: add a test set](https://docs.fast.ai/data_block.html#Add-a-test-set)
1. [Wrap in dataloaders and create a DataBunch](https://docs.fast.ai/data_block.html#Step-4:-convert-to-a-DataBunch)

29:57 Here the data block API is split into 2 cells because we'll be tweaking our DataBunch object later.

30:18 Note on [transformations](https://docs.fast.ai/vision.transform.html):
- set `flip_vert` to `True`. It's normal for satellites to see things upside down.
- set `max_warp` to `0`. Warp changes the perspective of the camera. Satellite images usually look straight down.

32:32 The types of transforms to apply depends on the data you have.

33:00 The rest of the steps for creating a classifier is largely the same as before. The differences are the architecture used, and the metrics.

## Defining [metrics](https://docs.fast.ai/metrics.html)

33:49 `metrics` shows how your model is doing. It doesn't affect training. You can have one, or a list of metrics: `metrics=[...]`. The `metrics` argument takes a function name (a `Callable`) or a list of function names.

34:37 `f_score` is used by kaggle. It takes false-positives & false-negatives into consideration.

35:27 [`fbeta `](https://docs.fast.ai/metrics.html#fbeta) uses F2 by default, and has a threshold of `0.2`.

36:01 What does `thres=0.2` mean? In previous lessons, we tried to classify pet breeds by selecting the *one* category with the highest probability. Now, for multi-label classification, we want to select *all categories* with probabilities greater than `0.2`.

37:42 `data.c` is the number of outputs a model can create. In planets example, we have 17 classes. Each class has a probability. That list of probabilities is what we're trying to predict in this lesson.

## Partial

38:56 In `create_cnn`, the `metrics` argument needs to be a Callable (or a list of Callables). But how do we specify the arguments to the metrics function? To **preload** a function's arguments with a value, use python's built-in [`partial()`](https://docs.python.org/3.7/library/functools.html#functools.partial) function.

```python
def old_func(arg1=99):
    return arg1**2

new_func = partial(old_func, arg1=0.1)
```

In the above example, we can see that `partial` *preloads* the function `old_func` with `arg1=0.1` and returns to `new_func`. When `new_func` is called, its argument will be `arg1=0.1`.

In our multi-label classification example, we need to preload `accuracy_thresh` with `thresh=0.2`; and preload `fbeta` with `thresh=0.2`:

```python
acc_02 = partial(accuracy_thresh, thresh=0.2)
f_score = partial(fbeta, thresh=0.2)
```

39:28 How is this useful? It can remove the need to write a new function.

Creating a new function that is identical to another function, and only differ in the value of arguments is called a **partial function**.

41:03 The next steps are identical to what we've learned from lessons 1 & 2.

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

