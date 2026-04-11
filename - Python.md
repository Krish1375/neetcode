
**I want to check if all the values, i.e values corresponding to all keys in a dictionary**


use `all()`:

```python
all(value == 0 for value in your_dict.values())
```

`all` returns `True` if all elements of the given iterable are true.

