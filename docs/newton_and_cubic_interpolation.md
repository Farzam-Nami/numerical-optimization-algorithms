# Newton and Cubic Interpolation for Scalar Optimization

Minimize the strictly convex scalar objective $f(x)=e^{x-3}-x/2-2$ using derivative-based Newton steps and a bracketed cubic interpolant. The analytical solution is $x^*=3-\log 2\approx2.30685281944$, with $f^*=-(3-\log 2)/2-1.5\approx-2.65342640972$.

[Open notebook](../notebooks/newton_and_cubic_interpolation.ipynb)

## Objective and derivatives

$f'(x)=e^{x-3}-1/2$ and $f''(x)=e^{x-3}>0$, so the stationary point is the unique global minimizer.

## Armijo line search

Shrink the trial length by a factor of 0.5 until the sufficient-decrease inequality holds, using $\sigma=0.1$.

## Newton iteration

The scalar Newton direction is $p=-f'(x)/f''(x)$. Terminate when $|f'(x)|<10^{-8}$ and track distance to the analytical optimum.

## Newton experiment and convergence plots

Start from $x_0=0$ and use initial trial step 1.

## Effect of initial trial step

Repeat the Newton experiment for five values with the same stopping tolerance.

## Cubic interpolation search

Use endpoint function values and derivatives to construct a cubic model on a bracket containing the stationary point.

### Objective and first derivative

The interpolant uses the same objective as the Newton experiment.

### Cubic Hermite model

Construct $p(t)=c_0+c_1t+c_2t^2+c_3t^3$ from $p(a)=f(a),p(b)=f(b),p'(a)=f'(a),p'(b)=f'(b)$. Solve the quadratic equation $p'(t)=0$ and choose an interior candidate by its actual objective value.

### Bracket refinement

Update the endpoint according to the sign of $f'(x)$, preserving $f'(a)<0<f'(b)$.

### Numerical cubic interpolation experiment

Begin with the interval $[0,4]$ and stop at the derivative tolerance.

## Results and interpretation

| Initial trial step | Newton convergence index |
|---:|---:|
| 0.1 | 166 |
| 0.5 | 23 |
| 1 | 4 |
| 2 | 4 |
| 5 | 12 |

With initial step 1, Newton obtains $x=2.3068528194402256$ and $f=-2.6534264097200273$. The cubic search returns $x=2.306852819606583$ and the same displayed objective in four interpolant evaluations. Newton with trial steps 1 and 2 is the fastest of the tested Newton configurations; small steps slow progress near the solution, while large steps can trigger backtracking.

The two algorithms use different information and initialization: Newton uses local second derivatives, whereas cubic search uses values and first derivatives at both endpoints. Equal iteration counts do not imply equal evaluation cost. The cubic routine assumes an interior stationary candidate exists; it does not implement a general fallback when interpolation fails.

## Saved figures

- [figure 09 02](../results/newton_and_cubic_interpolation/figure_09_02.png)
- [figure 09 03](../results/newton_and_cubic_interpolation/figure_09_03.png)
- [figure 09 04](../results/newton_and_cubic_interpolation/figure_09_04.png)
