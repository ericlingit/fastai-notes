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

If you've made changes to the neural network (eg unfreezing layers), you should re-run learning rate finder.

---

## How to improve your model and avoid overfitting:

With many parameters, a neural net will start to tailor itself for the training set (like rote memorization), instead of taking the lesson & applying it elsewhere (generalization).

The best solution is to give it more data.

But an easier & cheaper solution is data augmentation.

## Data augmentation
Data augmentation means applying small transformations to the data. For cats & dogs images, for example, zoom in a bit, rotate slightly, or flip horizontally.

![Imgur](https://i.imgur.com/fPoIw1M.png)

This artificially inflates the amount of data you have.

The type of augmentation should be different for different types of data. For example, to recognize letters & digits, it wouldn't make sense to flip them horizontally. While it makes sense to flip satellite images vertically.

Augmentation is applied on this line:

```python
ImageClassifierData.from_paths(PATH, tfms=tfms_from_model(arch, sz))
```

We can specify the transformations we want to do:

<pre lang="python">
tfms = tfms_from_model(arch, sz, <strong>aug_tfms=transforms_side_on, max_zoom=1.1</strong>)
data = ImageClassifierData.from_paths(PATH, <strong>tfms=tfms</strong>)
</pre>

`transforms_side_on`: for photos taken from the side. ie, side view of an object. Applies horizontal flips, slight rotations, slight zooms, small translations, or varied brightness & contrast.

---

Learning rate annealing = decrease learning rate as you train, so you don't overshoot minima.

<dl>
    <a href="https://www.dictionary.com/browse/anneal"><dt>Anneal, verb</dt></a>
    <dd><ol>
        <li>to heat (glass, earthenware, metals, etc.) to remove or prevent internal stress.</li>
        <li>to free from internal stress by heating and gradually cooling.</li>
        <li>to toughen or temper.</li>
    </ol></dd>
</dl>

Cosine annealing = the rate of decrease follows one-half of a cosine curve. ie, stay on high lr when you're still far away (from the minima), then quickly decrease lr when you get close.

![Imgur](https://i.imgur.com/VqgjvjJ.png)

