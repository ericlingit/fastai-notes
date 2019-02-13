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



####  Look at a simple linear model


## 56:38 Questions 
#### 56:46 How is `error_rate` calculated?

####  59:22 Default learning rate
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
    `xxx` is where the strongest slope is on `lr_find()`.

---

[Animated demonstration of matrix mutiplication](http://matrixmultiplication.xyz/)

The `@` operator is matrix multiplication in python 3.5+.

A tensor is just an array with symmetric shape.

Arrays have dimension, and tensors have ranks.

An array with 3 dimensions is a rank 3 tensor.

```python
x = torch.ones(n,2)
x[:,0].uniform_(-1,1)
```
`x` is a n-by-2 tensor. It's a rank-2 tensor because we passed 2 args. `n` is 100, so we have a 100 rows by 2 columns tensor of ones. 

`x[:,0]` indexes all rows in column 0. `.uniform_(-1,1)` *updates* all of the values in the indexed range to between -1 and 1. In pytorch, functions that end with an underscore will update the variable that called the function (instead of simply returning a value w/o modifying the variable).

The above points cover 95% of what you need to know with pytorch:
1. how to create an array (tensor)
1. how to change the values in that array (indexing and value updates)
1. how to multiply arrays (use `@`)

```python
y = x@a + torch.rand(n)
```
Here, `torch.rand(n)` adds noise to our function, so we don't end up with a straight line of dots.

Our goal here is to find the values of `a` (pretend that we didn't set them ourselves). 

Why is this useful? A simple linear function `y = mx + b` takes an input `x`, and outputs a value `y`. A neural network is basically the same, but with many more inputs, outputs. If we can map the relationship between the inputs (eg pixel values of an image) and the outputs (eg a decimal value for each dog/cat breed), we'll have a useful and predictive function (what breed is the cat shown in img002.jpg?).

The relationship we're trying to find is the weights (`m` and `b` in the linear function example).
