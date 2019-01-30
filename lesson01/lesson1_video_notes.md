# Notice

This is the 2018 course (v2), based on fastai v0.7.0. Course 2019 (v3) based on fastai v1.0.42 [has been released](https://www.fast.ai/2019/01/24/course-v3/).

---

[Lesson 01 video](https://youtu.be/IPBSB1HLNLo)

[Lesson 01 wiki](https://forums.fast.ai/t/wiki-lesson-1/9398)

## Course structure
The first-half of this course will run through each state-of-the-art models for different tasks. The second-half will be going into in-dept details of the models used and the theory behind them.

## Things to keep in mind

As you follow along, you should ...

> "Aim to get to through end of lesson 7 as quickly as you can, rather than aiming to fully understand every detail." - Jeremy @ 35:26

Focus on the code. See what goes in, and what comes out.

---

## How is a neural network useful?

A neural network has linear layers, and non-linears scattered among them. Their permutations can approximate any function. ie, solve any computable problem, given enough parameters. Think of it like this: __a combination of straight lines and curvy lines can approximate any shape__.

![Imgur](https://i.imgur.com/aufqpRw.png)

Gradient descent is a search through parameter space. In this space, you're trying to find the lowest point, where the combination of `b` and `m` (`m` is the weight, sometimes denoted as `w`) will produce the lowest `Loss`. __The parameters control where to curve and how much to curve in order to approximate a shape__. High loss means the curve doesn't fit the shape very well. Low loss means the curve follows the shape very well.

![Imgur](https://i.imgur.com/6j7AshF.png)

Thanks to GPU computing, we're able to search through that space fast and in large scales.

---

A convolution is a __linear__ operation: you take a pixel's values, multiply by some number, and then add them up.

![Imgur](https://i.imgur.com/RrACTtx.png)

Non-linearity transforms a linear value (straight line) into non-linear (curvy line).

[Here](http://neuralnetworksanddeeplearning.com/chap4.html) is an interactive demo showing how a combination of linear & non-linear units can approximate any shape. Go ahead and play with it!

Screenshot:
![Imgur](https://i.imgur.com/dEMrXUf.png)
 
 Notice how a higher weight `w` makes the curve more steep, and a lower `w` makes the curve flatter. Why is that? A high weight will amplify any input. While a low or negative weight has the reverse effect.
 
 A positive bias `b` shifts the curve left, and a negative bias shifts it right. Why? Since `y = sigmoid(wx + b)`, a negative `b` reduces the sum of `wx + b`, and thus 'delays' the activation by requiring a larger input `x`.

With numerous linear/non-linear units, you can not only approximate a line (2D), you can also appriximate more complex, multi-dimensional shapes:

![Imgur](https://i.imgur.com/dmrCAm2.png)

---

## Finding the optimal learning rate
Use `lr_finder`. it's part of `fastai` library:

```python
learn = ConvLearner.pretrained(arch, data, precompute=True)
lrf=learn.lr_find()
```

It starts with a small learning rate, and gradually increases it. At first, the loss will improve slowly, then quickly, then get worst again:

![Imgur](https://i.imgur.com/LOGGwJO.png)

Don't take the learning rate at the point of lowest loss. Why? Because that's where loss will start to climb dramatically. Instead, pick the learning rate with a good reduction in loss *just before* the bottom of the graph. 10^-2 (0.01).

## Picking an `epoch`
1 epoch = run thru all of the training data once (in batches).

1 batch = a small number of samples that are run thru the neural net together during training.

At the end of each epoch, check the validation accuracy and loss.

How many epochs should I run?
> "as many as you'd like ... if you run it for too long, the accuracy will start to get worse (overfitting) ... Run lots of epochs. Once you see it getting worse, you'll know how many epochs you can run." - Jeremy @ 1:19:49
