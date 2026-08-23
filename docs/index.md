# PLUOpS: Programming Languages Unambiguous Operator Specifiction

## Value Types

The following generic type descriptions are used here:

| Type Name | Description |
| --------- | ----------- |
| `int`     | Signed integer number
| `uint`    | Unsigned integer number
| `float`   | Floating point number
| `bool`    | Boolean value, it can be either `true` or `false`
| `ptr`     | Pointer value, containing a memory address. Can by typed or untyped

### Generic Type Rules for `int` and `float` types

The unary operators (the operators with single argument, like negation) usually keep their original types. The operator with two operands usually use the following rules:

| Operand 1 | Operand 2 | Result    |
| --------- | --------- | --------- |
| `int`     | `int`     | `int`     |
| `uint`    | `uint`    | `uint`    |
| `float`   | `float`   | `float`   |
| `uint`    | `int`     | `int`     |
| `int`     | `uint`    | `int`     |
| `float` | `int` or `uint` | `float` |
| `int` or `uint` | `float` | `float` |

## Unary Operators: Operators with One Operands

Form: `operator_symbol operand` or `operand operator_symbol`

| Operator Symbol | Valid Types | Description
| --- | --- | --- |
| `-` | `int`, `uint`, `float` | **Numerical negation**: `- a`<br>Either floating point negation, or integer negation.<br> Unsigned integers are converted to signed integers: `uint -> int`
| `not` | `bool` |**Logical NOT**: `not a`<br>result is `bool`
| `~` | `int`, `uint` | **Bitwise NOT**: `~ a`<br>Keeps integer signed-ness.<br>For script languages the floating point operands sould be converted to integers first with the Round() function.
| `%` | any addressable | **Address-of**: `% a`<br>The operand must be variable or an expression that can provide an unambiguous memory address.<br> The result is a typed `ptr`: `^int`, `^uint`, `^float` etc.
| `^`<br>(prefix)| any **type** | **Pointer type designator**: `^T`<br>**Allowed only in type expressions.**<br>The result is pointer **type** pointing to type `T`:<br> `^int`, `^uint`, `^float`, `^bool`<br>Multiple levels allowed with `^^int` or `^^^int` etc.
| `^`<br>(postfix)| `ptr` | **Pointer dereference**: `a^`<br>The result is the value with the type of the typed pointer:<br> `^int -> int`, `^uint -> uint`, `^float -> float` etc.

## Binary Operators: Operators with Two Operands

Form: `operand1 operator_symbol operand2`

| Operator Symbol | Valid Types | Description
| --- | --- | --- |
| `+` | `int`, `uint`, `float` | **Numerical addition**: `a + b`<br>Either floating point or integer addition according the "Generic Type Rules"
| `-` | `int`, `uint`, `float` | **Numerical substraction**: `a - b`<br>Either floating point or integer substraction according the "Generic Type Rules" with the exception:<br> `uint - uint -> int`
| `*` | `int`, `uint`, `float` | **Numerical multiplication**: `a * b`<br>Either floating point or integer multiplication according the "Generic Type Rules"
| `/` | `int`, `uint`, `float` | **Floating point division**: `a / b`<br>This operator does always floating point division, the result is always a floating-point value.
| `div` | `int`, `uint` | **Truncating integer division**: `a div b`<br>This operator does always truncating integer division.<br>`uint` result when `uint div uint`<br>`int` result when `int div uint` or `uint div int`.
| `mod` | `int`, `uint` | **Integer division remainder**: `a mod b`<br>`uint` result when `uint div uint`<br>`int` result when `int div uint` or `uint div int`.
| `&` | `int`, `uint` | **Bitwise AND**: `a & b`<br>Keeps integer signed-ness. <br>For script languages the floating point operands sould be converted to integers first with the Round() function (to handle 0.999 as 1)
| `\|` | `int`, `uint` | **Bitwise OR**: `a \| b`<br>Keeps integer signed-ness. Invalid operation on floating point numbers.<br>For script languages the floating point operands sould be converted to integers first with the Round() function
| `<<` | `int`, `uint` | **Bitwise Shift Left**: `a << b`<br>Keeps integer signed-ness of `a`. Invalid operation on floating point numbers.<br>For script languages the floating point operands sould be converted to integers first with the Round() function
| `>>` | `int`, `uint` | **Bitwise Shift Right**: `a >> b`<br>Keeps integer signed-ness of `a`. Invalid operation on floating point numbers.<br>For script languages the floating point operands sould be converted to integers first with the Round() function
| `and` | `bool` | **Logical AND**: `a and b`<br>result is `bool`
| `or` | `bool` | **Logical OR**: `a or b`<br>result is `bool`
| `==` | `int`, `uint`, `float` | **Numerical comparison for equality**: `a == b`<br>`int` comparison when both operands are either `int` or `uint`.<br>`float` comparison when any of the operands is `float`.<br> The result is always `bool`
| `<>` | `int`, `uint`, `float` | **Numerical comparison for non-equality**: `a <> b` (preferred)<br>`int` comparison when both operands are either `int` or `uint`.<br>`float` comparison when any of the operands is `float`.<br> The result is always `bool`
| `!=` | `int`, `uint`, `float` | **Numerical comparison for non-equality**: `a != b` (alternative)<br>`int` comparison when both operands are either `int` or `uint`.<br>`float` comparison when any of the operands is `float`.<br> The result is always `bool`
| `<` | `int`, `uint`, `float` | **Numerical comparison less-than**: `a < b`<br>`uint` comparison when `uint < uint`<br>`int` comparison when `int < uint` or `uint < int`.<br> `float` comparison when any of the operands is `float`.<br> The result is always `bool`
| `<=` | `int`, `uint`, `float` | **Numerical comparison less-than or equal**: `a <= b`<br>`uint` comparison when `uint <= uint`<br>`int` comparison when `int <= uint` or `uint <= int`.<br> `float` comparison when any of the operands is `float`.<br> The result is always `bool`
| `>` | `int`, `uint`, `float` | **Numerical comparison greater-than**: `a > b`<br>`uint` comparison when `uint > uint`<br>`int` comparison when `int > uint` or `uint < int`.<br> `float` comparison when any of the operands is `float`.<br> The result is always `bool`
| `>=` | `int`, `uint`, `float` | **Numerical comparison greater-than or equal**: `a >= b`<br>`uint` comparison when `uint >= uint`<br>`int` comparison when `int >= uint` or `uint >= int`.<br> `float` comparison when any of the operands is `float`.<br> The result is always `bool`



## Symbols Left Free

| Symbol | Possible Uses |
| --- | --- |
| `!` | - |
| `?` | DQ: inference marker |
| `#` | DO: preprocessor directive |
| `$` | DQ: context-dependant symbols |
| `` ` `` | Recommend to reserve for infix operators, like `` `cxdiv` ``




## Operator Precedence

Precedence is listed from highest to lowest.

| Level | Operators and syntax | Meaning |
| --- | --- | --- |
| 1 | literals, identifiers, `@namespace.name`, `(...)`, `[...]`, `Type(expr)`, `new`, builtins such as `Len(...)`, `SizeOf(...)`, `iif(...)` | Primary expressions, array literals, casts, allocation, builtin forms |
| 2 | `expr(args...)`, `expr.member`, `expr[index]`, `expr[start:end]`, `ptr[index]`, `ptr^` | Calls, member access, indexing, slicing, pointer indexing, pointer dereference |
| 3 | `%expr`, `-expr`, `~expr` | Address-of, unary minus, bitwise NOT |
| 4 | `<<`, `>>` | Bit shifts |
| 5 | `&` | Bitwise AND |
| 6 | `\|`, `xor` | Bitwise OR, bitwise XOR |
| 7 | `/`, `div`, `mod` | Division, integer division, integer modulo |
| 8 | `*` | Multiplication |
| 9 | `+`, `-` | Addition, subtraction |
| 10 | `==`, `<>`, `<`, `<=`, `>`, `>=`, `is`, `as` | Comparison, object type test, and explicit cast |
| 11 | `not` | Logical NOT |
| 12 | `and` | Logical AND |
| 13 | `or` | Logical OR |
