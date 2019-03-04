## lesson3-head-pose.ipynb

1:34:00

## The data

1:34:27 This dataset contains images of people in different poses, with coordinate labels marking a person's face.

Data directory structure:
```bash
~/.fastai/data/biwi_head_pose$ tree -L 1
.
├── 01/
|   ├── depth.cal
|   ├── frame_00003_rgb.jpg
|   ├── frame_00003_pose.txt
|   ├── frame_00004_rgb.jpg
|   ├── frame_00004_pose.txt
|   <snip>
|   ├── frame_00501_pose.txt
|   └── frame_00501_rgb.jpg
├── 01.obj
├── 02/
├── 02.obj
├── 03/
├── 03.obj
<snip>
├── 24/
├── 24.obj
├── io_sample.cpp
└── readme.txt

24 directories, 26 files
```

Each numbered directory from `01` to `24` is a different person. Each directory has the fames of a video.

1:34:40 Required preprocessing as speicified in the readme (not related to deep learning).

1:35:27 We're trying to figure out the coordinates of the center of the face: an x,y pair.

## Regression model

1:36:27 We need a model that returns numbers in a range: this is a **regression** problem. Previously, our models returned discreet, categorical values to deal with classification problems.

1:36:57 Regression just means that a model's output is a continuous number (or set of numbers).

#### Creating a DataBunch

1:37:41 `.split_by_valid_func()` Here, we use folder `13` as the validation set. Each numbered folder is a different person. We want to make sure the model can perform well on a person it's never seen before.

```python
.split_by_valid_func(lambda o: o.parent.name=='13')
```

`o` is a posixpath object.

`o.parent` is the full parent dir (up 1 level)

`o.parent.name` is the folder name of parent dir

![Imgur](https://i.imgur.com/bRWzbHW.png)

`split_by_valid_func`:
[docs](https://docs.fast.ai/data_block.html#ItemList.split_by_valid_func)
,
[source](https://github.com/fastai/fastai/blob/master/fastai/data_block.py#L200)

`.dataset()` specifies what type of dataset we're dealing with. Here, we're dealing with a `PointsDataset`.

For `transform()`, we'd also need to make sure that transformations are also applied to the ground truth `y`. e.g., if we flip an input image horizontally, we'd also need to move the target red dot. Pick a small `size` that'll work quickly.

Use `show_batch` to make sure our input and output (red dot) line up.

1:38:37 Don't worry if the red dot is not exactly in the center of the face.

#### Create a model

1:38:58 Like in previous lessons, we use a CNN here, but with a different loss function: mean square error (MSE).

1:39:39 Remember in lesson 2, we implemented MSE by hand. We'll use that as our error function.

The rest of the steps are exactly like we've practiced in lessons 1 and 2.
