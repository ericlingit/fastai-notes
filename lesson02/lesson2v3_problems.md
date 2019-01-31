### `ImageFileList` renamed to `ImageItemList`
When demoing data block API, Jeremy's notebook has this line:
```python
data = (ImageFileList.from_folder([planet]))
```

That class has been renamed to `ImageItemList`.

---

### Error when trying to display a pandas dataframe
```python
df = pd.read_csv(path/'train_v2.csv')
df.head()
```

```
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
~/anaconda3/envs/fastai-v3/lib/python3.7/site-packages/IPython/core/formatters.py in __call__(self, obj)
    343             method = get_real_method(obj, self.print_method)
    344             if method is not None:
--> 345                 return method()
    346             return None
    347         else:

~/anaconda3/envs/fastai-v3/lib/python3.7/site-packages/pandas/core/frame.py in _repr_html_(self)
    647         # display HTML, so this check can be removed when support for
    648         # IPython 2.x is no longer needed.
--> 649         if console.in_qtconsole():
    650             # 'HTML output is disabled in QtConsole'
    651             return None

~/anaconda3/envs/fastai-v3/lib/python3.7/site-packages/pandas/io/formats/console.py in in_qtconsole()
    121             ip.config.get('KernelApp', {}).get('parent_appname', "") or
    122             ip.config.get('IPKernelApp', {}).get('parent_appname', ""))
--> 123         if 'qtconsole' in front_end.lower():
    124             return True
    125     except NameError:

AttributeError: 'LazyConfigValue' object has no attribute 'lower'

  image_name                                       tags
0    train_0                               haze primary
1    train_1            agriculture clear primary water
2    train_2                              clear primary
3    train_3                              clear primary
4    train_4  agriculture clear habitation primary road
```

[Temporary fix](https://github.com/jupyter/notebook/issues/4369) (in a cell, run the line below):
```python
get_ipython().config.get('IPKernelApp', {})['parent_appname'] = ""
```

---
