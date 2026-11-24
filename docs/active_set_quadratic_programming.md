# Active-Set Method for Convex Quadratic Programming

Solve a two-dimensional strictly convex quadratic program with linear inequality constraints using a working-set KKT system. Compare an interior starting point, a vertex and a non-vertex boundary point.

[Open notebook](../notebooks/active_set_quadratic_programming.ipynb)

## Objective and feasible region

$$\min\; x_1^2+2x_2^2-2x_1-6x_2-2x_1x_2$$

subject to $x_1+x_2\le2$, $-x_1+2x_2\le2$ and $x_1,x_2\ge0$.

## Quadratic form

Write $f(x)=\tfrac12x^THx+c^Tx$ with $H=\begin{bmatrix}2&-2\\-2&4\end{bmatrix}$ and $c=(-2,-6)^T$. The leading principal minors are 2 and 4, so $H$ is positive definite.

## Matrix form of inequality constraints

Encode the four inequalities as $Ax\le b$ in the order shown by the code. Python uses zero-based indices.

## Working-set KKT solver

Solve $\begin{bmatrix}H&A_W^T\\A_W&0\end{bmatrix}\begin{bmatrix}p\\\lambda\end{bmatrix}=\begin{bmatrix}-g\\0\end{bmatrix}$. If $p=0$ and multipliers are nonnegative, stop; otherwise drop the most negative multiplier. For a nonzero direction, choose a feasible step limited by blocking inequalities and refresh the working set.

## Compare three feasible initial points

Start at $(0.5,0.3)$, $(0,0)$ and $(1.5,0.5)$.

## Results and interpretation

| Starting point | Reported solution | Objective | Iteration index |
|---|---|---:|---:|
| Interior | (0.8, 1.2) | -7.2 | 2 |
| Vertex | (0.8, 1.2) | -7.2 | 6 |
| Non-vertex boundary | (0.8, 1.2) | -7.2 | 1 |

The numerical solution is the unique optimum. Indeed, at $(0.8,1.2)$ the gradient is $(-2.8,-2.8)$ and the first constraint is tight. Its multiplier is 2.8 in the scaled representation $x_1+x_2\le2$ (or 5.6 for the original half-scaled constraint). The other inequalities are slack, so their multipliers are zero.

**Output consistency note:** the saved output prints `W* = [1]` in all three cases, but for the displayed constraint matrix the tight row is index **0**, not 1: $-0.8+2(1.2)=1.6<2$. The original printed indices are retained; the KKT verification above is the mathematically consistent interpretation. The experiment shows that the starting point influences the number of working-set transitions, but all displayed solutions agree.

## Saved figures

No figures are stored for this experiment; numerical output is available in the notebook.
