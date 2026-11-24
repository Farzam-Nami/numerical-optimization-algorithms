# Frank–Wolfe Optimization on the Probability Simplex

Solve a strictly convex quadratic over the three-dimensional probability simplex using conditional gradient (Frank–Wolfe) with an exact line search and a duality-gap stopping rule.

[Open notebook](../notebooks/frank_wolfe.ipynb)

## Quadratic objective and simplex oracle

$$\min_{x\ge0,\,\mathbf1^Tx=1}\; \tfrac12(x_1^2+x_2^2+0.1x_3^2)+0.55x_3.$$

The gradient is $(x_1,x_2,0.1x_3+0.55)^T$. The linear minimization oracle returns the unit vector at the index with the smallest gradient component. For $d=s-x$, exact line search gives $\alpha=\operatorname{clip}(-g^Td/(d^TQd),0,1)$ with $Q=\operatorname{diag}(1,1,0.1)$.

## Frank–Wolfe iteration

Update $x_{k+1}=x_k+\alpha_k(s_k-x_k)$. Convex combinations preserve feasibility; the implementation also performs numerical cleanup. Stop when $g_{FW}=\nabla f(x_k)^T(x_k-s_k)\le10^{-4}$ or the iteration limit is reached.

## Experiment from the simplex centroid

Start at $(1/3,1/3,1/3)$ with a maximum of 5,000 iterations.

## Results and interpretation

The saved output reports $x=(0.49955592,0.49945592,0.00098817)$, gap $9.999857700093272\times10^{-5}$ and iteration index 4,789. The exact minimizer is $(0.5,0.5,0)$ with objective 0.25: the gradient components for the two positive coordinates are 0.5, while the third is 0.55, satisfying simplex optimality. Strict convexity gives uniqueness.

The convergence curve approaches 0.25 rapidly at first and then slowly. The third coordinate shrinks toward zero; standard Frank–Wolfe steps toward simplex vertices can be slow near a boundary optimum. The nonnegative gap bounds primal suboptimality for this convex problem. The numerical result meets the chosen gap criterion rather than achieving the exact boundary point.

## Saved figures

- [figure 07 02](../results/frank_wolfe/figure_07_02.png)
