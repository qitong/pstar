# [`pstar`](/docs/pstar.md).[`defaultpdict`](/docs/pstar_defaultpdict.md).`__setattr__(self, name, value)`

Attribute assignment operation. Forwards to subscript assignment.

Permits [`pdict`](/docs/pstar_pdict.md)-style field assignment.

**Examples:**
```python
pd = defaultpdict(int).update(foo=1, bar=2.0, baz='three')
pd.floo = 4.0
assert (pd.floo == pd['floo'] == 4.0)
```

**Args:**

>    **`name`**: Any `hash`able value or list of `hash`able values, as in `defaultpdict.__setitem__`,
>          but generally just a valid identifier string provided by the compiler.

>    **`value`**: Any value, or [`plist`](/docs/pstar_plist.md) of values of the same length as the corresponding list in
>           `name`.

**Returns:**

>    `self` to allow chaining through direct calls to `defaultpdict.__setattr__`.


