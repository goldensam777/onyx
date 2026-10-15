# 2. Core syntax

## Blocks and comments

Blocks are introduced by `:` and delimited by indentation. Comments use `(<`
to open and `>)` to close; they can be single-line or multi-line.

```onyx
if active:
    print("Running")

(< This is a comment. >)
```

**To decide:** indentation policy (spaces/tabs, width) and whether empty blocks
are allowed.

## Variable declarations

Variables can be declared using inference or an explicit type annotation.

```onyx
variable_name = data
variable_name: data_type = data
```

Bindings are mutable by default. A later assignment to an existing variable is
permitted:

```onyx
value = 1
value = 2
```

**To decide:** constants, declarations without a value, and the exact point at
which an annotation is checked.
