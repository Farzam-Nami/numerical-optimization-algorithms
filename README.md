# Numerical Optimization Algorithms

Python implementations and experimental analyses of unconstrained and constrained numerical optimization, including line-search methods, quasi-Newton updates, convex quadratic programming, Lagrangian duality and an application to binary image classification.

## Experiments

| Topic | Notebook | Technical notes |
|---|---|---|
| Steepest Descent and Modified Newton on the Rosenbrock Function | [Notebook](notebooks/steepest_descent_and_modified_newton.ipynb) | [Analysis](docs/steepest_descent_and_modified_newton.md) |
| Newton and Cubic Interpolation for Scalar Optimization | [Notebook](notebooks/newton_and_cubic_interpolation.ipynb) | [Analysis](docs/newton_and_cubic_interpolation.md) |
| Nonlinear Conjugate Gradient and L-BFGS for Binary MNIST Classification | [Notebook](notebooks/logistic_regression_optimization.ipynb) | [Analysis](docs/logistic_regression_optimization.md) |
| BFGS and DFP Quasi-Newton Optimization | [Notebook](notebooks/bfgs_and_dfp.ipynb) | [Analysis](docs/bfgs_and_dfp.md) |
| Frank–Wolfe Optimization on the Probability Simplex | [Notebook](notebooks/frank_wolfe.ipynb) | [Analysis](docs/frank_wolfe.md) |
| Active-Set Method for Convex Quadratic Programming | [Notebook](notebooks/active_set_quadratic_programming.ipynb) | [Analysis](docs/active_set_quadratic_programming.md) |
| Lagrangian Duality and Objective–Constraint Geometry | [Notebook](notebooks/lagrangian_duality.ipynb) | [Analysis](docs/lagrangian_duality.md) |
| Quadratic Penalty and Augmented Lagrangian Methods | [Notebook](notebooks/penalty_and_augmented_lagrangian.ipynb) | [Analysis](docs/penalty_and_augmented_lagrangian.md) |

## Selected results

| Experiment | Observed result |
|---|---|
| Rosenbrock: steepest descent vs. modified Newton | Convergence indices 13,727 vs. 25 at a gradient tolerance of $10^{-6}$ |
| Scalar minimization | Newton (initial step 1) and cubic search both report four iterations, with objective approximately -2.65342640972 |
| Binary MNIST | NCG-FR has the lowest reported loss (110.5241); NCG-PR+ reaches 100% reported test accuracy for two initial steps, but none of the configurations meets the gradient tolerance |
| BFGS vs. DFP | BFGS reaches the small-gradient regime sooner in the plotted experiment |
| Frank–Wolfe | Gap approximately $10^{-4}$ at iteration index 4,789 |
| Active-set QP | All three starts reach $(0.8,1.2)$, objective -7.2 |
| Lagrangian duality | Same primal optimum -1; unique multiplier 1 versus multiplier interval $[0,1]$ |
| Equality-constrained quadratic | Augmented Lagrangian residual $8.76\times10^{-9}$ with fixed penalty 10 |

The numerical observations are experiment-specific. Iteration counts across algorithms do not measure equal computational work. The notebooks and technical notes distinguish empirical results from analytical solutions and discuss implementation limitations, including the inconsistent printed active-constraint index and the support-line sign in the duality illustration.

## Repository structure

```text
notebooks/     Jupyter notebooks with implementations and saved outputs
docs/          Mathematical background, experimental analysis and limitations
results/       Saved figures and reported numerical metrics
datasets/      Data access instructions
```

## Getting started

```bash
git clone https://github.com/farzam-nami/numerical-optimization-algorithms.git
cd numerical-optimization-algorithms
python -m pip install -r requirements.txt
cd notebooks
jupyter notebook
```

Open the notebook for the desired experiment and run cells in order. The MNIST experiment can download data using scikit-learn (OpenML); optionally install `tensorflow` to use the Keras loader. See [data access](datasets/README.md) for offline instructions. Generated files are saved using the paths in each notebook, relative to its working directory.

## Results and reproducibility

The notebooks contain recorded numerical outputs. The [results directory](results/README.md) provides the saved figures and the reported MNIST metric table. Exact reproducibility depends on package versions, numerical libraries, and random sampling; the duality illustration has no fixed random seed. The project is intended to demonstrate and compare the implemented methods on the specified problems, not to establish general performance rankings.
