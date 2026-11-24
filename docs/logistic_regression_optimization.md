# Nonlinear Conjugate Gradient and L-BFGS for Binary MNIST Classification

Train a binary logistic regression classifier on MNIST digits 0 and 1 using Fletcher–Reeves (NCG-FR), Polak–Ribière+ (NCG-PR+) and limited-memory BFGS. Each optimizer starts from zero parameters and uses Armijo backtracking. The experiment compares initial trial lengths, objective reduction, gradient norm and classification accuracy.

[Open notebook](../notebooks/logistic_regression_optimization.ipynb)

## Experimental overview

All three optimizers minimize the same unregularized sum of cross-entropy losses. The model has 784 pixel weights and one bias.

## Numerical utilities and classification rule

Pixels are represented by a 784-dimensional vector. The sigmoid maps the score $z=w^Tx+b$ to a probability. The implementation deliberately evaluates labels using the **raw score threshold 0.5**, not a probability threshold of 0.5 (which would correspond to a raw score of zero). Accuracy is one minus the mean absolute binary-label error.

## Logistic objective and gradient

$$L(\theta)=-\sum_i[y_i\log p_i+(1-y_i)\log(1-p_i)],\quad p_i=\sigma(w^Tx_i+b).$$

The gradient is $[X^T(p-y),\mathbf1^T(p-y)]$. The routine supports optional weight-only L2 regularization, but the experiment uses zero regularization. Probability clipping and a small logarithm offset are used for numerical stability.

## Armijo backtracking

Accept $\alpha=\alpha_0\rho^j$ when $L(\theta+\alpha d)\le L(\theta)+c_1\alpha g^Td$. Non-descent directions fall back to $-g$. The search is capped at 50 trials and returns an unsuccessful zero step if no trial is accepted.

## Nonlinear conjugate gradient

$$d_{k+1}=-g_{k+1}+\beta_kd_k,$$

with Fletcher–Reeves $\beta_k=\|g_{k+1}\|^2/\|g_k\|^2$ and PR+ $\beta_k=\max(0,g_{k+1}^T(g_{k+1}-g_k)/\|g_k\|^2)$. Restart the direction when it is not descending.

## Limited-memory BFGS

Use the two-loop recursion to apply an implicit inverse-Hessian approximation. Retain the latest ten displacement/gradient-difference pairs in the experiment, accepting a pair only when $y_k^Ts_k>10^{-12}$.

## Experiment artifacts

Save loss and gradient-norm plots, history CSV files, final parameter arrays and timing metadata for each configuration.

## MNIST acquisition and preprocessing

Try Keras MNIST, then OpenML, then a local `mnist.npz`. Retain digits 0 and 1, normalize pixels by 255 and flatten each image. Training and test data retain their separate roles.

## Experimental protocol

Use initial trial lengths 1, 0.5, 0.1 and 0.05; maximum 200 iterations, gradient tolerance $10^{-6}$, Armijo reduction 0.5 and sufficient-decrease constant $10^{-4}$. Record both training and test accuracy. Summary loss and gradient are the last logged pre-update values, while accuracy uses the returned parameter vector.

## Run the optimizer comparison

The code writes its generated artifacts to `outputs/` relative to the working directory.

## Fletcher–Reeves convergence curves

Loss and gradient norm for the four initial trial lengths.

## Polak–Ribière+ convergence curves

Loss and gradient norm for the four initial trial lengths.

## Limited-memory BFGS convergence curves

Loss and gradient norm for the four initial trial lengths.

## Test accuracy across configurations

## Results and interpretation

The numerical table below is transcribed from the accompanying experimental analysis (rounded as reported); the saved notebook plots show the same test-accuracy pattern. The original generated summary CSV was not supplied.

| Method | Initial step | Logged loss | Gradient norm | Train acc. | Test acc. |
|---|---:|---:|---:|---:|---:|
| NCG-FR | 1 | 110.5244 | 15.3659 | 0.9997 | 0.9991 |
| NCG-FR | 0.5 | 110.5241 | 20.7197 | 0.9997 | 0.9995 |
| NCG-FR | 0.1 | 276.3102 | 27.8553 | 0.9992 | 0.9995 |
| NCG-FR | 0.05 | 255.6187 | 22.2671 | 0.9992 | 0.9995 |
| NCG-PR+ | 1 | 442.0963 | 34.6280 | 0.9987 | 1.0000 |
| NCG-PR+ | 0.5 | 442.1825 | 34.5873 | 0.9987 | 1.0000 |
| NCG-PR+ | 0.1 | 1243.3832 | 112.2495 | 0.9964 | 0.9991 |
| NCG-PR+ | 0.05 | 1125.9571 | 91.7196 | 0.9967 | 0.9995 |
| L-BFGS | 1 | 233.0927 | 31.0410 | 0.9993 | 0.9991 |
| L-BFGS | 0.5 | 221.0483 | 23.0236 | 0.9994 | 0.9995 |
| L-BFGS | 0.1 | 248.6792 | 38.1942 | 0.9993 | 0.9986 |
| L-BFGS | 0.05 | 295.7253 | 31.2668 | 0.9991 | 0.9986 |

All configurations record 200 iterations, and none has a final logged gradient norm near the stopping tolerance. Thus the near-perfect binary classification accuracy does not establish numerical convergence. FR with trial step 0.5 yields the lowest reported loss; PR+ with 1 or 0.5 yields the highest reported test accuracy (100%). L-BFGS with 0.5 is the best of its four settings by reported loss and test accuracy. These findings do not establish universal optimizer superiority.

The plateaus in the curves indicate stalled or very small progress; the saved figures alone cannot determine the cause. Clipping/offsets and the implemented gradient can differ from the exact derivative of the stabilized loss at extreme scores; line-search failure also permits zero updates. No validation split or repeated independent runs are reported. Comparing test accuracy across trial steps is descriptive rather than an independent hyperparameter-selection protocol.

## Saved figures

- [FR alpha0 0p05 gradnorm](../results/logistic_regression_optimization/FR_alpha0_0p05_gradnorm.png)
- [FR alpha0 0p05 loss](../results/logistic_regression_optimization/FR_alpha0_0p05_loss.png)
- [FR alpha0 0p1 gradnorm](../results/logistic_regression_optimization/FR_alpha0_0p1_gradnorm.png)
- [FR alpha0 0p1 loss](../results/logistic_regression_optimization/FR_alpha0_0p1_loss.png)
- [FR alpha0 0p5 gradnorm](../results/logistic_regression_optimization/FR_alpha0_0p5_gradnorm.png)
- [FR alpha0 0p5 loss](../results/logistic_regression_optimization/FR_alpha0_0p5_loss.png)
- [FR alpha0 1 gradnorm](../results/logistic_regression_optimization/FR_alpha0_1_gradnorm.png)
- [FR alpha0 1 loss](../results/logistic_regression_optimization/FR_alpha0_1_loss.png)
- [LBFGS m10 alpha0 0p05 gradnorm](../results/logistic_regression_optimization/LBFGS_m10_alpha0_0p05_gradnorm.png)
- [LBFGS m10 alpha0 0p05 loss](../results/logistic_regression_optimization/LBFGS_m10_alpha0_0p05_loss.png)
- [LBFGS m10 alpha0 0p1 gradnorm](../results/logistic_regression_optimization/LBFGS_m10_alpha0_0p1_gradnorm.png)
- [LBFGS m10 alpha0 0p1 loss](../results/logistic_regression_optimization/LBFGS_m10_alpha0_0p1_loss.png)
- [LBFGS m10 alpha0 0p5 gradnorm](../results/logistic_regression_optimization/LBFGS_m10_alpha0_0p5_gradnorm.png)
- [LBFGS m10 alpha0 0p5 loss](../results/logistic_regression_optimization/LBFGS_m10_alpha0_0p5_loss.png)
- [LBFGS m10 alpha0 1 gradnorm](../results/logistic_regression_optimization/LBFGS_m10_alpha0_1_gradnorm.png)
- [LBFGS m10 alpha0 1 loss](../results/logistic_regression_optimization/LBFGS_m10_alpha0_1_loss.png)
- [PRplus alpha0 0p05 gradnorm](../results/logistic_regression_optimization/PRplus_alpha0_0p05_gradnorm.png)
- [PRplus alpha0 0p05 loss](../results/logistic_regression_optimization/PRplus_alpha0_0p05_loss.png)
- [PRplus alpha0 0p1 gradnorm](../results/logistic_regression_optimization/PRplus_alpha0_0p1_gradnorm.png)
- [PRplus alpha0 0p1 loss](../results/logistic_regression_optimization/PRplus_alpha0_0p1_loss.png)
- [PRplus alpha0 0p5 gradnorm](../results/logistic_regression_optimization/PRplus_alpha0_0p5_gradnorm.png)
- [PRplus alpha0 0p5 loss](../results/logistic_regression_optimization/PRplus_alpha0_0p5_loss.png)
- [PRplus alpha0 1 gradnorm](../results/logistic_regression_optimization/PRplus_alpha0_1_gradnorm.png)
- [PRplus alpha0 1 loss](../results/logistic_regression_optimization/PRplus_alpha0_1_loss.png)
- [test accuracy all](../results/logistic_regression_optimization/test_accuracy_all.png)
