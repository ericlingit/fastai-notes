# NLP

2:05

3:42 text classification problem

3:51

Why it's hard: our dataset consist of many words and each record has a category of positive or negative label. There isn't a lot of clues for a neural network to really understand the English language.

The solution? Transfer learning!

6:29 We'll start with a pre-trained language model.

## What is a language model?

6:39

It's a model that can predict the next word in a sentence.

To predict the next word, it must have "world knowledge" (ie, common sense), and a good understanding of the language.

10:30

We take a pre-trained language model that is already pretty good (Wikitext103) and finetune it using IMDB text. Now we have a new language model that not only understands the English language, but also knows how to compose movie reviews.

11:25 We don't need labels to train a language model. 

## lesson3-imdb.ipynb

15:13

Data columns:
- label: positive or negative
- text: the text of movie reviews
- is_valid: validation set (True or False)

We can use the `TextClasDataBunch` to create a DataBunch. It'll automatically tokenize and numericalize for us. Words like `it's` are broken into `it` and `'s`.

The list of unique words (ranked by frequency) is your vocabulary.

17:16 In our weight matrix, every word has a vector that represents it. To avoid that weight matrix from getting too big, we limit the number of words to 60,000. We also omit words that appear fewer than twice.

Those omitted words are replaced by a `xxunk` (unknown) token. There are other special tokens like `xxcap` that denotes words in all-caps.

18:30 DataBunch with data block API:
- We're dealing with text, so we use `TextList`, and because our data is in a csv file, we call `load_csv`.
- `.split_from_df(cols=2)`: our validation set is specified in col 2
- `.label_from_df(cols=0)`: our target label is specified in col 0
- `.databunch()`: create databunch.

19:21 The full IMDB dataset has 50,000 reviews: train/valid is split 50/50; and another 50,000 unlabeled reviews.
- `train/pos`: positive reviews
- `train/neg`: negative reviews
- `train/unsup`: unlabeled samples

### Language Model

19:44

Lucky for us, someone has already trained the Wikitext language model (takes 2-3 days!) and made it available.

Even if you have text from specific fields like medicine, you should still start from Wikitext. Use transfer learning whenever possible.

#### Training data for language model

20:33 The full IMDB dataset is a list of text files. Each review is in its own file, and they're all stored in the same folder; so we use `TextFileList.from_folder(path)`.

20:54 Why randomly split by 10%? When training a language model, you don't care about the labels. So you can combine the training and test set and have a smaller validation set. Use as much text as you can get when training a language model.

22:03 So how do we label? Remember that a language model has its own internal label: the next word **is** the label. So we use `.label_for_lm()`.

22:33 Create a language model learner. This creates a RNN. The basic structure largely the same: a lot of matrix multiplication with negative values zeroed out (relu).

For our learner, we need the training data, the pretrained model to use (Wikitext103; WT103), and dropout. Dropout reduces overfitting.

When we `fit_one_cycle`, we're fientuning the last layers of Wikitext103.

As usual, we'd then unfreeze and train some more with a lower learning rate. Note that this takes 2+ hours even on a high-end GPU.

25:00 An accuracy of 0.3 means we correctly guess the next word 1/3 of the time. This level of accuracy is actually quite good, and is a good sign that our language model is doing well.

We can now predict the next words of a sentence. But this is not trained to be good a text generation. We're only checking that it can follow correct grammatical rules.

At this point, we have a language model that understands movie reviews.

26:33 A language model as 2 primary parts: one part understands the sentences, and the other predicts what word comes next. We need only the first part: the encoder. We'll save it and use it for our classifier.

### Classifier

27:17

#### Training data for classifier model

We'll load the same data with one difference: we'll load the vocabulary set from the language model training data. So that for both models, their vocabularies match.

Last time, we split randomly. This time, we need to specify the validation set is in the `test` folder because we're training a classifier. We also need to specify the label classes.

Use a lower batch size for the databunch to avoid running out of GPU memory.

29:07

## Questions

12:46 Does the language model transfer learning approach work for informal English or slang?

Yes, as long as you finetune it on the target corpus.

People thought that this wouldn't work well because Wikitext is not representative of how people write on the Internet.

