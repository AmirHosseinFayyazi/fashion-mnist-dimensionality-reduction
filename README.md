# Fashion-MNIST Dimensionality Reduction

A comparative study of dimensionality reduction techniques on the Fashion-MNIST dataset.

This project explores **PCA, LDA, t-SNE, and UMAP** and investigates how different dimensionality reduction methods represent high-dimensional image data.

---

## Project Goal

The main goal of this project is to understand the differences between several dimensionality reduction techniques by applying them to the same dataset.

Instead of only implementing the algorithms, the project focuses on:

* Understanding what each method tries to preserve.
* Visualizing high-dimensional image data in 2D.
* Comparing the resulting representations.
* Investigating PCA image reconstruction using different numbers of components.

---

## Dataset

The project uses the **Fashion-MNIST** dataset.

Fashion-MNIST contains grayscale images of clothing items.

Each image has:

* Size: `28 × 28` pixels
* Features after flattening: `784`
* Classes: `10`
* Training samples: `60,000`
* Test samples: `10,000`

The classes are:

| Label | Class       |
| ----- | ----------- |
| 0     | T-shirt/top |
| 1     | Trouser     |
| 2     | Pullover    |
| 3     | Dress       |
| 4     | Coat        |
| 5     | Sandal      |
| 6     | Shirt       |
| 7     | Sneaker     |
| 8     | Bag         |
| 9     | Ankle boot  |

For the dimensionality reduction experiments, a subset of the training data was used.

---

## Data Preprocessing

The images initially have the shape:

```text
(samples, 28, 28)
```

They were flattened into 784-dimensional feature vectors:

```text
(samples, 784)
```

The pixel values were also scaled from the original range:

```text
0 - 255
```

to:

```text
0 - 1
```

This produces a high-dimensional representation where each image is a point in:

```text
R^784
```

---

# Methods

## 1. Principal Component Analysis (PCA)

**PCA** is an unsupervised linear dimensionality reduction method.

The main idea of PCA is to find directions in the data along which the variance is maximized.

The first principal component captures the largest possible variance, the second captures the largest remaining variance while being orthogonal to the first, and so on.

In this project, PCA was used for:

1. Reducing the 784-dimensional images to 2 dimensions.
2. Measuring explained variance.
3. Studying principal components.
4. Reconstructing images using different numbers of components.

### 2D Visualization

The data was reduced from:

```text
784D → 2D
```

and visualized using the class labels.

### Explained Variance

The explained variance ratio was examined to determine how much of the original variance is retained by the principal components.

For example:

```python
pca.explained_variance_ratio_
```

and the cumulative explained variance can be calculated using:

```python
pca.explained_variance_ratio_.sum()
```

### PCA Image Reconstruction

One of the main experiments of the project was reconstructing an image using different numbers of principal components.

The reconstruction follows the idea:

```text
x ≈ mean + z₁PC₁ + z₂PC₂ + ... + zₙPCₙ
```

where:

* `PCᵢ` is a principal component direction.
* `zᵢ` is the coordinate/score of the image along that direction.
* `mean` is the mean image.

The same image was reconstructed using different numbers of components, such as:

```text
1
5
10
20
50
100
```

This demonstrates the trade-off between:

```text
Number of components
        ↓
Representation size
        ↓
Reconstruction quality
```

With more components, more information from the original image is retained and the reconstruction becomes closer to the original.

---

## 2. Linear Discriminant Analysis (LDA)

**LDA** is a supervised dimensionality reduction technique.

Unlike PCA, LDA uses the class labels.

Its objective is to find directions that provide good separation between different classes.

Therefore, LDA focuses on:

```text
Between-class separation
```

while considering:

```text
Within-class variation
```

Since Fashion-MNIST contains 10 classes, the maximum number of LDA components is:

```text
C - 1 = 10 - 1 = 9
```

For visualization, the first two LDA components were used:

```text
784D → 2D
```

This allows the class separation produced by LDA to be compared with PCA.

---

## 3. t-SNE

**t-SNE** is a nonlinear dimensionality reduction technique primarily designed for visualization.

Its main goal is to preserve **local neighborhoods**.

In other words, points that are close to each other in the original high-dimensional space are encouraged to remain close in the low-dimensional representation.

The Fashion-MNIST data was embedded into:

```text
784D → 2D
```

and visualized using the class labels.

t-SNE is particularly useful for investigating whether local structures and groups of similar samples become visible in a 2D representation.

Unlike PCA, t-SNE does not provide a PCA-style reconstruction from the low-dimensional representation back to the original 784-dimensional space.

---

## 4. UMAP

**UMAP** is another nonlinear dimensionality reduction technique.

Like t-SNE, UMAP is useful for visualizing high-dimensional data.

UMAP primarily preserves local structure while generally retaining more information about the global structure than t-SNE.

The data was reduced from:

```text
784D → 2D
```

and visualized to examine the structure of the Fashion-MNIST classes.

---

# Comparison

The main conceptual difference between the methods can be summarized as follows:

| Method | Type                     | Main Objective                                     |
| ------ | ------------------------ | -------------------------------------------------- |
| PCA    | Linear / Unsupervised    | Maximize variance                                  |
| LDA    | Linear / Supervised      | Maximize class separation                          |
| t-SNE  | Nonlinear / Unsupervised | Preserve local neighborhoods                       |
| UMAP   | Nonlinear / Unsupervised | Preserve local structure and some global structure |

The methods therefore answer different questions.

### PCA

> What directions contain the most variance?

### LDA

> What directions separate the known classes best?

### t-SNE

> Which samples have similar local neighborhoods?

### UMAP

> What local and broader structure exists in the high-dimensional data?

---

# Important Observation

A 2D visualization should not automatically be interpreted as saying that one method is universally "better" than another.

Each method optimizes a different objective.

For example:

* PCA is useful when variance and linear structure are important.
* LDA is useful when class separation is the objective and labels are available.
* t-SNE is useful for examining local neighborhoods and visual clusters.
* UMAP is useful for exploring both local structure and broader relationships.

Therefore, the appropriate dimensionality reduction method depends on the purpose of the analysis.

---

# Project Structure

```text
fashion-mnist-dimensionality-reduction/
│
├── README.md
├── requirements.txt
├── .gitignore
│
└── notebooks/
    └── dimensionality_reduction.ipynb
```

---

# Technologies and Libraries

The project was implemented using Python and the following libraries:

* NumPy
* Pandas
* Matplotlib
* Scikit-learn
* TensorFlow / Keras
* UMAP-learn

---

# Running the Project

Clone the repository and create a virtual environment:

```bash
python -m venv .venv
```

Activate the environment on Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Then open:

```text
notebooks/dimensionality_reduction.ipynb
```

and run the notebook cells.

---

# Main Questions Investigated

This project was built around the following questions:

1. How does PCA represent Fashion-MNIST in a low-dimensional space?
2. How much variance can PCA preserve?
3. How does the number of PCA components affect image reconstruction?
4. How does supervised LDA differ from unsupervised PCA?
5. How well can LDA separate the Fashion-MNIST classes?
6. What local structures become visible using t-SNE?
7. How does UMAP represent the same dataset?
8. Why do the 2D representations produced by these methods look different?
9. What information is preserved or lost when reducing 784 dimensions to 2?

---

# Conclusion

This project demonstrates that dimensionality reduction is not a single problem with a single optimal solution.

Different methods preserve different properties of the original data:

```text
PCA
│
└── Variance / Linear structure

LDA
│
└── Class separation / Supervised structure

t-SNE
│
└── Local neighborhoods

UMAP
│
└── Local + broader structure
```

The experiments show why selecting a dimensionality reduction technique should depend on the objective of the analysis rather than simply choosing the method that produces the most visually separated 2D plot.

---

## Author

Computer Engineering student interested in Machine Learning and Artificial Intelligence.
