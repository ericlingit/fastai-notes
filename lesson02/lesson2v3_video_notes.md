Things to look out for during training:
- learning rate too high
- learning rate too low
- epochs too high
- epochs too low

`3e-3` (0.003) is a good default learning rate before you unfreeze. 

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
