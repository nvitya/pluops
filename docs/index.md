# PLUOpS: Programming Languages Unambiguous Operator Specification

Publicly available, open specification for designing progamming languages.

Version: 1.1 (2026-09-03)
Specification page: [https://nvitya.github.io/pluops/](https://nvitya.github.io/pluops/)

## Value Types

The following generic type descriptions are used here:

| Type Name | Description |
| --------- | ----------- |
| `int`     | Signed integer number
| `uint`    | Unsigned integer number
| `float`   | Floating-point number
| `bool`    | Boolean value; it can be either `true` or `false`
| `ptr`     | Pointer value containing a memory address. It can be typed or untyped

### Generic Type Rules for Numeric Types

Unary operators (operators with a single operand, such as negation) usually preserve the operand's type. Operators with two operands usually use the following rules:

| Operand 1 | Operand 2 | Result    |
| --------- | --------- | --------- |
| `int`     | `int`     | `int`     |
| `uint`    | `uint`    | `uint`    |
| `float`   | `float`   | `float`   |
| `uint`    | `int`     | `int`     |
| `int`     | `uint`    | `int`     |
| `float` | `int` or `uint` | `float` |
| `int` or `uint` | `float` | `float` |

## Unary Operators: Operators with One Operand

Form: `operator_symbol operand` or `operand operator_symbol`

| Operator Symbol | Valid Types | Description |
| --- | --- | --- |
| `-` | `int`, `uint`, `float` | **Numerical negation**: `- a`<br>Either floating-point negation or integer negation.<br>Unsigned integers are converted to signed integers: `uint -> int`.
| `not` | `bool` | **Logical NOT**: `not a`<br>The result is `bool`.
| `~` | `int`, `uint` | **Bitwise NOT**: `~ a`<br>Preserves integer signedness.<br>For scripting languages, floating-point operands should first be converted to integers using the `Round()` function.
| `%`<br>(recommended) | any addressable value | **Address-of**: `% a`<br>The operand must be a variable or an expression that can provide an unambiguous memory address.<br>The result is a typed `ptr`, such as `^int`, `^uint`, or `^float`.<br>Do not use this symbol for integer remainder.
| `^`<br>(prefix) | any **type** | **Pointer type designator**: `^T`<br>**Allowed only in type expressions.**<br>The result is a pointer **type** pointing to type `T`, such as `^int`, `^uint`, `^float`, or `^bool`.<br>Multiple levels are allowed, such as `^^int` or `^^^int`.
| `^`<br>(postfix) | `ptr` | **Pointer dereference**: `a^`<br>The result has the type referenced by the typed pointer: `^int -> int`, `^uint -> uint`, `^float -> float`, etc.

## Binary Operators: Operators with Two Operands

Form: `operand1 operator_symbol operand2`

| Operator Symbol | Valid Types | Description |
| --- | --- | --- |
| `+` | `int`, `uint`, `float` | **Numerical addition**: `a + b`<br>Either floating-point or integer addition according to the "Generic Type Rules".
| `-` | `int`, `uint`, `float` | **Numerical subtraction**: `a - b`<br>Either floating-point or integer subtraction according to the "Generic Type Rules", with one exception:<br>`uint - uint -> int`.
| `*` | `int`, `uint`, `float` | **Numerical multiplication**: `a * b`<br>Either floating-point or integer multiplication according to the "Generic Type Rules".
| `/` | `int`, `uint`, `float` | **Floating-point division**: `a / b`<br>This operator always performs floating-point division; the result is always a floating-point value.
| `div` | `int`, `uint` | **Truncating integer division**: `a div b`<br>This operator always performs truncating integer division.<br>The result is `uint` for `uint div uint`.<br>The result is `int` for `int div uint` or `uint div int`.
| `rem` | `int`, `uint` | **Integer division remainder**: `a rem b`<br>Same as the `a % b` in the C programming language.<br> `r = a - (a div b) * b`, can be negative!<br> The result is `uint` for `uint rem uint`.<br>The result is `int` for `int rem uint` or `uint rem int`.
| `mod` | `int`, `uint` | **Integer modulo**: `a mod b`<br>`r = a - (a div b) * b`<br>`if r < 0 then r = r + b`<br>Cannot be negative.<br>The result is `uint` for `uint mod uint`.<br>The result is `int` for `int mod uint` or `uint mod int`.
| `&` | `int`, `uint` | **Bitwise AND**: `a & b`<br>Preserves integer signedness.<br>For scripting languages, floating-point operands should first be converted to integers using the `Round()` function (to handle 0.999 as 1).
| <code>&#124;</code> | `int`, `uint` | **Bitwise OR**: <code>a &#124; b</code><br>Preserves integer signedness. This operation is invalid for floating-point numbers.<br>For scripting languages, floating-point operands should first be converted to integers using the `Round()` function.
| `<<` | `int`, `uint` | **Bitwise shift left**: `a << b`<br>Preserves the signedness of `a`. This operation is invalid for floating-point numbers.<br>For scripting languages, floating-point operands should first be converted to integers using the `Round()` function.
| `>>` | `int`, `uint` | **Bitwise shift right**: `a >> b`<br>Preserves the signedness of `a`. This operation is invalid for floating-point numbers.<br>For scripting languages, floating-point operands should first be converted to integers using the `Round()` function.
| `and` | `bool` | **Logical AND**: `a and b`<br>The result is `bool`.
| `or` | `bool` | **Logical OR**: `a or b`<br>The result is `bool`.
| `==` | `int`, `uint`, `float`, `bool` | **Numerical equality comparison**: `a == b`<br>An `int` comparison is used when each operand is either `int` or `uint`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `<>` | `int`, `uint`, `float`, `bool` | **Numerical inequality comparison**: `a <> b` (preferred)<br>An `int` comparison is used when each operand is either `int` or `uint`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `!=` | `int`, `uint`, `float`, `bool` | **Numerical inequality comparison**: `a != b` (alternative)<br>An `int` comparison is used when each operand is either `int` or `uint`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `<` | `int`, `uint`, `float` | **Numerical less-than comparison**: `a < b`<br>A `uint` comparison is used for `uint < uint`.<br>An `int` comparison is used for `int < uint` or `uint < int`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `<=` | `int`, `uint`, `float` | **Numerical less-than-or-equal comparison**: `a <= b`<br>A `uint` comparison is used for `uint <= uint`.<br>An `int` comparison is used for `int <= uint` or `uint <= int`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `>` | `int`, `uint`, `float` | **Numerical greater-than comparison**: `a > b`<br>A `uint` comparison is used for `uint > uint`.<br>An `int` comparison is used for `int > uint` or `uint > int`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `>=` | `int`, `uint`, `float` | **Numerical greater-than-or-equal comparison**: `a >= b`<br>A `uint` comparison is used for `uint >= uint`.<br>An `int` comparison is used for `int >= uint` or `uint >= int`.<br>A `float` comparison is used when either operand is `float`.<br>The result is always `bool`.
| `.` | structured types | **Structured type member access**: `a.member_name`.
| `is` | any expression + Type | **Type test**: `a is T`<br>The result is `bool`: `true` if the type of `a` is `T` or a descendant of `T`.
| `as` | any expression + Type | **Type casting**: `a as T`<br>The result is `a` converted to type `T`.<br>**The conversion might be invalid.**<br>Some languages might provide alternative forms, such as `T(a)`.
| `[`+`]`<br>(postfix) | `a`: array or `ptr`<br>`b`: `int`, `uint` | **Pointer or array indexing**: `a[b]`<br>**On arrays:** The array element at index `b` in array `a` is returned.<br>**On pointers:** When `a = ^T`, the result type is also `^T` and points to the address `a + b * SizeOf(T)` (without dereferencing, unlike in C).

## Parentheses

Form: `( expression )`

The `(` and `)` symbols are used to group expressions, as in mathematics. The enclosed expression is treated as a single operand in the containing expression, overriding the default operator precedence where applicable.

A parenthesized expression has the same type, value, and addressability as the enclosed expression. Parenthesized expressions may be nested.

## Operator Precedence

The operator precedence was designed to avoid the need for parentheses in the most common expressions.

Precedence is listed from highest to lowest.

| Level | Operators and syntax | Meaning |
| --- | --- | --- |
| 1 | literals, identifiers, `(...)`, `[...]`, `T(expr)`, `new T` | Primary expressions, array literals, casts, allocation |
| 2 | `expr(args...)`, `expr.member`, `expr[index]`, `expr[start:end]`, `ptr[index]`, `ptr^` | Function calls, member access, indexing, slicing, pointer indexing, pointer dereference |
| 3 | `%expr`, `-expr`, `~expr` | Address-of, unary minus, bitwise NOT |
| 4 | `<<`, `>>` | Bit shifts |
| 5 | `&` | Bitwise AND |
| 6 | <code>&#124;</code>, `xor` | Bitwise OR, bitwise XOR |
| 7 | `/`, `div`, `mod` | Floating-point division, integer division, integer remainder |
| 8 | `*` | Multiplication |
| 9 | `+`, `-` | Addition, subtraction |
| 10 | `==`, `<>`, `<`, `<=`, `>`, `>=`, `is`, `as` | Comparison, type test, "as" cast |
| 11 | `not` | Logical NOT |
| 12 | `and` | Logical AND |
| 13 | `or` | Logical OR |


## Symbols Left Free

| Symbol | Possible Uses |
| --- | --- |
| `{`, `}` | block delimiters
| `'` | String delimiter
| `"` | String delimiter
| `:` | Block start, type designation marker, member name designation marker
| `;` | list separator
| `;` | Statement termination
| `\` | Escape symbol in strings, line continuation
| `?` | DQ: inference marker
| `#` | DO: preprocessor directive
| `$` | DQ: context-dependent symbols
| `@` | DQ: namespace designator
| `` ` `` | Recommended for infix operators, such as `` `cxdiv` ``
| `!` | - |


## Document Changes

| Version | Date (ISO) | Persons | Changes |
| --- | --- | --- | --- |
| 1.1 | 2026-09-03 | Viktor Guáth-Nagy | Added `rem` operator<br>corrected `==`, `<>` and `!=` valid with bool
| 1.0 | 2026-08-23 | Viktor Guáth-Nagy | Initial version


## Contributing

For changes or improvements please create an issue here:

[https://github.com/nvitya/pluops/issues](https://github.com/nvitya/pluops/issues)
