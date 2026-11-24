# Steepest Descent and Modified Newton on the Rosenbrock Function

Compare first-order and curvature-aware minimization of the nonconvex Rosenbrock function from the same starting point. Both methods use Armijo backtracking, and their trajectories, objective values, distances to the minimizer and accepted step lengths are recorded.

The unique global minimizer is $(1,1)$ because the objective is a sum of nonnegative squares and vanishes only there.

[Open notebook](../notebooks/steepest_descent_and_modified_newton.ipynb)

## Rosenbrock objective

$$f(x)=100(x_2-x_1^2)^2+(1-x_1)^2.$$

Its narrow curved valley makes steepest descent challenging.

## Gradient

$$\nabla f(x)=\begin{bmatrix}-400x_1(x_2-x_1^2)-2(1-x_1)\\200(x_2-x_1^2)\end{bmatrix}.$$

## Hessian and curvature

$$\nabla^2 f(x)=\begin{bmatrix}1200x_1^2-400x_2+2&-400x_1\\-400x_1&200\end{bmatrix}.$$

At $(1,1)$ the eigenvalues are approximately 1001.6 and 0.399; the pronounced scale difference explains ill-conditioning. At $(0,1)$ the first diagonal entry is negative, so the function is not globally convex.

## Armijo backtracking

Starting from $\alpha_0=1$, reduce the trial step by $\beta=0.5$ until $f(x+\alpha p)\le f(x)+\sigma\alpha g^Tp$, with $\sigma=0.1$.

## Steepest descent

Use $p_k=-\nabla f(x_k)$ and stop when the gradient norm falls below $10^{-6}$. The recorded histories support convergence diagnostics.

## Configurable initial trial step

The same sufficient-decrease test is used with an externally supplied initial step.

## Step-size sensitivity solver

This variant records the convergence index for a given initial trial step, allowing a controlled comparison.

## Modified Newton method

Solve $H_kp_k=-g_k$ after shifting the Hessian by $(-\lambda_{\min}+10^{-3})I$ when its smallest eigenvalue is nonpositive. The shift produces positive curvature; backtracking globalizes the step.

## Shared initialization and numerical comparison

Both methods start at $(-1,2)$ with the same gradient tolerance.

## Initial trial-step experiment

Compare five initial trial steps while retaining the Armijo reduction and stopping criterion.

## Steepest descent convergence plots

Inspect the decrease in objective value, distance to the solution and accepted step sizes.

## Modified Newton convergence plots

The same diagnostics reveal the much shorter curvature-aware trajectory.

## Results and interpretation

| Method | Reported convergence index | Final objective |
|---|---:|---:|
| Steepest descent | 13,727 | $6.9273\times10^{-13}$ |
| Modified Newton | 25 | $7.4688\times10^{-21}$ |

Steepest descent ends at approximately $(0.99999917,0.99999833)$; modified Newton reaches $(1,1)$ to the displayed precision. The difference in iteration counts illustrates the benefit of exploiting local curvature in the Rosenbrock valley. It is not a runtime comparison: Newton iterations have a higher per-step computational cost.

| Initial trial step | Steepest descent convergence index |
|---:|---:|
| 0.1 | 13,414 |
| 0.5 | 12,577 |
| 1 | 13,727 |
| 2 | 13,727 |
| 5 | 7,361 |

Among these settings, 5 gives the smallest iteration count. Backtracking means that the initial trial step is not the actual step, and the relationship between trial step and convergence is not monotone. The objective decreases under accepted Armijo steps, whereas Euclidean distance need not decrease monotonically. The results are specific to this initialization, tolerance and implementation.

## Saved figures

- [figure 23 01](../results/steepest_descent_and_modified_newton/figure_23_01.png)
- [figure 23 02](../results/steepest_descent_and_modified_newton/figure_23_02.png)
- [figure 23 03](../results/steepest_descent_and_modified_newton/figure_23_03.png)
- [figure 25 01](../results/steepest_descent_and_modified_newton/figure_25_01.png)
- [figure 25 02](../results/steepest_descent_and_modified_newton/figure_25_02.png)
- [figure 25 03](../results/steepest_descent_and_modified_newton/figure_25_03.png)
