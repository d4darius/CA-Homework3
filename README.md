# Computational Algebra Project

*Singular Value Decomposition* used to device a Movie recommendation system

In the following homework we decided to explore the topic of *Singular Value Decomposition* used to device a Movie recommendation system like the one used nowdays by many streaming services.

## Dataset Specifications
The dataset we decided to use is the *MovieLens Dataset* which contains 100000 user ratings for 9000 movies along with metadata like movie genres, titles and timestamps.

In particular, we considered the _MovieLens 100K_ dataset. Data are distributed into:
- "ratings.csv": which contains all the ratings
- "movies.csv": which contains all the movies information

### Ratings dataset
All ratings are contained in the file "ratings.csv" and are characterized by the following attributes: 
- *userId*
- *movieId*
- *rating*
- *timestamp*

Ratings are made on a 5-star scale, with half-star increments (0.5 stars - 5.0 stars).

### Movies dataset
Movie information is in the file "movies.csv" and is expressed in the following format: 

MovieID::Title::Genres

- Titles are identical to titles provided by the IMDB (including
year of release)
- Genres are pipe-separated and are selected from the following genres:

	* Action
	* Adventure
	* Animation
	* Children's
	* Comedy
	* Crime
	* Documentary
	* Drama
	* Fantasy
	* Film-Noir
	* Horror
	* Musical
	* Mystery
	* Romance
	* Sci-Fi
	* Thriller
	* War
	* Western

# Singular Value Decomposition (SVD) - Theory

SVD is a matrix factorization technique for a matrix $A \in \mathbb{C}^{m \times n}$, expressed as:

$A = U \Sigma V^H$

where:
- $U$ is an orthogonal $m \times m$ matrix with left singular vectors,
- $\Sigma$ is a diagonal $m \times n$ matrix with non-negative singular values,
- $V$ is an orthogonal $n \times n$ matrix with right singular vectors.

Key properties:
- The columns of $U$ are eigenvectors of $A A^T$.
- The columns of $V$ are eigenvectors of $A^T A$.
- The diagonal elements of $\Sigma$ are the singular values, corresponding to the square roots of the eigenvalues of $A A^T$ or $A^T A$.
If $A \in \mathbb{R}^{m \times n}$, the decomposition becomes: $A = U \Sigma V^T$.

### Geometric Interpretation

The SVD decomposes the transformation represented by any matrix into a sequence of three operations:
1. **Rotation by $V^T$**: Aligns the input space with the right singular vectors.
2. **Scaling by $\Sigma$**: Scales along axes defined by the singular vectors.
3. **Rotation by $U$**: Rotates the output space to align with the left singular vectors.

This decomposition helps in understanding and visualizing linear transformations.

## Weighted SVD (WSVD) for Handling Missing Values in Recommender Systems

**Weighted Singular Value Decomposition (WSVD)** is an extension of standard **SVD** that explicitly accounts for missing values by introducing a weight matrix. This approach avoids bias from simple mean imputation while still leveraging matrix factorization for recommendations.

#### **1. How Does Weighted SVD Work?**
Instead of treating missing values as zeros or averages, WSVD assigns **weights** to known and unknown values:

- **Observed ratings (known values):** Weight = 1  
- **Missing ratings (unknown values):** Weight = 0

1. Define the weight matrix $W$

2. Set the missing values of $A$ to 0 to compute the first approximation.

3. Iteratively refine the imputation of missing values:

   1. Compute the rank- $k$ approximated matrix  
    $$A_k = U_k \cdot S_k \cdot V_k^\top$$

   2. Update Missing Values:  
      $$A_{k+1} = W \cdot A + (1 - W) \cdot A_k$$
      $A_{k+1}$ keeps the known values from $A$ unchanged and updates the missing values by substituting them with the entries of $A_k$.

   3. Check for convergence: stop if $$\| A_{k+1} - A_k \| < \text{relTol}$$ or if the maximum number of iterations $maxIter$ is reached.
## Performing predictions
To perform a prediction of the rating of a movie $y$ from a user $x$ the procedure adopted is the following:
1. We select the matrices relative to the genres of the movie $y$ out of the training dataset
2. We perform WSVD on the obtained matrix
3. We assign a similarity to each user with respect to $x$, in particular we use the *cosine similarity*
$$cos(x,\={x}) = \frac{x\cdot \={x}}{||x||||\={x}||}$$ 
4. We use a KNN (k-nearest neighbors) approach to predict the rating. We retrieve the top $k$ most similar users to $x$ from the similarity matrix and predict the new rating according to a weighted average:

$$\text{weightedRating} = \frac{\sum(\text{similarities} \times \text{ratings})}{\sum\text{similarities}}$$

To evaluate the predictions $\={r}$ made by our model we compute the **RMSE** w.r.t. the actual ratings given by the users $r$ as:
$$RMSE = \sqrt{\sum_{i=1}^n\frac{(\={r}-r)^2}{n}}$$

## Conclusions
In this report, we develop and evaluate a movie recommendation system using Weighted Singular Value Decomposition (WSVD), combined with k nearest neighbors (KNN) for personalized rating prediction. We demonstrated how the system could effectively handle missing values by leveraging WSVD, which allows for dimensionality reduction and a better representation of the latent factors in the user-item interaction matrix.
The results showed a good alignment with actual user ratings, highlighting the system's effectiveness in capturing user preferences. 

We also compared the WSVD-based model with a simpler approach that used mean imputation for missing values. The results confirmed that the WSVD approach outperforms the mean imputation model. 

In conclusion, the combination of WSVD and KNN provides a robust solution for building recommendation systems, particularly for large-scale datasets. Future work could explore further optimizations in matrix factorization techniques or incorporate additional features, such as user demographics or movie metadata, to further improve the accuracy of the model.

### Collaborators
- Maretto Chiara: s339401
- Gosmar Dario: s337625
