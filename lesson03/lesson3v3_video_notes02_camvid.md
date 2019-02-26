## lesson3-camvid.ipynb

56:35

## Image segmentation

From this:

![Imgur](https://i.imgur.com/2mun06A.png)

to this:

![Imgur](https://i.imgur.com/tDvlLQ0.png)

57:19 Image segmentation is like a magnified version of classification: you're trying to classify every single pixel in an image.

59:28 [fastai datasets](https://course.fast.ai/datasets)

1:03:06 How to do image segmentation? Same as before:

1:04:01 The label image filenames simply have a `_P` before `.jpg`.

1:04:35 `open_image()` vs `open_mask()`: 
- `open_image` is for opening normal images (floats).
- `open_mask` is for loading images with integers.
- `mask.show` is for displaying masks

`codes.txt` has the color codes for each pixel color.

1:05:56 Create a DataBunch with data block API.

1:06:13 Why aren't train/valid sets split randomly? Because the images are frames of a video. Random splits can put two continuous frames into train/valid, one frame for each set. These frames are pretty much identical and is considered cheating. The `valid.txt` file prevents that, and is done for us (non-continuous).

1:06:51 Datasets: each pixel is labeled by an integer code. The English meaning of the code is stored in `code.txt`.

1:07:33 Transformations: when flipping the input data, the label must also be flipped! In `.transform`, include the arg `tfm_y=True` so that the `x` and `y` have matching transforms.

1:08:11 DataBunch batch size: we're using a smaller batch size of 8 because we're trying to classify every single pixel, it'll take up a lot more resource. If you run out of cuda memory, use `bs=4`.

1:08:34 fastai's `show_batch` automatically overlays the mask over the input image.

1:16:24 For **image segmentation**, we *could* use a CNN, but an **U-Net** is better. 

1:16:45 [U-Net](https://docs.fast.ai/vision.models.unet.html). On the left side is a normal CNN: it condenses an image down over many steps. On the right side, an U-Net reinflates the image back over an equal number of steps, and copies some info from the CNN stage.

![U-Net](https://docs.fast.ai/imgs/u-net-architecture.png)

1:18:03 For image segmentation, use an U-Net learner [`Learner.create_unet`](https://docs.fast.ai/vision.learner.html#unet_learner) to get the best result. The rest of the steps are the same as before!

1:18:52 `learner.recorder` records the stats during training. Its `plot_losses` function can sometimes show a slight increase in loss before a gradual decrease. Why? Because our learning rate also increased before gently dropping. Why? Because we used `fit_one_cycle`.

1:19:28 Fit one cycle: hikes the learning rate up before dropping it. Why?

1:21:44 The increase in learning rate helps escape local minima and find a proper global minima. The decrease helps settle down into a good global minima. This is called **learning rate annealing**.

1:23:01 Benefit of increasing learning rate during training: better generalization (stuck in local minima). An increase in learning rate helps the function explore the loss landscape and settle into a good, flat global minima.

1:25:00 So if you call `plot_losses`, and you see the loss go up slightly then down, you've found a good maximum learning rate. When you call `fit_one_cycle`, you're actually passing the maximum learning rate. **If** the loss curve only trends downwards, you could end up being stuck in a local minima. Try increasing the learning rate.

1:25:50 After each training cycle, it's a good idea to examine the loss plot. Very the learning rate to see how the plot will respond. This kind of practice will give you  a good sense of what works and what doesn't.

1:26:58 Step up the image size. Reduce the batch size. If you double the image size, reduce the batch size by half.

1:27:15 If you ran out of GPU memory, create a new learner and load the weights from previous stage.

1:27:57 Call `learn.show_results` to see how your model's predictions compare to the ground truth.

---

## Questions

1:00:28 Can `lr_find()` just return the optimal learning rate rather than eyeballing a graph?

No. Picking a good lr from the graph depends on what stage you're at and the shape of the loss curve. 

Experiment & play around with it to get a good sense.

1:10:03 Is it possible to use unsupervised learning to automatically create image masks?

Yes, but it won't be as accurate as human-labeled mask.

1:12:35 What does accuracy mean for pixel-level segmentation? Is it just correctly_predicted / all_pixels?

Yes. We can just pass the `accuracy` function as metric, but here we'll use another function because some pixels are labeled `void`. We'll exclude void pixels from accuracy calculation (as specified in the original camvid research paper).

1:15:04 My training loss is still higher than validation loss despite training for many epochs or with higher lr, help!

If you're still underfitting, you could decrease regularization. We'll talk about this in part 2. Topics include: weight decay, dropout, and data augmentation.

