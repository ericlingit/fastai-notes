# NLP

2:05

3:42 text classificatio problem

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



## Questions

12:46 Does the language model transfer learning apprach work for informal English or slang?

Yes, as long as you finetune it on the target corpus.

People thought that this wouldn't work well because Wikitext is not representative of how people write on the Internet.

