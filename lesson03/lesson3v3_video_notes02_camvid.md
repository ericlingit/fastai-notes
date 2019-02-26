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

1:08:11 DataBunch batch size: we're using a smaller batch size of 8 because we're trying to classify every single pixel, it'll take up a lot more resource.

1:08:34 fastai's `show_batch` automatically overlays the mask over the input image.

1:09:04 Model. 

use an unet to do segmentation

---

## Questions

1:00:28 Can `lr_find()` just return the optimal learning rate rather than eyeballing a graph?

No. Picking a good lr from the graph depends on what stage you're at and the shape of the loss curve. 

Experiment & play around with it to get a good sense.

1:10:03 Is it possible to use unsupervised learning to automatically create image masks?

Yes, but it won't be as accurate as human-labeled mask.

