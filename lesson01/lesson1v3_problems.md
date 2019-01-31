### `ConvLearner` renamed to `create_cnn`
In the video, Jeremy's notebook has a `ConvLearner` class. It's been renamed to `create_cnn`.

---

### Error when trying to display a pandas dataframe
[Temporary fix](https://github.com/jupyter/notebook/issues/4369) (in a cell, run the line below):
```python
get_ipython().config.get('IPKernelApp', {})['parent_appname'] = ""
```
