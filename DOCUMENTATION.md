# onyx

```ascii
                  ██████  ████████   █████ ████ █████ █████
                 ███░░███░░███░░███ ░░███ ░███ ░░███ ░░███ 
                ░███ ░███ ░███ ░███  ░███ ░███  ░░░█████░  
                ░███ ░███ ░███ ░███  ░███ ░███   ███░░░███ 
                ░░██████  ████ █████ ░░███████  █████ █████
                 ░░░░░░  ░░░░ ░░░░░   ░░░░░███ ░░░░░ ░░░░░ 
                                      ███ ░███             
                                     ░░██████              
                                      ░░░░░░            
```

## Principal motivations

`onyx` is an initiative to modern and accurate programming taking advantage
by its speed and modern syntax to give the best results as possible. It can be considered as a research language, that will serve various purposes in computing.

### Basic Data Types

We can note that these are the fundamental data types found in many
programming language. However, we need to mention new types integration to
the language, to improve syntax and prorgamming accross precision,
optimization, and diversity.

- `string`

- `int` (, int32, int64, int128, int256, etc...)

- `real` (, r32, r64, r128, r256, etc...)

- `complex`

- `bool`

- `arr` for array

- `none`

Note: There is interoperability between numerical types (i.e int, real, complex)

This suggests that the basic operators can be applied in between them without any problem. There we have:

- `+`
- `-`
- `*`
- `/`
- `%`
- `^`
- `ln()`
- `exp()`

A little remark: all mathematical operations listed here have exactly the same properties as the mathematical rules operate.
`token` and `map` are set aside for now -- to be introduced later once their syntax is decided.

### The `arr` type — Multidimensional Tensors

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

Note: `arr` was deliberately not built on top of Clifford algebra, which
caps out at 24 components -- true independent-axis tensors scale to any
rank, which is more useful for a general-purpose multidimensional type.

### Keywords

- `lambda`: to create lambda functions.

```onyx
lam_func = lambda (n: int) = n + 1
result = lam_func(2)
print("And the result is: {result}") (< It should be 3 >)
print("And the result is: {lam_func(8)}")

(< Zero-parameter lambda: parentheses are always required, never omitted. >)
no_op = lambda () = none
```

There is only one way to use this: only as shown in the example. `=`
separates the parameter list from the body -- no return type is written
after the parentheses, and the parentheses are never omitted, even with
zero parameters.

- `class`: to create data types and object oriented programming.

```onyx
(< Simulating Value class from micrograd: >)

class Value:
    #from(* data: real, children: tuple = (), op: string = "", label: string = "") (<They all become attributes. `data` is the only required one -- the others must have a default value, or it's a compile/interpret error.>)
    .grad: real = 0.0 (< All variables preceded of a dot belong to the instance. >)
    .backward: none = lambda () = none
    #format = "Value({.data})" (<Formatting function for `Value` instance.>)
```

- `print`: to print out on screen, with `{}` to print variables by string formatting.

```onyx
var: string = "programming language."
print("How to use a {var}")
print('How to use   {var}')
```

- `match/case`:
    It functions as it always in its algorithmic way

```onyx
value = 9

match value:
    case 1:
        print("Monday")
    case 2:
        print("Tuesday")
    case 3:
        print("Wednesday")
    case 4:
        print("Thursday")
    case 5:
        print("Friday")
    case 6:
        print("Saturday")
    case 7:
        print("Sunday")
    default:
        print("Value error encountered")
```

- `try/fallback`:
    The code block stated in `try` is tested, the effects kept,
    if not completed, or code block has an error, fallback is executed.

```onyx
try:
    this = Value(2.6)
fallback:
    print("Error: Failed to initialize this.")
```

- `if`/`elif`/`else`:
    Functions the algorithmic way.

```onyx
real_var = 0.366565649486516846584684

if real_var > 1:
    print("{real_var} is greater than one.")
elif real_var < 1:
    print("{real_var} is less than one")
else:
    print("{real_var} is equal to 0")
```

- Comments:

```onyx
    (< Multi-line (And single-line) comments symbol >)
    Start: '(<'
    End  : '>)'
```

### CONVENTIONS & CONCEPTS

- VARIABLE DECLARATION CONTRACT:

    It follows the following template:

    ```onyx
    variable_name = data
    ```

    The variable's datatype is automatically infered. But if you want precision on your data, you have the possibility to write it as follows:

    ```onyx
    variable_name: data_type = data
    ```

- THE `STRANGER` CONCEPT:

    In a class/data structure definition,
    all variables preceded by a dot belong to the
    class definition body, all other variables
    are strangers.

- THE `class` CONFIGURATION FOR DATA TYPES AND CLASSES:

    A class is first of all, a data structure and computed
    representation of data and situational concepts to illustrate
    a real world situation. Looking at an object's structure,
    we can see that it has so many useful components we can explore, such as:

  - Parents:

    An object is certainly the result of it's preceding state. If it doesn't, it means it's a new concept at new sight. In onyx, we use the `#parents(...)` syntax to enumerate and collect it's preceding components. When used, this means that the child class heritates the methods, the attributes and all possible other characteristics of its parents.

  - Exterior Factors:

    This is certainly more complex than other concepts
    to get. These are the outside variables that helps us to
    create the instance of an object, taken in account for the
    object to exist. Those vars become certainly the property of
    the object so created. They belong to the object. This is
    certainly done with the help of the `#from(...)` keyword, that
    initializes those variables in the object's constructor to
    make them it's property.

  - Format:

    All the objects we use in a situation gives us information on
    how useful they can be in a situation. They possess attributes
    and that is why they appear in a given way. This been said, it
    suggests that these informations can be represented to us the
    way we want to see them. That's why we use `#format = "..."` to
    express our class's implementation's representation, with there
    is to be known by the language.

  - The IMPORTANT concept:

    In the `#from` operator, it is important to note that, if a `*` is
    preceding a variable, this means, that the concerned variables
    are required to create a class. If no default value is attributed
    to the other vars, the program will automatically raise an error and not compile (or interpret).

## Open Design Questions

| # | Topic | Status |
|---|-------|--------|
| 1 | Mutability (can a variable/attribute be reassigned after declaration? const vs. mutable?) |undone |
| 2 | Scoping & closures (how a `lambda` captures variables from its enclosing scope) | undone |
| 3 | Error model (what an error actually *is* in onyx: a type? a code+message pair? a `class`?) | undone |
