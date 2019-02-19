## 16:29 How to create your own dataset (teddy bear identifier)

Go ahead and build a model and train it. The incorrectly labeled images and images that don't belong will bubble up as most-confused and least confident.

#### 31:31 Clean up dataset

#### 35:18 Jupyter notebook widgets (apps)

#### 37:36 The model can deal with random noise well.
It's normal that the model improves only very slightly after data clean up. The problem is when you data is biased.

#### 38:27 Putting model in production
You're more likely to run the production model on a CPU instead of a GPU. A GPU is efficient when many users are sending many requests at a time for batch inference. Unless your website is visited by lots of people every second, a CPU is sufficient. It's less complicated and easier to setup.

#### 41:16 Inference

#### 43:39 Serve your model as a web app

#### 43:52 Python `await`, `async` keyword
Lets you wait for some long-running task to be done without taking up/holding up a process.

## 46:51 Things that can go wrong
- learning rate too high
    - symptom: very high validation loss
    - fix: decrease your learning rate
- learning rate too low
    - symptom:
        - error_rate improves, but very slowly
        - underfitting: training loss > valid loss
    - fix: increase your learning rate
- epochs too low
    - symptom: 
        - underfitting: training loss is much higher than valid loss
    - fix: increase number of epochs. if still slow to improve, increase the learning rate
- epochs too high
    - symptom: overfitting: error_rate improves, but then deteoriates
    - fix: reduce the number of epochs

#### 50:45 It's hard to overfit
Many people claim that train_loss < valid_loss is a sign of overfitting. This is not true. A correctly trained model will always have train_loss < valid_loss. It's not overfitting as long as the error_rate is improving.

## 53:11 Understand what goes on under the hood

In the previous teddy bear detector example, we have these **inputs**: the pixel values of each image. We then have these **outputs**: a 3-number value each represents the probability of either a black bear, a brown bear, or a teddy bear.

In the digits-recognition example, the input is the pixel value of each number image. The output is a 10-value list of numbers. Each value represents the probability that the image is a particular number between 0-9.

Use `np.argmax` to find the index of the highest value. It's what `learn.predict()` uses.

#### 1:02:19 Look at a simple linear model

`y = a*x + b`

Pair a1 with x. Replace `b` with `a2`, and pair it with a `1`. This makes matrix multiplication easier:

`y = a1*x + a2*1`

We can further replace `1` with `x2` and set it to always equal to `1`:

```
Given x2 = 1
y = a1*x1 + a2*x2
```

`yi` means there are many values of `y`, and `xi` means there are many values of `x`. This is the same as indexing in a list.

So if `x` is a list of numbers from 1-10, `x1` is the same as `x[0]`.

`a1` and `a2` (`b`) remain the same. They're the coefficient (or parameters).

When you have multi-value variables that have to multiply by one another, it's easier to turn them into equal-length lists and use matrix/vector multiplication. This saves you from having to write loops to iterate through every number in a list.

`y_vec = X * a_vec`

`X` is a matrix of `x1` and `x2` (where `x2` is `1`s).

`a_vec` is a vector of 2 values: `a1` (weight), and `a2` (aka `b` or bias).

[Animated demonstration of matrix mutiplication](http://matrixmultiplication.xyz/)

#### 1:14:13 Why vectorize?
So when we code, we won't have to write loops, and it's faster.

#### 1:16:14 [Lesson2-sgd.ipynb](https://github.com/fastai/course-v3/blob/master/nbs/dl1/lesson2-sgd.ipynb): fit `y_vec = X * a_vec` to some data using SGD

Generate random data `x1 and x2`:

```python
n = 100 # the number of records/data points
x = torch.ones(n,2) # 2 columns of ones: x1 and x2
x[:,0].uniform_(-1.,1) # set random numbers for x1
```
Remember, x2 is always 1.

`x[:,0]` indexes all rows in column 0. `.uniform_(-1,1)` *updates* all of the values in the indexed range to between -1 and 1. In `pytorch`, functions that end with an underscore will update the variable that called the function (instead of simply returning a value w/o modifying the variable).

Create some coefficients `a1` and `a2`:
```python
a = tensor(3.,2)
```

Our 'architecture':
```python
y = x@a + torch.rand(n)
```

`x@a` is the matrix product between `x` and `a` (this notation was introduced in python 3.5).

Adding `torch.rand(n)` gives `y` some randomness.

#### 1:18:15 tensor
A tensor is just an array with a regular shape: rectangular or cubed (meaning, every column has the same length, and every row has the same length).

Arrays have dimensions, and tensors have ranks. An array with 3 dimensions is a rank 3 tensor.

In our simple linear equation example, `x` is a n-by-2 tensor: a rank-2 tensor because it has 2 dimensions. `n` is 100, so we have a 100 rows by 2 columns tensor of ones. 

**Note**: in code, the shape of a 2D tensor is expressed  in `rows x columns`.

The above points cover 95% of what you need to know with pytorch:
1. how to create an array (tensor)
1. how to change the values in that array (indexing and value updates)
1. how to multiply arrays (use `@`)

![Imgur](https://i.imgur.com/C6HdFL8.png)

Our goal here is to find the values of `a` (pretend that we didn't set them ourselves).

Why is this useful? A simple linear function `y = ax + b` takes an input `x`, and outputs a value `y`. A neural network is basically the same, but with many more inputs and outputs. If we can map the relationship between the inputs (eg pixel values of an image) and the outputs (eg a decimal value for each dog/cat breed), we'll have a useful and predictive function (eg, what breed is the cat shown in img002.jpg?).

The relationship is the weight `a` and bias `b` (combined into `a_vec` in the linear function example) that has the least amount of error (ie, the line the function draws fits the dots well).

The 'error' is the average distance between the line and the dots. The lower the distances, the lower the error (or loss).

#### Bad fit, high error:

![Imgur](https://i.imgur.com/d5dh7eg.png)

#### Good fit, low error:

![Imgur](https://i.imgur.com/6Oytlfh.png)

#### 1:30:31 Error is measured using mean squared error (mse)

It tells you how good your line is. The lower the loss, the better.

#### 1:34:18 How to come up with the line

You have to start by making random, wild guesses for `a1` and `a2` (in `a_vec`).

Let's say, we use -1 and 1 for `a1` and `a2`.

#### 1:35:00 Make sure you use `floats` and not `int`

Simply adding a dot after a number will do. Like this: `1.`

#### 1:35:50 Make our predictions using randomized `a_vec`, and use MSE to measure the error.

It'll look like this:

![Imgur](https://i.imgur.com/d5dh7eg.png)

#### 1:36:57 What next? How do we improve on that?

We can try increasing or decreasing `a1` and `a2` to see how they affect the line. `a1` is the slope, so increasing it will tilt the line up. `a2` is the y-intercept, so increasing it will translate (shift) the line up.

Just eyeballing the above chart tells us that we need to tilt the line up, and shift the y-intercept (at x=0) upward.

How could we achieve the same without having to eyeball the chart? Use derivatives from calculus!

Calculating the derivative helps us know whether to increase or decrease the parameters.

#### 1:39:12 Gradient descent

The pytorch function that calculates the gradient: `tensor.backward()`

#### 1:41:12 Learning rate

More detail on gradient descent and learning rate is covered in the Introduction to Machine Learning course.

#### 1:46:24 Animate gradient descent

Rather than using a loop, use matplotlib's animation function.

Replan the animation with a too-large learning rate, and a too-small learning rate. See what effects that'll have.

#### 1:48:06 Mini batches

In every iteration (a single update of parameters), our example measures the error between the line and *all* points of data. If the dataset (the number of points) is many millions, measuring the error on all points will become very slow. Instead, we can measure error against a small portion of the data, then update the parameter. This small portion of data is a **mini-batch**.

---

## Questions 
#### 56:46 How is `error_rate` calculated?

####  59:22 Default learning rate / Why use 3s for `learning_rate`?
`3e-3` (0.003) is a good default learning rate before you finetune.

A rule of thumb for learning rate range after unfreezing: for the upper bound of the slice, use a value 1/10 of the default learning rate. Use `lr_find()` to find the lower bound.

The process generally looks like this:
1. run a few epochs at `3e-3`:
    ```python
    learn.fit_one_cycle(4, 3e-3)
    ```
1. unfreeze:
    ```python
    learn.unfreeze()
    ```
1. run a few more epochs with a slice lr
    ```python
    learn.fit_one_cycle(4, slice(xxx, 3e-4))
    ```
    `xxx` is where the strongest down slope is on `lr_find()`.

#### 1:08:35 How many images is enough? How do you know you don't have enough data?

You found a good leanring rate, but after training long enough (until you start to overfit), you're still not satisfied with the accuracy.

The solution is get more data.

There's no shortcut to know how much data you'll need before hand. Start with a small amount first, then see how it goes before gathering more.

#### 1:10:00 What to do if you have unbalanced classes?
You can take the uncommon class and make duplicates (over-sampling). But Jeremy says things work just fine without having to do anything special.

#### 1:11:04 What to do if after finetuning, it's still underfitting?
Just try it: run another cycle or start over with more epochs. Jeremy normally runs a few more cycles.

#### 1:12:04 What's in resnet? When we saved the teddy-bear model, the file was about 85MB. What's it?
resnet is just a function (formula/architecture). When you save/export a model, what you're saving is the parameters/weights. When you use a pretrained model, you're reusing the weights someone has exported.

