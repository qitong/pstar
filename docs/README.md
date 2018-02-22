# `pstar`
## `numpy` for arbitrary data.

`pstar` provides easy, expressive, and concise manipulation of arbitrary data.

## Examples:

```bash
$ pip install pstar

$ python  # Give it a spin! (Or use ipython, if installed.)
```

```python
from pstar import *


# pdict basics
pd = pdict(foo=1, bar=2.0)
pd.baz = 'three'

pd.qj('Hello, pdict!')
# Logs:
#   qj: <module_level_code> Hello, pdict! <6>: {'bar': 2.0, 'baz': 'three', 'foo': 1}

pd.update({'bin': 44}).qj('Chaining through update!')
# Logs:
#   qj: <module_level_code> Chaining through update! <10>: {'bar': 2.0, 'baz': 'three', 'bin': 44, 'foo': 1}

pd[['foo', 'bar']].qj('Multi-indexing!')
# Logs:
#   qj: <module_level_code> Multi-indexing! <14>: [1, 2.0]

pd[['foo', 'bar', 'bin']] = ['one', 'ii', '44']
pd.qj('Multi-assignment!')
# Logs:
#   qj: <module_level_code> Multi-assignment! <19>: {'bar': 'ii', 'baz': 'three', 'bin': '44', 'foo': 'one'}

pd[pd.peys()] = 'what?! ' + pd.palues()
pd.qj('Easy manipulation of values!')
# Logs:
#   qj: <module_level_code> Easy manipulation of values! <24>: {'bar': 'what?! ii', 'baz': 'what?! three', 'bin': 'what?! 44', 'foo': 'what?! one'}

pd.rekey(foo='floo').qj('Easy manipulation of keys!')
# Logs:
#   qj: <module_level_code> Easy manipulation of keys! <28>: {'bar': 'what?! ii', 'baz': 'what?! three', 'bin': 'what?! 44', 'floo': 'what?! one'}


# defaultpdict basics
dpd = defaultpdict(int)
dpd.bar = dpd.foo + 1

dpd.qj('Hello, defaultpdict!')
# Logs:
#   qj: module_level_code: Hello, defaultpdict! <39>: {'bar': 1, 'foo': 0}

dpd[['foo', 'bar']].qj('The same api as pdict!')
# Logs:
#   qj: module_level_code:   The same api as pdict! <43>: [0, 1]

dpd = defaultpdict(lambda: defaultpdict(list))
dpd.name = 'Thing 1'
dpd.stats.foo.append(1)
dpd.stats.bar.append(22)

dpd.qj('Nested defaultpdicts make great lightweight objects!')
# Logs:
#   qj: module_level_code: Nested defaultpdicts make great lightweight objects! <6>: {'name': 'Thing 1', 'stats': {'bar': [22], 'foo': [1]}}


# plist basics
pl = plist[1, 2, 3]
pl.qj('Hello, plist!')
# Logs:
#   qj: module_level_code: Hello, plist! <33>: [1, 2, 3]

pl *= pl
pl.qj('plists can mostly be used as if they are a single instance of the type they contain!')
# Logs:
#   qj: module_level_code: plists can mostly be used as if they are a single instance of the type they contain! <8>: [1, 4, 9]

pl = plist([pdict(foo=i, bar=i % 2) for i in range(5)])
pl.baz = pl.foo % 3

pl.qj('That includes plists of pdicts, or other arbitrary objects!')
# Logs:
#   qj: module_level_code: That includes plists of pdicts, or other arbitrary objects! <4>: [{'bar': 0, 'baz': 0, 'foo': 0}, {'bar': 1, 'baz': 1, 'foo': 1}, {'bar': 0, 'baz': 2, 'foo': 2}, {'bar': 1, 'baz': 0, 'foo': 3}, {'bar': 0, 'baz': 1, 'foo': 4}]

(pl.bar == 0).qj('Data are meant to be filtered!')[('foo', 'baz')].pstr().replace(', ', ' {bar} ').qj('And processed!').format(bar=(pl.bar == 0).bar).qj('And merged!')
# Logs:
#   qj: module_level_code: Data are meant to be filtered! <1>: [{'bar': 0, 'baz': 0, 'foo': 0}, {'bar': 0, 'baz': 2, 'foo': 2}, {'bar': 0, 'baz': 1, 'foo': 4}]
#   qj: module_level_code:  And processed! <1>: ['(0 {bar} 0)', '(2 {bar} 2)', '(4 {bar} 1)']
#   qj: module_level_code:   And merged! <1>: ['(0 0 0)', '(2 0 2)', '(4 0 1)']

by_bar = pl.bar.groupby().qj('Grouping data is also powerful!')
# Logs:
#   qj: module_level_code: Grouping data is also powerful! <1>: [[{'bar': 0, 'baz': 0, 'foo': 0}, {'bar': 0, 'baz': 2, 'foo': 2}, {'bar': 0, 'baz': 1, 'foo': 4}], [{'bar': 1, 'baz': 1, 'foo': 1}, {'bar': 1, 'baz': 0, 'foo': 3}]]

by_bar[('foo', 'baz')].pstr().replace(', ', ' {bar} ').format(bar=by_bar.bar).qj('Now we can get same result for all of the data!')
# Logs:
#   qj: module_level_code: Now we can get same result for all of the data! <1>: [['(0 0 0)', '(2 0 2)', '(4 0 1)'], ['(1 1 1)', '(3 1 0)']]

by_bar.qj('Note that the original data are unchanged!')
# Logs:
#   qj: module_level_code: Note that the original data are unchanged! <1>: [[{'bar': 0, 'baz': 0, 'foo': 0}, {'bar': 0, 'baz': 2, 'foo': 2}, {'bar': 0, 'baz': 1, 'foo': 4}], [{'bar': 1, 'baz': 1, 'foo': 1}, {'bar': 1, 'baz': 0, 'foo': 3}]]

pl.foo.apply(lambda floo, blar, blaz: floo * (blar + blaz), pl.bar, blaz=pl.baz).qj('Want to send your data element-wise to a function? Easy!')
# Logs:
#   qj: module_level_code: Want to send your data element-wise to a function? Easy! <1>: [0, 2, 4, 3, 4]

by_bar.foo.apply(lambda floo, blar, blaz: floo * (blar + blaz), by_bar.bar, blaz=by_bar.baz).qj('The same function just works when using groups!')
# Logs:
#   qj: module_level_code: The same function just works when using groups! <3>: [[0, 4, 4], [2, 3]]

new_pd = pd.qj('Remember pdicts?').palues().qj('Some of their functions return plists').replace('what?! ', 'sweet! ').qj('allowing you to update their values naturally').pdict().qj('and turn it back into a new pdict!')
# Logs:
#   qj: module_level_code: Remember pdicts? <101>: {'bar': 'what?! ii', 'baz': 'what?! three', 'bin': 'what?! 44', 'foo': 'what?! one'}
#   qj: module_level_code:  Some of their functions return plists <101>: ['what?! ii', 'what?! three', 'what?! 44', 'what?! one']
#   qj: module_level_code:   allowing you to update their values naturally <101>: ['sweet! ii', 'sweet! three', 'sweet! 44', 'sweet! one']
#   qj: module_level_code:    and turn it back into a new pdict! <101>: {'bar': 'sweet! ii', 'baz': 'sweet! three', 'bin': 'sweet! 44', 'foo': 'sweet! one'}

# A hint at how easy it can be to do complex data manipulations...
whoa = plist[pd, new_pd]
whoa.palues().replace(whoa.palues()._[:6:1], whoa.palues()._[7::1]).pdict_().qj('whoa!')
# Logs:
#   qj: module_level_code: whoa! <2>: [{'bar': 'ii ii', 'baz': 'three three', 'bin': '44 44', 'foo': 'one one'}, {'bar': 'ii ii', 'baz': 'three three', 'bin': '44 44', 'foo': 'one one'}]
```

See [build_docs.py](../build_docs.py) for a more extensive example of using `pstar`.


## Philosophy:

`pstar` makes writing and debugging data-processing code easy and concise.

### `pdict` and `defaultpdict`:

`pdict` and `defaultpdict` are drop-in replacements for `dict` and `defaultdict`, but
provide substantial usability improvements, including dot notation for field access,
chaining from calls to `update`, and easy methods to modify their keys and values.

### `plist`:

`plist` is close to a drop-in replacement for `list`. It is also close to a drop-in
replacement for whatever values it contains. This is the core trick of `plist`:
write your data processing code like you are working with one datum. The closer your
code gets to that ideal, the easier it is to write, debug, and understand.

### Chaining:

`pstar` attempts to always maintain the possibility of chaining. Chaining allows you
to write code like a sentence, without needing to break up your thoughts to define
intermediate variables or to introduce obvious control flow, such as `for` loops.

The consequence of this perspective is that code written using `pstar` can often
be written with no explicit looping, and each line of code can be read as a
straightforward transformation of data from one relevant state to another.

### Debugging:

During data processing, it is easy to spend a great deal of time debugging while
getting the data into the desired shape or format. Most debugging starts with
log statements. `pstar` incorporates in-chain logging with `qj` so that code can
be minimally modified to add and remove logging.

`qj` is a logger built for debugging, and has many useful features that are
available directly in `pstar`, including dropping into the debugger at any
point in your code:
```python
pl = plist['abc', 'def', '123']
pl = pl.qj().replace(pl._[0].qj(d=1), pl._[-1].qj()).qj()
# Logs:
#   qj: module_level_code: <empty log> <2>: ['abc', 'def', '123']
#   qj: module_level_code:  <empty log> <2>: ['a', 'd', '1']
# Then drops into the debugger. After debugging completes, logs:
#   qj: module_level_code:   <empty log> <2>: ['c', 'f', '3']
#   qj: module_level_code:    <empty log> <2>: ['cbc', 'fef', '323']
```

See [`qj`](https://github.com/iansf/qj) for documentation.

### Testing and Examples:

`pstar` is extensively tested. Additionally, almost all of the example code
found in the documentation is automatically added to the test suite when
documentation is built. Therefore, every block of code in a page of documentation
is a self-contained, runnable example that you can copy into a
python terminal and run immediately.

Note that because many of the tests check the contents of `plist`s, and
`plist`s do filtering when compared, many of the tests look like:
`assert (foos.aslist() == [...])` in order to bypass the filtering and just run
an equality check on two lists. Under normal use, you do not need to call
`plist.aslist()` very often.

### Concision:

In the very simple example below, `pstar` does in six lines with no explicit
control flow, what takes 10 lines and three levels of indentation in regular
python. The extra lines are from the explicit control flow and the inability
to chain the output to a print statement.
```python
# Trivial pstar data processing:
pl = plist([pdict(foo=i, bar=i % 2) for i in range(5)])
pl.baz = (pl.foo + pl.bar) % (len(pl) // 2 + 1)
by_bar = pl.bar.groupby()
(by_bar.bar == 0).bin = '{floo} {blaz} {other}'
(by_bar.bar != 0).bin = '{floo} {blar} {blaz}'
output = by_bar.bin.format(floo=by_bar.foo, blar=by_bar.bar, blaz=by_bar.baz, other=(by_bar.baz + by_bar.foo) * by_bar.bar).qj('output')

# Non-pstar equivalent:
l = [dict(foo=i, bar=i % 2) for i in range(5)]
output = [[], []]
for d in l:
  d['baz'] = (d['foo'] + d['bar']) % (len(l) // 2 + 1)
  if d['bar'] == 0:
    d['bin'] = '{floo} {blaz} {other}'
  else:
    d['bin'] = '{floo} {blar} {blaz}'
  output[d['bar']].append(d['bin'].format(floo=d['foo'], blar=d['bar'], blaz=d['baz'], other=(d['baz'] + d['foo']) * d['bar']))
print('output: ', output)
```

Worse than the extra length and complexity, the non-`plist`
code has a bug: if the values for `bar` are ever something other than 0 or 1,
the output list will fail. The `pstar` version of the code is completely robust
to that kind of bug. The only assumptions about the data are that it is provided
with two fields, 'foo' and 'bar', and that both of the fields are numeric.

See [build_docs.py](../build_docs.py) for a more extensive example of using `pstar`.


## Basic Usage:

### Install with pip:
```bash
$ pip install pstar
```

### Add the following import:
```python
from pstar import *
```

### Equivalently:
```python
from pstar import defaultpdict, pdict, plist, pset
```

### Basic [`defaultpdict`](./pstar_defaultpdict.md) use:

`defaultdict` subclass where everything is automatically a property.

**Examples:**

Use with dot notation or subscript notation:
```python
  p = defaultpdict()
  p.foo = 1
  assert (p['foo'] == p.foo == 1)
```

Set the desired default constructor as normal to avoid having to construct
individual values:
```python
  p = defaultpdict(int)
  assert (p.foo == 0)
```

`list` subscripts also work and return a [`plist`](./pstar_plist.md) of the corresponding keys:
```python
  p = defaultpdict(foo=1, bar=2)
  assert (p[['foo', 'bar']].aslist() == [1, 2])
```

Setting with a `list` subscript also works, using a single element or a matching
`list` for the values:
```python
  p = defaultpdict()
  p[['foo', 'bar']] = 1
  assert (p[['foo', 'bar']].aslist() == [1, 1])
  p[['foo', 'bar']] = [1, 2]
  assert (p[['foo', 'bar']].aslist() == [1, 2])
```

`defaultpdict.update()` returns `self`, rather than `None`, to support chaining:
```python
  p = defaultpdict(foo=1, bar=2)
  p.update(bar=3).baz = 4
  assert (p.bar == 3)
  assert ('baz' in p.keys())
```

Nested `defaultpdict`s make nice lightweight objects:
```python
  p = defaultpdict(lambda: defaultpdict(list))
  p.foo = 1
  p.stats.bar.append(2)
  assert (p['foo'] == 1)
  assert (p.stats.bar == [2])
```

### Basic [`pdict`](./pstar_pdict.md) use:

`dict` subclass where everything is automatically a property.

**Examples:**

Use with dot notation or subscript notation:
```python
  p = pdict()
  p.foo = 1
  assert (p['foo'] == p.foo == 1)
```

`list` subscripts also work and return a [`plist`](./pstar_plist.md) of the corresponding keys:
```python
  p = pdict(foo=1, bar=2)
  assert (p[['foo', 'bar']].aslist() == [1, 2])
```

Setting with a `list` subscript also works, using a single element or a matching
`list` for the values:
```python
  p = pdict()
  p[['foo', 'bar']] = 1
  assert (p[['foo', 'bar']].aslist() == [1, 1])
  p[['foo', 'bar']] = [1, 2]
  assert (p[['foo', 'bar']].aslist() == [1, 2])
```

`pdict.update()` returns `self`, rather than `None`, to support chaining:
```python
  p = pdict(foo=1, bar=2)
  p.update(bar=3).baz = 4
  assert (p.bar == 3)
  assert ('baz' in p.keys())
```

### Basic [`plist`](./pstar_plist.md) use:

`list` subclass for powerful, concise data processing.

**Homogeneous access:**

`plist` is the natural extension of object-orientation to homogeneous lists of
arbitrary objects. With `plist`, you can treat a list of objects of the same
type as if they are a single object of that type, in many (but not all)
circumstances.

```python
pl = plist['abc', 'def', 'ghi']
assert ((pl + ' -> ' + pl.upper()).aslist() ==
        ['abc -> ABC', 'def -> DEF', 'ghi -> GHI'])
```

**Indexing:**

Indexing `plist`s is meant to be both powerful and natural, while accounting
the fact that the elements of the `plist` may need to be indexed as well.

See `plist.__getitem__`, `plist.__setitem__`, and `plist.__delitem__` for
more details.

Indexing into the `plist` itself:
```python
foos = plist([pdict(foo=0, bar=0), pdict(foo=1, bar=1), pdict(foo=2, bar=0)])

# Basic scalar indexing:
assert (foos[0] ==
        dict(foo=0, bar=0))

# plist slice indexing:
assert (foos[:2].aslist() ==
        [dict(foo=0, bar=0), dict(foo=1, bar=1)])

# plist int list indexing:
assert (foos[[0, 2]].aslist() ==
        [dict(foo=0, bar=0), dict(foo=2, bar=0)])
```

Indexing into the elements of the `plist`:
```python
# Basic scalar indexing:
assert (foos['foo'].aslist() ==
        [0, 1, 2])

# tuple indexing
assert (foos[('foo', 'bar')].aslist() ==
        [(0, 0), (1, 1), (2, 0)])

# list indexing
assert (foos[['foo', 'bar', 'bar']].aslist() ==
        [0, 1, 0])
```

Indexing into the elementes of the `plist` when the elements are indexed by
`int`s, `slice`s, or other means that confict with `plist` indexing:
```python
pl = plist[[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Basic scalar indexing:
assert (pl._[0].aslist() ==
        [1, 4, 7])

# slice indexing (note the use of the 3-argument version of slicing):
assert (pl._[:2:1].aslist() ==
        [[1, 2], [4, 5], [7, 8]])

# list indexing:
pl = pl.np()
assert (pl._[[True, False, True]].apply(list).aslist() ==
        [[1, 3], [4, 6], [7, 9]])
```

**[`root`](./pstar_plist_root.md) and [`uproot`](./pstar_plist_uproot.md):**

`plist`s all have a root object. For newly created `plist`s, the root is `self`,
but as computations are performed on the `plist`, the root of the resulting
`plist`s almost always remain the original `plist`:
```python
pl = plist[1, 2, 3]
# `plist` operations don't modify the original (except where natural)!
assert ((pl + 5) is not pl)
assert ((pl + 5).root() is pl)
```

In some cases, you don't want to maintain the original root. To reset the root
to `self`, simply call [`uproot`](./pstar_plist_uproot.md):
```python
pl2 = pl + 5
assert (pl2.root() is not pl2)
assert (pl2.uproot().root() is pl2)
assert (pl2.root() is pl2)
```

See [`root`](./pstar_plist_root.md) and [`uproot`](./pstar_plist_uproot.md) for more details.

**Filtering:**

`plist` overrides comparison operations to provide filtering. This is reasonable,
since an empty `plist` is a `False` value, just like an empty `list`, so a filter
that filters everything is equivalent to the comparison failing.

Filtering always returns the root of the `plist`, which allows you to filter a
`plist` on arbitrary values computed from the root, and then proceed with your
computation on the (filtered) original data.

See [`comparator`](./pstar_plist_comparator.md) and [`filter`](./pstar_plist_filter.md) for more details.

```python
foos = plist([pdict(foo=0, bar=0), pdict(foo=1, bar=1), pdict(foo=2, bar=0)])
# Filtering on a property:
zero_bars = foos.bar == 0
# The result is a `plist` of the original [`pdict`](./pstar_pdict.md)s, correctly filtered:
assert (zero_bars.aslist() ==
        [{'foo': 0, 'bar': 0},
         {'foo': 2, 'bar': 0}])

# filter can take any function to filter by, but it defaults to bool():
nonzero_bars = foos.bar.filter()
assert (nonzero_bars.aslist() ==
        [{'foo': 1, 'bar': 1}])
```

**Grouping and Sorting:**

Just as with filtering, you can group and sort a `plist` on any arbitrary
value computed from the `plist`.

This shows a basic grouping by a property of the data. Note that [`groupby`](./pstar_plist_groupby.md)
returns the root, just like filtering:
```python
foos = plist([pdict(foo=0, bar=1), pdict(foo=1, bar=0), pdict(foo=2, bar=1)])
# Note that the `bar == 1` group comes before the `bar == 0` group. The ordering
# is determined by the sort order of the `plist`.
assert (foos.bar.groupby().aslist() ==
        [[{'bar': 1, 'foo': 0}, {'bar': 1, 'foo': 2}], [{'bar': 0, 'foo': 1}]])
# Note that foos is unchanged:
assert (foos.aslist() ==
        [{'bar': 1, 'foo': 0}, {'bar': 0, 'foo': 1}, {'bar': 1, 'foo': 2}])
```

In contrast, sorting a `plist` modifies the order of both the current `plist` and
its root, but returns the current `plist` instead of the root:
```python
assert (foos.bar.sortby().aslist() ==
        [0, 1, 1])
assert (foos.aslist() ==
        [{'bar': 0, 'foo': 1}, {'bar': 1, 'foo': 0}, {'bar': 1, 'foo': 2}])
```

This distinction between the behavios of [`groupby`](./pstar_plist_groupby.md) and [`sortby`](./pstar_plist_sortby.md) permits natural
chaining of the two when sorted groups are desired. It also ensures that
`plist`s computed from the same root will be ordered in the same way.
```python
foos = plist([pdict(foo=0, bar=1), pdict(foo=1, bar=0), pdict(foo=2, bar=1)])
assert (foos.bar.sortby().groupby().aslist() ==
        [[{'bar': 0, 'foo': 1}], [{'bar': 1, 'foo': 0}, {'bar': 1, 'foo': 2}]])
```

See [`groupby`](./pstar_plist_groupby.md) and [`sortby`](./pstar_plist_sortby.md) for more details.

**Function Application and Multiple Arguments:**

The most prominent case where you can't treat a `plist` as a single object is
when you need to pass a single object to some function that isn't a propert of
the elements of the `plist`. In this case, just use [`apply`](./pstar_plist_apply.md):
```python
pl = plist['abc', 'def', 'ghi']
assert (pl.apply('foo: {}'.format).aslist() ==
        ['foo: abc', 'foo: def', 'foo: ghi'])
```

Where [`apply`](./pstar_plist_apply.md) shines (and all calls to `plist` element functions) is when dealing
with multi-argument functions. In this case, you will often find that you want to
call the function with parallel values from parallel `plist`s. That is easy and
natural to do, just like calling the function with corresponding non-`plist`
values:
```python
foos = plist([pdict(foo=0, bar=0), pdict(foo=1, bar=1), pdict(foo=2, bar=0)])
foos.baz = 'abc' * foos.foo
# Do a multi-argument string format with plist.apply:
assert (foos.foo.apply('foo: {} bar: {} baz: {baz}'.format, foos.bar, baz=foos.baz).aslist() ==
        ['foo: 0 bar: 0 baz: ', 'foo: 1 bar: 1 baz: abc', 'foo: 2 bar: 0 baz: abcabc'])
# Do the same string format directly using the plist as the format string:
assert (('foo: ' + foos.foo.pstr() + ' bar: {} baz: {baz}').format(foos.bar, baz=foos.baz).aslist() ==
        ['foo: 0 bar: 0 baz: ', 'foo: 1 bar: 1 baz: abc', 'foo: 2 bar: 0 baz: abcabc'])
```

See [`__call__`](./pstar_plist___call__.md), [`apply`](./pstar_plist_apply.md), and [`reduce`](./pstar_plist_reduce.md) for more details.

### Basic [`pset`](./pstar_pset.md) use:

Placeholder frozenset subclass. Not yet implemented.


## API Overview:

Links for detailed documentation are below.

____

### [`pstar.defaultpdict(defaultdict)`](./pstar_defaultpdict.md)

`defaultdict` subclass where everything is automatically a property.

#### [`pstar.defaultpdict.__getattr__(self, name)`](./pstar_defaultpdict___getattr__.md)

Override `getattr`. If `name` starts with '_', attempts to find that attribute on `self`. Otherwise, looks for a field of that name in `self`.

#### [`pstar.defaultpdict.__getitem__(self, key)`](./pstar_defaultpdict___getitem__.md)

Subscript operation. Keys can be any normal `dict` keys or `list`s of such keys.

#### [`pstar.defaultpdict.__init__(self, *a, **kw)`](./pstar_defaultpdict___init__.md)

Initialize [`defaultpdict`](./pstar_defaultpdict.md).

#### [`pstar.defaultpdict.__setattr__(self, name, value)`](./pstar_defaultpdict___setattr__.md)

Attribute assignment operation. Forwards to subscript assignment.

#### [`pstar.defaultpdict.__setitem__(self, key, value)`](./pstar_defaultpdict___setitem__.md)

Subscript assignment operation. Keys and values can be scalars or `list`s.

#### [`pstar.defaultpdict.__str__(self)`](./pstar_defaultpdict___str__.md)

Readable string representation of `self`.

#### [`pstar.defaultpdict.copy(self)`](./pstar_defaultpdict_copy.md)

Copy `self` to new [`defaultpdict`](./pstar_defaultpdict.md). Performs a shallow copy.

#### [`pstar.defaultpdict.palues(self)`](./pstar_defaultpdict_palues.md)

Equivalent to `self.values()`, but returns a [`plist`](./pstar_plist.md) with values sorted as in `self.peys()`.

#### [`pstar.defaultpdict.peys(self)`](./pstar_defaultpdict_peys.md)

Get `self.keys()` as a sorted [`plist`](./pstar_plist.md).

#### [`pstar.defaultpdict.pitems(self)`](./pstar_defaultpdict_pitems.md)

Equivalent to `self.items()`, but returns a [`plist`](./pstar_plist.md) with items sorted as in `self.peys()`.

#### [`pstar.defaultpdict.qj(self, *a, **kw)`](./pstar_defaultpdict_qj.md)

Call the [`qj`](./pstar_pdict_qj.md) logging function with `self` as the value to be logged. All other arguments are passed through to [`qj`](./pstar_pdict_qj.md).

#### [`pstar.defaultpdict.rekey(self, map_or_fn=None, inplace=False, **kw)`](./pstar_defaultpdict_rekey.md)

Change the keys of `self` or a copy while keeping the same values.

#### [`pstar.defaultpdict.update(self, *a, **kw)`](./pstar_defaultpdict_update.md)

Update `self`. **Returns `self` to allow chaining.**

____

### [`pstar.pdict(dict)`](./pstar_pdict.md)

`dict` subclass where everything is automatically a property.

#### [`pstar.pdict.__getitem__(self, key)`](./pstar_pdict___getitem__.md)

Subscript operation. Keys can be any normal `dict` keys or `list`s of such keys.

#### [`pstar.pdict.__init__(self, *a, **kw)`](./pstar_pdict___init__.md)

Initialize [`pdict`](./pstar_pdict.md).

#### [`pstar.pdict.__setitem__(self, key, value)`](./pstar_pdict___setitem__.md)

Subscript assignment operation. Keys and values can be scalars or `list`s.

#### [`pstar.pdict.__str__(self)`](./pstar_pdict___str__.md)

Readable string representation of `self`.

#### [`pstar.pdict.copy(self)`](./pstar_pdict_copy.md)

Copy `self` to new [`defaultpdict`](./pstar_defaultpdict.md). Performs a shallow copy.

#### [`pstar.pdict.palues(self)`](./pstar_pdict_palues.md)

Equivalent to `self.values()`, but returns a [`plist`](./pstar_plist.md) with values sorted as in `self.peys()`.

#### [`pstar.pdict.peys(self)`](./pstar_pdict_peys.md)

Get `self.keys()` as a sorted [`plist`](./pstar_plist.md).

#### [`pstar.pdict.pitems(self)`](./pstar_pdict_pitems.md)

Equivalent to `self.items()`, but returns a [`plist`](./pstar_plist.md) with items sorted as in `self.peys()`.

#### [`pstar.pdict.qj(self, *a, **kw)`](./pstar_pdict_qj.md)

Call the [`qj`](./pstar_defaultpdict_qj.md) logging function with `self` as the value to be logged. All other arguments are passed through to [`qj`](./pstar_defaultpdict_qj.md).

#### [`pstar.pdict.rekey(self, map_or_fn=None, inplace=False, **kw)`](./pstar_pdict_rekey.md)

Change the keys of `self` or a copy while keeping the same values.

#### [`pstar.pdict.update(self, *a, **kw)`](./pstar_pdict_update.md)

Update `self`. **Returns `self` to allow chaining.**

____

### [`pstar.plist(list)`](./pstar_plist.md)

`list` subclass for powerful, concise data processing.

#### [`pstar.plist._(self)`](./pstar_plist__.md)

Causes the next call to `self` to be performed as deep as possible in the [`plist`](./pstar_plist.md).

#### [`pstar.plist.__call__(self, *args, **kwargs)`](./pstar_plist___call__.md)

Call each element of self, possibly recusively.

#### [`pstar.plist.__contains__(self, other)`](./pstar_plist___contains__.md)

Implements the `in` operator to avoid inappropriate use of [`plist`](./pstar_plist.md) comparators.

#### [`pstar.plist.__delattr__(self, name)`](./pstar_plist___delattr__.md)

Deletes an attribute on elements of `self`.

#### [`pstar.plist.__delitem__(self, key)`](./pstar_plist___delitem__.md)

Deletes items of `self` using a variety of indexing styles.

#### [`pstar.plist.__delslice__(self, i, j)`](./pstar_plist___delslice__.md)

Delegates to [`__delitem__`](./pstar_plist___delitem__.md) whenever possible. For compatibility with python 2.7.

#### [`pstar.plist.__enter__(self)`](./pstar_plist___enter__.md)

Allow the use of plists in `with` statements.

#### [`pstar.plist.__exit__(self, exc_type, exc_value, traceback)`](./pstar_plist___exit__.md)

Allow the use of plists in `with` statements.

#### [`pstar.plist.__getattr__(self, name, _pepth=0)`](./pstar_plist___getattr__.md)

Recursively attempt to get the attribute `name`.

#### [`pstar.plist.__getattribute__(self, name)`](./pstar_plist___getattribute__.md)

Returns a plist of the attribute for self, or for each element.

#### [`pstar.plist.__getitem__(self, key)`](./pstar_plist___getitem__.md)

Returns a new [`plist`](./pstar_plist.md) using a variety of indexing styles.

#### [`pstar.plist.__getslice__(self, i, j)`](./pstar_plist___getslice__.md)

Delegates to [`__getitem__`](./pstar_defaultpdict___getitem__.md) whenever possible. For compatibility with python 2.7.

#### [`pstar.plist.__init__(self, *args, **kwargs)`](./pstar_plist___init__.md)

Constructs plist.

#### [`pstar.plist.__setattr__(self, name, val)`](./pstar_plist___setattr__.md)

Sets an attribute on a [`plist`](./pstar_plist.md) or its elements to `val`.

#### [`pstar.plist.__setitem__(self, key, val)`](./pstar_plist___setitem__.md)

Sets items of `self` using a variety of indexing styles.

#### [`pstar.plist.__setslice__(self, i, j, sequence)`](./pstar_plist___setslice__.md)

Delegates to [`__setitem__`](./pstar_defaultpdict___setitem__.md) whenever possible. For compatibility with python 2.7.

#### [`pstar.plist.all(self, *args, **kwargs)`](./pstar_plist_all.md)

Returns `self` if `args[0]` evaluates to `True` for all elements of `self`.

#### [`pstar.plist.any(self, *args, **kwargs)`](./pstar_plist_any.md)

Returns `self` if `args[0]` evaluates to `True` for any elements of `self`.

#### [`pstar.plist.apply(self, func, *args, **kwargs)`](./pstar_plist_apply.md)

Apply an arbitrary function to elements of self, forwarding arguments.

#### [`pstar.plist.aslist(self)`](./pstar_plist_aslist.md)

Recursively convert all nested [`plist`](./pstar_plist.md)s from `self` to `list`s, inclusive.

#### [`pstar.plist.aspdict(self)`](./pstar_plist_aspdict.md)

Convert `self` to a [`pdict`](./pstar_pdict.md) if there is a natural mapping of keys to values in `self`.

#### [`pstar.plist.aspset(self)`](./pstar_plist_aspset.md)

Recursively convert all nested [`plist`](./pstar_plist.md)s from `self` to [`pset`](./pstar_plist_pset.md)s, inclusive.

#### [`pstar.plist.astuple(self)`](./pstar_plist_astuple.md)

Recursively convert all nested [`plist`](./pstar_plist.md)s from `self` to `tuple`s, inclusive.

#### [`pstar.plist.binary_op(self, other)`](./pstar_plist_binary_op.md)

[`plist`](./pstar_plist.md) binary operation; applied element-wise to `self`.

#### [`pstar.plist.comparator(self, other, return_inds=False)`](./pstar_plist_comparator.md)

[`plist`](./pstar_plist.md) comparison operator. **Comparisons filter plists.**

#### [`pstar.plist.copy(self)`](./pstar_plist_copy.md)

Copy `self` to new [`plist`](./pstar_plist.md). Performs a shallow copy.

#### [`pstar.plist.enum(self)`](./pstar_plist_enum.md)

Wrap the current [`plist`](./pstar_plist.md) values in tuples where the first item is the index.

#### [`pstar.plist.filter(self, func=<type 'bool'>, *args, **kwargs)`](./pstar_plist_filter.md)

Filter `self` by an arbitrary function on elements of `self`, forwarding arguments.

#### [`pstar.plist.groupby(self)`](./pstar_plist_groupby.md)

Group `self.root()` by the values in `self` and return `self.root()`.

#### [`pstar.plist.lfill(self, v=0, s=None)`](./pstar_plist_lfill.md)

Returns a **`list`** with the structure of `self` filled in order from `v`.

#### [`pstar.plist.logical_op(self, other)`](./pstar_plist_logical_op.md)

[`plist`](./pstar_plist.md) logical operation. **Logical operations perform set operations on [`plist`](./pstar_plist.md)s.**

#### [`pstar.plist.me(self, name_or_plist='me', call_pepth=0)`](./pstar_plist_me.md)

Sets the current plist as a variable available in the caller's context.

#### [`pstar.plist.none(self, *args, **kwargs)`](./pstar_plist_none.md)

Returns `self` if `args[0]` evaluates to `False` for all elements of `self`.

#### [`pstar.plist.nonempty(self, r=0)`](./pstar_plist_nonempty.md)

Returns a new [`plist`](./pstar_plist.md) with empty sublists removed.

#### [`pstar.plist.np(self, *args, **kwargs)`](./pstar_plist_np.md)

Converts the elements of `self` to `numpy.array`s, forwarding passed args.

#### [`pstar.plist.pand(self, name='__plist_and_var__', call_pepth=0)`](./pstar_plist_pand.md)

Stores `self` into a [`plist`](./pstar_plist.md) of `tuple`s that gets extended with each call.

#### [`pstar.plist.pd(self, *args, **kwargs)`](./pstar_plist_pd.md)

Converts `self` into a `pandas.DataFrame`, forwarding passed args.

#### [`pstar.plist.pdepth(self, s=False)`](./pstar_plist_pdepth.md)

Returns a [`plist`](./pstar_plist.md) of the recursive depth of each leaf element, from 0.

#### [`pstar.plist.pdict(self, *args, **kwargs)`](./pstar_plist_pdict.md)

Convert `self` to a [`pdict`](./pstar_pdict.md) if there is a natural mapping of keys to values in `self`.

#### [`pstar.plist.pequal(self, other)`](./pstar_plist_pequal.md)

Shortcutting recursive equality function.

#### [`pstar.plist.pfill(self, v=0, s=None)`](./pstar_plist_pfill.md)

Returns a [`plist`](./pstar_plist.md) with the structure of `self` filled in order from `v`.

#### [`pstar.plist.pleft(self)`](./pstar_plist_pleft.md)

Returns a [`plist`](./pstar_plist.md) with the structure of `self` filled `plen(-1)` to 0.

#### [`pstar.plist.plen(self, r=0, s=False)`](./pstar_plist_plen.md)

Returns a [`plist`](./pstar_plist.md) of the length of a recursively-selected layer of `self`.

#### [`pstar.plist.plt(self, **kwargs)`](./pstar_plist_plt.md)

Convenience method for managing `matplotlib.pyplot` state within a [`plist`](./pstar_plist.md) chain.

#### [`pstar.plist.pset(self)`](./pstar_plist_pset.md)

Converts the elements of self into pset objects.

#### [`pstar.plist.pshape(self)`](./pstar_plist_pshape.md)

Returns a [`plist`](./pstar_plist.md) of the same structure as `self`, filled with leaf lengths.

#### [`pstar.plist.pstr(self)`](./pstar_plist_pstr.md)

Returns a plist with leaf elements converted to strings.

#### [`pstar.plist.pstructure(self)`](./pstar_plist_pstructure.md)

Returns a `list` of the number of elements in each layer of `self`.

#### [`pstar.plist.puniq(self)`](./pstar_plist_puniq.md)

Returns a new [`plist`](./pstar_plist.md) with only a single element of each value in `self`.

#### [`pstar.plist.qj(self, *args, **kwargs)`](./pstar_plist_qj.md)

Applies logging function qj to self for easy in-chain logging.

#### [`pstar.plist.reduce(self, func, *args, **kwargs)`](./pstar_plist_reduce.md)

Apply a function repeatedly to its own result, returning a plist of length at most 1.

#### [`pstar.plist.remix(self, *args, **kwargs)`](./pstar_plist_remix.md)

Returns a new [`plist`](./pstar_plist.md) of `pdicts` based on selected data from `self`.

#### [`pstar.plist.root(self)`](./pstar_plist_root.md)

Returns the root of the [`plist`](./pstar_plist.md).

#### [`pstar.plist.sortby(self, key=None, reverse=False)`](./pstar_plist_sortby.md)

Sorts `self` and `self.root()` in-place and returns `self`.

#### [`pstar.plist.unary_op(self)`](./pstar_plist_unary_op.md)

[`plist`](./pstar_plist.md) unary operation; applied element-wise to `self`.

#### [`pstar.plist.ungroup(self, r=1, s=None)`](./pstar_plist_ungroup.md)

Inverts the last grouping operation applied and returns a new plist.

#### [`pstar.plist.uproot(self)`](./pstar_plist_uproot.md)

Sets the root to `self` so future `root()` calls return this [`plist`](./pstar_plist.md).

#### [`pstar.plist.values_like(self, value=0)`](./pstar_plist_values_like.md)

Returns a [`plist`](./pstar_plist.md) with the structure of `self` filled with `value`.

#### [`pstar.plist.wrap(self)`](./pstar_plist_wrap.md)

Adds and returns an outer [`plist`](./pstar_plist.md) around `self`.

#### [`pstar.plist.zip(self, *others)`](./pstar_plist_zip.md)

Zips `self` with `others`, recursively.

____

### [`pstar.pset(frozenset)`](./pstar_pset.md)

Placeholder frozenset subclass. Not yet implemented.



## Testing:

pstar has extensive tests. You can run them with nosetests:
```bash
$ nosetests
........................................................................................
----------------------------------------------------------------------
Ran 88 tests in 1.341s

OK
```

Or you can run them directly:
```bash
$ python pstar/tests/pstar_test.py
........................................................................................
----------------------------------------------------------------------
Ran 88 tests in 1.037s

OK
```


## Disclaimer:

This project is not an official Google project. It is not supported by Google
and Google specifically disclaims all warranties as to its quality,
merchantability, or fitness for a particular purpose.


## Contributing:

See how to [contribute](./CONTRIBUTING.md).


## License:

[Apache 2.0](./LICENSE).