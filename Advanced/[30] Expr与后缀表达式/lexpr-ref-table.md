# lexpr reference table

| Operator  |         Stack Before           |           Stack After             |
| --------- | ------------------------------ | --------------------------------- |
|           |  (constants)                   |                                   |
|  `pi`     |    `[]`                        |   `[ math.pi ]`                   |
|  `1 exp`  |    `[]`                        |   `[ math.e ]`                    |
|  `N`      |    `[]`                        |   `[ (current frame number) ]`    |
|  `X`      |    `[]`                        |   `[ (current column number) ]`   |
|  `Y`      |    `[]`                        |   `[ (current row number) ]`      |
|  `width`  |    `[]`                        |   `[ (current plane width) ]`     |
|  `height` |    `[]`                        |   `[ (current plane height) ]`    |
|           |  (Arithmetic operators)        |                                   |
|  `+`      |    `[ a, b ]`                  |   `[ a+b ]`                       |
|  `-`      |    `[ a, b ]`                  |   `[ a-b ]`                       |
|  `*`      |    `[ a, b ]`                  |   `[ a*b ]`                       |
|  `/`      |    `[ a, b ]`                  |   `[ a/b ]`                       |
|  `%`      |    `[ a, b ]`                  |   `[ a-int(a/b)*b ]`              |
|`pow`/`**` |    `[ a, b ]`                  |   `[ a**b ]`                      |
|           |  (Unary operators)             |                                   |
|  `abs`    |    `[ a ]`                     |   `[ math.abs(a) ]`               |
|  `exp`    |    `[ a ]`                     |   `[ math.exp(a) ]`  (base e)     |
|  `log`    |    `[ a ]`                     |   `[ math.log(a) ]`  (base e)     |
|  `sqrt`   |    `[ a ]`                     |   `[ math.sqrt(a) ]`              |
|  `sin`    |    `[ a ]`                     |   `[ math.sin(a) ]`  (only for a < 1e5) |
|  `cos`    |    `[ a ]`                     |   `[ math.cos(a) ]`  (only for a < 1e5) |
|           |  (Rounding)                    |                                   |
|  `round`  |    `[ a ]`                     |   `[ round(a) ]`                  |
|  `floor`  |    `[ a ]`                     |   `[ math.floor(a) ]`             |
|  `trunc`  |    `[ a ]`                     |   `[ math.trunc(a) ]`             |
|           |  (comparison operators)        |  (returns 0 if false, 1 if true)  |
|  `<`      |    `[ a, b ]`                  |   `[ (1 if a < b else 0) ]`       |
|  `<=`     |    `[ a, b ]`                  |   `[ (1 if a <= b else 0) ]`      |
|  `>`      |    `[ a, b ]`                  |   `[ (1 if a > b else 0) ]`       |
|  `>=`     |    `[ a, b ]`                  |   `[ (1 if a >= b else 0) ]`      |
|  `=`      |    `[ a, b ]`                  |   `[ (1 if a == b else 0) ]`      |
|           |  (logical operators)           |  (treat <=0 as false, >0 as true) |
|  `and`    |    `[ a, b ]`                  |   `[ int(a >= 0 and b >= 0) ]`    |
|  `or`     |    `[ a, b ]`                  |   `[ int(a >= 0 or b >= 0) ]`     |
|  `xor`    |    `[ a, b ]`                  |   `[ int(a >=0 xor b >= 0) ]`     |
|  `not`    |    `[ a ]`                     |   `[ int(a < 0) ]`                |
|           |  (Bitwise operators)           |                                   |
|  `bitand` |    `[ a, b ]`                  |   `[ int(a) & int(b) ]`           |
|  `bitor`  |    `[ a, b ]`                  |   `[ int(a) \| int(b) ]`          |
|  `bitxor` |    `[ a, b ]`                  |   `[ int(a) ^ int(b) ]`           |
|  `bitnot` |    `[ a ]`                     |   `[ ~int(a) ]`                   |
|           |  (Ternary operators)           |                                   |
|  `?`      |    `[ c, t, f ]`               |   `[ c ? t : f ]` <br> (python: `[ (t if c else f) ]` |
| `clip`/`clamp` |  `[ v, mi, ma ]`          |   `[ max(min(v, ma), mi)  ]`      |
|           |  (Functions)      |                                   |
|  `lerp`   |    `[ left, right, x ]`        |   `[ left * (1 - x) + right * x ]` |
|  $\texttt{polyval}N$/  $\texttt{polyeval}N$ <br> ($N \ge 0$) | $[ c_N, c_{N-1}, \ldots, c_1, c_0, x ]$ <br>($N+2$ elements) | $[ \sum_{0\le i\le N} c_i x^i ]$ |
|           |  (Stack operators)             |                                   |
|  `swap`/`swap1` |    `[a, b]`                    |   `[b, a]`                        |
|  $\texttt{swap}N$ <br> ($N \gt 1$) | $[ a_{N}, a_{N-1}, \ldots, a_0 ]$ <br>($N+1$ elements) | $[ a_0, a_{N-1}, \ldots, a_1, a_{N} ]$ <br> ($N+1$ elements) |
|  `dup`/`dup0` |    `[a]`                       |   `[a, a]`                        |
|  $\texttt{dup}N$ <br> ($N \ge 1$)  | $[ a_{N}, a_{N-1}, \ldots, a_0 ]$ <br>($N+1$ elements) | $[ a_N, a_{N-1}, \ldots, a_0, a_{N} ]$ <br> ($N+2$ elements) |
|  `drop`   |    `[a, b, c]`                 |   `[a, b]`                        |
|  $\texttt{drop}N$ <br> ($N > 1$)  | $[ a_{N-1}, a_{N-1}, \ldots, a_0 ]$ <br>($N$ elements) | $[  ]$  <br>($0$ elements) |
|           |   (Ordering)                   |                                   |
|  `min`    |    `[a, b]`                    |   `[ min(a,b) ]`                  |
|  $\texttt{argmin}N$[^2] <br> ($N > 1$)  | $[ a_{N-1}, a_{N-1}, \ldots, a_0 ]$ ($N$ elements) | $[ b ]$ <br> ($a_{b}$ is the minimum value of all $a_i$) |
|  `max`    |    `[a, b]`                    |   `[ max(a,b) ]`                  |
|  $\texttt{argmax}N$[^2] <br> ($N > 1$)  | $[ a_{N-1}, a_{N-1}, \ldots, a_0 ]$ ($N$ elements) | $[ b ]$ <br> ($a_{b}$ is the maximum value of all $a_i$) |
|  $\texttt{sort}N$ <br> ($N > 1$)  | $[ a_{N-1}, a_{N-1}, \ldots, a_0 ]$ ($N$ elements) | $[ b_{N-1}, b_{N-1}, \ldots, b_0 ]$ <br> ($N$ elements, $\{b\}$ is a permutation of $\{a\}$ and $b_i \ge b_j$ whenever $i > j$) |
|  $\texttt{argsort}N$[^2] <br> ($N > 1$)  | $[ a_{N-1}, a_{N-1}, \ldots, a_0 ]$ ($N$ elements) | $[ b_{N-1}, b_{N-1}, \ldots, b_0 ]$ <br> ($N$ elements, $\lbrace b \rbrace$ is a permutation of $0\ldots N-1$, s.t. $a_{b_i} \ge a_{b_j}$ whenever $i > j$) |
|           |   (Variables)                  |                                   |
| `var!`    |    `[a]`                       | `[]`, and set variable `var` to a |
| `var@`    |    `[]` <br> variable `var` has value `b` | `[b]` <br> variable `var` value unchanged |
|           |   (Clip inputs)                |                                   |
| `x`/`src0`|    `[]`                        | `[ (in0[Y][X]) ]`                 |
| `x[relX,relY]`|`[]`                        | `[ (in0[Y+relY][X+relX]) ]`<br> has boundary conditions |
| `x[]`     |    `[absX absY]`               | `[ (in0[round(absY)][round(absX)]) ]`<br> use clamp boundary condition  |
|           |  (Frame properties)            |                                   |
| `x.PropName` |  `[]`                       |   `[ (in0.props['PropName']) ]` |

[^2]: only available in `Select`/`PropExpr`, not available in `Expr`.
