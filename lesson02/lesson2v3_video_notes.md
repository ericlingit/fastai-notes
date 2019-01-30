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

