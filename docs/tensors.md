# 4. The `arr` type — Multidimensional tensors

`arr` is not a flat list: it is a true multidimensional tensor, meaning
each of its axes is independent from the others (as opposed to matrices
that would be concatenated/assembled into blocks).

```onyx
arr[ElementType] <d1, ..., dN>
```

- `ElementType` (inside `[...]`) is the type stored in every cell of the
  tensor (homogeneous: every cell has the same type).
- `<d1, ..., dN>` (inside `<...>`) is the shape: the size of each axis.
- The rank (number of axes) is never written separately -- it is simply
  the number of sizes given between `<...>`. Writing 3 sizes always
  means a rank-3 tensor, automatically; there is no redundant "rank"
  parameter to keep in sync.

```onyx
(< Basic declaration, for memory allocation for example.
   <2, 3, 6> means 2 shells, 3 lines, 6 columns. >)
var_array: arr[r128] <2, 3, 6>

(< Declaration with an initial value: the literal is a flat list, given
   in order, matching the total element count (1x1x10 = 10 values here). >)
var_array: arr[int32] <1, 1, 10> = [1, 2, 3, 4, 5, 6, 7, 53, 19, 10]
```

Note: `arr` is a tensor abstraction with independent axes. It is separate from
hypercomplex-number systems such as quaternions and, potentially in the future,
Clifford algebras.

## Indexing, shape, and mutation

An `arr` is indexed with one integer coordinate per axis. The number of indices
must equal the tensor rank:

```onyx
values[index]                    (< arr[T] <n> >)
values[layer, line, index]       (< arr[T] <d1, d2, d3> >)
```

An index outside the bounds of its axis is an error. Supplying too few or too
many indices is also an error. An indexed cell can be read or written when the
assigned value is compatible with the array element type.

```onyx
values[index] = value
```

Every array exposes `.shape`, which represents its full shape. A rank-one array
also exposes `.length`, which is its sole dimension:

```onyx
numbers: arr[int] <5> = [64, 25, 12, 22, 11]
size: int = numbers.length
dimensions = numbers.shape
```

**To decide:** the type and formatting of `.shape`, negative indices, slices,
and views.
