# Data access

Only the binary logistic regression experiment requires an external dataset: MNIST handwritten digits. The notebook first attempts `tensorflow.keras.datasets.mnist.load_data()`, then `sklearn.datasets.fetch_openml("mnist_784", version=1)`, then a local `mnist.npz` containing `x_train`, `y_train`, `x_test`, `y_test`.

- [MNIST via Keras](https://keras.io/api/datasets/mnist/)
- [MNIST on OpenML](https://www.openml.org/d/554)

Run the notebooks with `notebooks/` as their working directory. If using the offline fallback, place `mnist.npz` in `notebooks/` (the original loader expects this location). The dataset is not redistributed in this repository. Other experiments use analytical objectives or synthetic points defined in code.
