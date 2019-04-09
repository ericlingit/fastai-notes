# Collaborative Filtering

53:08

Representing information on "who bought what"

You have info like 'who bought what items'. The data is basically columns of user ID and the list of items they bought.

58:06 `get_collab_learner`

`n_factors=50` is the architecture. We'll learn what `factors` means later.

`min_score=0`, `max_score=5.` let the learner know wha the range of scores are.

You can now pick an user ID and a movie ID and predict if this user will like the movie. In practice, there's the cold-start problem.

59:09 the cold-start problem

The most useful time for the prediction model to make a recommendation is when you have a new user or a new movie. But you have no data at that point in time. `fastai` currently doesn't handle this problem.

One solution is to use another model that looks at the meta data for new users or movies and make recommendations based on that.

Another solution employed by Netflix is to ask new users if they've seen certain films and have the users rate them, then ask follow up questions based on the answers to 'pre-heat' the lack of data.

If you don't have the freedom of asking users for input, try to gather the background information of your users (e.g. age, location, nationality, language) and make initial recommendations based on that.

Collaborative filtering is for when you have collected sizable info on your users.

1:06:35 The half way point

We're now at the half-way point of the MOOC part 1, we'll start to look at the theory and dig into the code.

## Questions

1:01:36 How does the language model perform on mixed-language data. Example: English mixed with French; or text with emoji.

Text with emoji is fine as long as you finetune the language model with a corpus where emojis are used.

Jeremy uses mandarin as an example where you could map chinese characters to pinyin.

1:05:08 Time series on tabular data. Is there a RNN in tabular.models?

Generally you don't use RNN for time series tabular data. Instead you extract columns (e.g. day of week; weekend or not). Doing that will give you state-of-the-art result. We'll look at it next week.

1:06:13 Resources to learn about cold-start problem

He'll look it up.
