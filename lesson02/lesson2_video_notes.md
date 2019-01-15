[Lesson 02 video](https://course.fast.ai/lessons/lesson2.html)

Lesson topics (copied from course page):
> more about image classification, covering several core deep learning concepts that are necessary to get good performance:

- What a learning rate is 
- How to choose a good learning rate
- How to vary your learning rate over time
- How to improve your model with data augmentation (including test-time augmentation)
- Practical tips (such as training on smaller images)
- 8-step process to train a world-class image classifier
- Pore information on your hardware setup

---

## Review
#### Learning rate

learning rate determines how fast you get to optimal solution

when implementing in python, we'll translate `10^-1` notation to `1e-1`:

notation | python | decimal
---------|--------|---------
`10^-1`  | `1e-1` | 0.1
`10^-2`  | `1e-2` | 0.01

Find the lowest point, and go back (left) by 1 order of magnitude (10x):

![Imgur](https://i.imgur.com/LOGGwJO.png)

Lowest point: 0.1

A good learning rate: 0.01 (or 0.001 if 0.01 doesn't work well)

---

## How to improve your model and avoid overfitting:

With many parameters, the neural net will start to tailor itself for the training set (like rote memorization), instead of taking the lesson & applying it elsewhere (generalization).

One solution is to give it more data.

But an easier & cheaper way is data augmentation.

## Data augmentation
Data augmentation means applying small tweaks to the data. For cats & dogs images, for example, zoom in a bit, rotate slightly, or flip horizontally.

The type of augmentation should be determined by the type of data you have. For example, to recognize words & digits, it wouldn't make sense to flip them horizontally.

