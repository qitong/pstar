# [`pstar`](/docs/pstar.md).[`plist`](/docs/pstar_plist.md).`qj(self, *args, **kwargs)`

Applies logging function qj to self for easy in-chain logging.

[`qj`](/docs/pstar_defaultpdict_qj.md) is a debug logging function. Calling `plist.qj()` is often the fastest way
to begin debugging an issue.

See [qj](https://github.com/iansf/qj) for detailed information on using [`qj`](/docs/pstar_defaultpdict_qj.md).

**Examples:**
```python
foos = plist([pdict(foo=0, bar=0), pdict(foo=1, bar=1), pdict(foo=2, bar=0)])
assert (foos.foo.qj('foo').aslist() ==
        [0, 1, 2])
# Logs:
# qj: <calling_module> calling_func: foo <3869>: [0, 1, 2]
```

**Args:**

>    **`*args`**: Arguments to pass to [`qj`](/docs/pstar_defaultpdict_qj.md).

>    **`**kwargs`**: Keyword arguments to pass to [`qj`](/docs/pstar_defaultpdict_qj.md).

**Returns:**

>    `self`


