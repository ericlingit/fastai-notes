### `ConvLearner` renamed to `create_cnn`
In the video, Jeremy's notebook has a `ConvLearner` class. It's been renamed to `create_cnn`.

---

### Error when trying to display a pandas dataframe
[Temporary fix](https://github.com/jupyter/notebook/issues/4369) (in a cell, run the line below):
```python
get_ipython().config.get('IPKernelApp', {})['parent_appname'] = ""
```

---

### AssertionError: Failed to find "re.compile('/([^/]+)_\\d+.jpg$')" in "/home/eric/.fastai/data/oxford-iiit-pet/images/Abyssinian_101.mat"

Full trace:
```
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-23-fb71d1e9f11f> in <module>
----> 1 data = ImageDataBunch.from_name_re(path=path/'images', fnames=img_files,  pat=label_regex, bs=bs, ds_tfms=get_transforms(), size=224).normalize(imagenet_stats)

/opt/anaconda3/lib/python3.7/site-packages/fastai/vision/data.py in from_name_re(cls, path, fnames, pat, valid_pct, **kwargs)
    155             assert res,f'Failed to find "{pat}" in "{fn}"'
    156             return res.group(1)
--> 157         return cls.from_name_func(path, fnames, _get_label, valid_pct=valid_pct, **kwargs)
    158 
    159     @staticmethod

/opt/anaconda3/lib/python3.7/site-packages/fastai/vision/data.py in from_name_func(cls, path, fnames, label_func, valid_pct, **kwargs)
    144         "Create from list of `fnames` in `path` with `label_func`."
    145         src = ImageList(fnames, path=path).split_by_rand_pct(valid_pct)
--> 146         return cls.create_from_ll(src.label_from_func(label_func), **kwargs)
    147 
    148     @classmethod

/opt/anaconda3/lib/python3.7/site-packages/fastai/data_block.py in _inner(*args, **kwargs)
    461         assert isinstance(fv, Callable)
    462         def _inner(*args, **kwargs):
--> 463             self.train = ft(*args, from_item_lists=True, **kwargs)
    464             assert isinstance(self.train, LabelList)
    465             kwargs['label_cls'] = self.train.y.__class__

/opt/anaconda3/lib/python3.7/site-packages/fastai/data_block.py in label_from_func(self, func, label_cls, **kwargs)
    285     def label_from_func(self, func:Callable, label_cls:Callable=None, **kwargs)->'LabelList':
    286         "Apply `func` to every input to get its label."
--> 287         return self._label_from_list([func(o) for o in self.items], label_cls=label_cls, **kwargs)
    288 
    289     def label_from_folder(self, label_cls:Callable=None, **kwargs)->'LabelList':

/opt/anaconda3/lib/python3.7/site-packages/fastai/data_block.py in <listcomp>(.0)
    285     def label_from_func(self, func:Callable, label_cls:Callable=None, **kwargs)->'LabelList':
    286         "Apply `func` to every input to get its label."
--> 287         return self._label_from_list([func(o) for o in self.items], label_cls=label_cls, **kwargs)
    288 
    289     def label_from_folder(self, label_cls:Callable=None, **kwargs)->'LabelList':

/opt/anaconda3/lib/python3.7/site-packages/fastai/vision/data.py in _get_label(fn)
    153             if isinstance(fn, Path): fn = fn.as_posix()
    154             res = pat.search(str(fn))
--> 155             assert res,f'Failed to find "{pat}" in "{fn}"'
    156             return res.group(1)
    157         return cls.from_name_func(path, fnames, _get_label, valid_pct=valid_pct, **kwargs)

AssertionError: Failed to find "re.compile('/([^/]+)_\\d+.jpg$')" in "/home/eric/.fastai/data/oxford-iiit-pet/images/Abyssinian_100.mat"
```

Cause: Non-image files in path/images.

Explanation: when passing arg `fnames=img_files` to `ImageDataBunch.from_name_re()`, `img_files` contains non-image files. In a previous cell, `img_files` was defined as `img_files = (path/'images').ls()`. This is wrong because it's a list of *all* files in path/images. Whereas we want all *image* files. By using the `ls()` function, we included a few non-image files (there are a few .mat files in path/images. Example: `images/Abyssinian_101.mat`).

Solution: use `get_image_files(path/'images')`. This utility function removes non-image files for you.

Compare the length of `get_image_files(path/'images')` to the length of `(path/'images').ls()`. The former should be shorter than the latter:

```
print(f"get_image_files length     : {len(get_image_files(path/'images'))}")
print(f"(path/'images').ls() length: {len((path/'images').ls())}")

# Output:

get_image_files length     : 7390
(path/'images').ls() length: 7393
```
