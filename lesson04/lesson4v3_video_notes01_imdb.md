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

19:44 

## Questions

12:46 Does the language model transfer learning approach work for informal English or slang?

Yes, as long as you finetune it on the target corpus.

People thought that this wouldn't work well because Wikitext is not representative of how people write on the Internet.

