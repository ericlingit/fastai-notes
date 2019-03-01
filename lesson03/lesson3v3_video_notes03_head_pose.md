## lesson3-head-pose.ipynb

1:34:00

1:34:40 Preprocessing as speicified in the readme (not related to deep learning).

1:35:27 We're trying to figure out the coordinates of the center of the face: an x,y pair.

We need a model that returns numbers in a range: this is a **regression** problem. Previously, our models returned discreet, categorical values to deal with classification problems.

1:36:57 Regression just means that a model's output is a continuous number (or set of numbers).

#### Creating a dataset

1:37:41 `.split_by_valid_func()` Here, we use folder `13` as the validation set. Each numbered folder is a different person. We want to make sure the model can perform well on a person it's never seen before.

`.dataset()` specifies what type of dataset we're dealing with. Here, we're dealing with a `PointsDataset`.

For `transform()`, we'd also need to make sure that transformations are also applied to the ground truth `y`. e.g., if we flip an input image horizontally, we'd also need to move the target red dot. Pick a small `size` that'll work quickly.

Use `show_batch` to make sure our input and output (red dot) line up.

Don't worry if the red dot is not exactly in the center of the face.

#### Create a model

1:38:58 Like in previous lessons, we use a CNN here, but with a different loss function: mean square error (MSE).

1:39:39 Remember in lesson 2, we implemented MSE by hand. We'll use that as our error function.

The rest of the steps are exactly like we've practiced in lessons 1 and 2.
