## lesson3-camvid.ipynb

56:35

## Image segmentation

From this:

![Imgur](https://i.imgur.com/2mun06A.png)

to this:

![Imgur](https://i.imgur.com/tDvlLQ0.png)

57:19 Image segmentation is like a magnified version of classification: you're trying to classify every single pixel in an image.


59:28 [fastai datasets](https://course.fast.ai/datasets)


---

use an unet to do segmentation

---

## Questions

1:00:28 Can `lr_find()` just return the optimal learning rate rather than eyeballing a graph?

No. Picking a good lr from the graph depends on what stage you're at and the shape of the loss curve. 

Experiment & play around with it to get a good sense.

