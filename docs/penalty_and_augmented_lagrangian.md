# Quadratic Penalty and Augmented Lagrangian Methods

Compare exact quadratic-penalty subproblem solves and the method of multipliers for a strictly convex equality-constrained quadratic. Use the analytical KKT solution as a reference for primal feasibility, objective convergence and multiplier convergence.

[Open notebook](../notebooks/penalty_and_augmented_lagrangian.ipynb)

## Equality-constrained objective

$$\min f(x)=x_1^2+2x_2^2+3x_3^2,\qquad h(x)=x_1+x_2+x_3-1=0.$$

## Analytical KKT solution

For $Q=\operatorname{diag}(1,2,3)$, stationarity is $2Qx+\nu\mathbf1=0$. Enforcing $\mathbf1^Tx=1$ gives $\nu^*=-12/11$ and $x^*=(6/11,3/11,2/11)^T$.

## Exact quadratic-penalty subproblem

Minimize $f(x)+\tfrac\rho2h(x)^2$ by solving $(2Q+\rho\mathbf1\mathbf1^T)x=\rho\mathbf1$.

## Exact augmented Lagrangian subproblem

Minimize $f(x)+\lambda h(x)+\tfrac\rho2h(x)^2$ by solving $(2Q+\rho\mathbf1\mathbf1^T)x=(\rho-\lambda)\mathbf1$.

## Outer iterations and comparison

The penalty method multiplies $\rho$ by 10 from an initial 1 for nine subproblems. The multiplier method keeps $\rho=10$ fixed for eight subproblems and updates $\lambda\leftarrow\lambda+\rho h(x)$. The specified initial point is retained as an argument, but exact subproblem solves do not depend on it.

## Constraint, objective and multiplier convergence

Compare the residuals on a logarithmic scale, the objective against its analytical optimum, and the multiplier against its KKT value.

## Results and interpretation

The exact solution is $(6/11,3/11,2/11)\approx(0.545455,0.272727,0.181818)$, with $f^*=6/11$ and multiplier $-12/11$.

| Method | Subproblems | Final penalty parameter | Final absolute constraint residual |
|---|---:|---:|---:|
| Quadratic penalty | 9 | $10^8$ | $1.090909\times10^{-8}$ |
| Augmented Lagrangian | 8 | 10 | $8.761356\times10^{-9}$ |

Both objective values approach the constrained optimum from below because intermediate points need not satisfy the equality. The augmented Lagrangian reaches a slightly smaller residual using a fixed moderate penalty and one fewer subproblem. Its multiplier approaches the analytical KKT multiplier. In contrast, the quadratic penalty uses increasingly large coefficients, which can worsen the conditioning of the linear system.

This is a comparison of exact subproblem solutions with chosen schedules, not a general runtime benchmark. The displayed residuals and rounded objective values do not imply exact feasibility.

## Saved figures

- [figure 13 02](../results/penalty_and_augmented_lagrangian/figure_13_02.png)
- [figure 13 03](../results/penalty_and_augmented_lagrangian/figure_13_03.png)
- [figure 13 04](../results/penalty_and_augmented_lagrangian/figure_13_04.png)
