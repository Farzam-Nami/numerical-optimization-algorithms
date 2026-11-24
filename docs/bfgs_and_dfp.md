# BFGS and DFP Quasi-Newton Optimization

Compare two inverse-Hessian update rules on the two-dimensional Rosenbrock function, starting at $(-1.2,1)$ with an identity inverse-Hessian approximation. Both methods use the same Armijo line search and gradient-norm stopping criterion.

[Open notebook](../notebooks/bfgs_and_dfp.ipynb)

## Rosenbrock objective and gradient

$$f(x)=100(x_2-x_1^2)^2+(1-x_1)^2.$$ The unique global minimizer is $(1,1)$.

## Armijo backtracking

Start with unit trial step, reduce by 0.5 and enforce sufficient decrease with $c_1=10^{-4}$.

## DFP inverse-Hessian update

For $s=x_{k+1}-x_k$, $y=g_{k+1}-g_k$,

$$H_{k+1}=H_k+\frac{ss^T}{y^Ts}-\frac{H_kyy^TH_k}{y^TH_ky}.$$

## BFGS inverse-Hessian update

$$H_{k+1}=(I-\rho sy^T)H_k(I-\rho ys^T)+\rho ss^T,\quad \rho=(y^Ts)^{-1}.$$

## Shared quasi-Newton solver

Compute $p=-H_kg_k$, perform backtracking and update the approximation with the selected formula. Record the iterate, objective and gradient norm. The limit is 500 iterations and the gradient tolerance is $10^{-6}$.

## Execute both update rules

Use identical initial conditions.

## Objective and gradient-norm convergence

Logarithmic scales reveal the late-stage convergence behavior.

## Results and interpretation

The saved objective and gradient-norm plots show that BFGS reaches the small-gradient regime sooner than DFP. BFGS ends near iteration 34 and DFP near iteration 48 (approximate indices read from the plots). Both obtain very small objective values; BFGS shows a faster late-stage decline in this particular experiment.

The comparison is limited to one starting point and one objective. The inverse-Hessian updates assume nonzero denominators and appropriate curvature (in particular $s^Ty>0$ for preservation of positive definiteness). The code does not guard against all curvature breakdowns, and Armijo alone does not guarantee the usual Wolfe curvature condition. These are limitations of the implementation, not evidence that they occurred in the displayed experiment.

## Saved figures

- [figure 15 01](../results/bfgs_and_dfp/figure_15_01.png)
- [figure 15 02](../results/bfgs_and_dfp/figure_15_02.png)
