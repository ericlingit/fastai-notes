## lesson3-imdb.ipynb

1:41:08

We need to create 2 data bunches: one for the language model, and one for the classifier.

There are some preprocessing done to the input text when creating a databunch:
1. tokenization: turns the article into a list of words (tokens).
1. numiericalization: turn the list tokens (the vocabulary) into a list of numbers, ranked by frequency of appearance.

Create a language model, train it and save it.

Finetune the language model in the classifier.

We'll go deeper in the next lesson.

## Wrapping up

2:01:20 We started out with single-label classification. The exact same steps we used for that can be utitilized to do other tasks: multi-label classification (planet), image segmentation (camvid), image regression (head pose), and NLP classification (imdb), and a lot more!

The basic idea is to use gradient descent to find the best parameters for our model.

#### Non-linearity

2:03:00

Non-linearity is what gives neural networks the ability to approximate any function.

## Homework

2:03:30 Try to use what we've learned to solve a problem of your choice.

The hardest part will be creating the databunch.

## Questions

1:46:50 model and normalization

Always normalize with the same stats the pre-trained model was trained on so that the normalization doesn't end up distorting the inputs.

1:56:47 tokenization

RNN can learn the meaning of many words that show up together

1:59:09 how to deal with data that has weird number of channels when using a pre-trained model?

They'll try to improve `fastai` more to handle edge cases. If you have a missing channel, you can fill the missing channel with zeros or the average of the other channels.

For datasets with 4 channels, don't get rid of the extra channel. Modify the model instead: initialize the weights with an additional tensor rank (from 3 to 4). We'll learn how to do that in a future lesson.
