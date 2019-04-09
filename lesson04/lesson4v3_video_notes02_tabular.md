# Tabular Data

36:40

## lesson4-tabular.ipynb

41:56

43:04 For a TabularList object, you need to let it know which columns contain categorical values, and which columns contain continuous values.

Categorical values will be transformed into embeddings. Continuous values are already numerical, so they can be used as is. This is why you have to let TabularList know which columns are categorical, and which ones aren't.

45:04 `procs`

Similar to image transforms, `procs` is the list preprocessings to be applied to the dataframe. The difference from image augmentation is that a set of specific text preprocessings is done once to the whole dataframe; image augmentation is random changes that are applied to the input images as they're fed into the model.

- `FillMissing` deals with missing values by filling them with the median and add a column specifying whether the cell had a missing value
- `Categorify` turns categorical values into pandas categories
- `Normalize` normalizes continuous values between 0-1.

Remember: whatever processing you apply to the training set, you need to do the same to the test/validation set. Conveniently, `fastai` handles those details for you!

47:15 `split_by_idx`

If your data has structure, it's a good idea to split the validation set together (and not randomly) so that you won't end up with validation data that are very similar to the training set. For example, if you split a video's frames randomly, you could end up with frame 10 in the training set, and frame 11 in the test set. These 2 frames will look mostly identical.

48:10 `label_from_df`

Tell it which column we want to use as the dependent variable. In this example, we're trying to find out who makes over 50k.

48:55 `get_tabular_learner`

`layers=[200, 100]` this is information about the neural network architecture.



## Questions

40:45 What are the 10% of cases (for tabular data) that Jeremy won't default to using neural nets?

Just try both neural nets and random forests to experiment.

49:14 How to combine NLP tokenized data with tabular data? For example, combine IMDB data with meta data related to each film (who the actors are, what year it's made, etc).

We're not quite up to that yet. You can merge different sets of input to various layers of a neural network. We might get into that in part 2, but might not have enough time to cover it.

50:36 Do you think `scikit-learn` and `XGBoost` will become outdated with everyone switching to neural nets?

No idea.
