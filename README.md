# check_shapes

Easily check shapes of arrays and arraylikes with a simple decorator

```python
import numpy as np

from check_shapes import args


# Supports symbolic/dynamic axis dimensions
@args(X=("n", "p"), y=("n",)).returns(("p",))
def calculate_regression_coefficients(X: np.ndarray, y: np.ndarray) -> np.ndarray:
    return np.linalg.inv(X.T @ X) @ X.T @ y


# Specify the exact dimensions of inputs and outputs
@args(X=(3, 3)).returns((3, 3))
def expects_3_3(X: np.ndarray) -> np.ndarray:
    return X


# X has shape (3, 3) and y has shape (3,)
X = np.array([[1, 2, 4], [4, 5, 5], [7, 8, 10]])
y = np.array([1, 2, 4])

b = calculate_regression_coefficients(X, y)
print(b)

# Here, W has shape (4, 3)
W = np.array([[1, 2, 4], [4, 5, 5], [7, 8, 10], [1, 2, 4]])

# This raises an IncompatibleDimensionError
a = calculate_regression_coefficients(W, y)

# Can also specify axis dimensions precisely
expects_3_3(X)

# This raises an error
expects_3_3(W)
```

Why `check_shapes`? There's usually an error anyways.

* Sometimes there isn't an error.
* Ability to enforce exact or (coming soon) ranges of dimensions with a simple decorator.
* Catch incompatible shapes immediately, before any expensive intermediate calculation.
* Make your code self-documenting.