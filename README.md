# ⚽ Football Player Profiling using Unsupervised Learning

## 📌 Project Overview

This project applies **unsupervised machine learning** to identify meaningful football player profiles based on their playing attributes.

Instead of using predefined player positions as labels, the project discovers naturally occurring groups of players using their **technical, attacking, defensive, physical, and goalkeeping attributes**.

The complete workflow includes:

**EDA → Feature Selection → Standardization → PCA → Clustering → Model Comparison → Validation → Statistical Testing → Cluster Profiling**

The final clusters are interpreted as meaningful football player archetypes such as **Attacking Players, Goalkeepers, and other player profiles**.

---

## 🎯 Objectives

- Discover naturally occurring groups of football players.
- Identify player archetypes using multidimensional skill attributes.
- Reduce dimensionality while preserving most of the information.
- Compare different clustering approaches.
- Determine an appropriate number of clusters.
- Validate cluster stability and separation.
- Statistically test whether the discovered clusters differ.
- Interpret the resulting clusters in football terms.

---

## 📊 Dataset

The dataset contains approximately:

- **17,954 players**
- **51 variables**
- **29 football skill attributes used for clustering**

The dataset contains player-level information related to:

- Attacking
- Shooting
- Passing
- Dribbling
- Defending
- Physical attributes
- Goalkeeping
- Overall rating
- Potential
- Age
- Market value

---

## 🧠 Feature Selection Strategy

The clustering model was primarily based on **football skill attributes**.

Variables such as:

- `Overall Rating`
- `Potential`
- `Market Value`

were not directly used as clustering features.

### Why?

These variables are better treated as **evaluation or interpretation variables** rather than primary clustering inputs.

For example:

- **Overall rating** is a composite assessment of player ability.
- **Potential** represents expected future development.
- **Market value** is influenced by factors beyond playing ability, such as age, reputation, contracts, demand, and league.

Using them directly could cause the clusters to reflect rating or economic differences rather than distinct playing profiles.

---

# 🔍 Exploratory Data Analysis

EDA was performed before clustering to understand the structure and quality of the data.

The analysis included:

- Dataset dimensions
- Data types
- Missing-value analysis
- Duplicate analysis
- Numerical feature distributions
- Feature ranges
- Correlation analysis
- Identification of highly correlated variables

EDA was particularly important because clustering algorithms are sensitive to:

- Feature scale
- Correlation
- Irrelevant variables
- Outliers

---

# ⚙️ Data Preprocessing

## 1. Feature Scaling

The selected numerical attributes were standardized using `StandardScaler`.

The transformation is:

\[
z = \frac{x-\mu}{\sigma}
\]

where:

- \(x\) = original feature value
- \(\mu\) = feature mean
- \(\sigma\) = feature standard deviation

### Why Standardization?

The clustering algorithm is distance-based.

Without scaling, a feature with a larger numerical range could contribute disproportionately to the Euclidean distance.

Standardization places the variables on a comparable scale.

---

# 📉 Principal Component Analysis

After standardization, **Principal Component Analysis (PCA)** was applied.

### Why PCA?

The original football attributes are highly correlated.

PCA was used to:

- Reduce dimensionality
- Remove redundant information
- Reduce multicollinearity
- Improve computational efficiency
- Create a compact representation for clustering

The final representation retained **8 principal components**, capturing approximately **90% of the total variance**.

### Important Point

PCA does **not** select the eight most important original features.

Instead, it creates eight new variables, where each principal component is a linear combination of the original football attributes.

---

# 🤖 Clustering

Multiple clustering algorithms were considered:

- **K-Means**
- **Agglomerative Clustering**
- **Gaussian Mixture Model (GMM)**

## Final Approach: K-Means

K-Means was selected as the primary clustering approach because:

- The variables are numerical.
- PCA produces a suitable continuous feature space.
- Euclidean distance is appropriate after standardization.
- K-Means is computationally efficient.
- Cluster centroids are easy to interpret.
- The resulting player profiles can be clearly summarized.

The K-Means objective is:

\[
\min \sum_{k=1}^{K}\sum_{x_i\in C_k}
\|x_i-\mu_k\|^2
\]

where:

- \(C_k\) = cluster \(k\)
- \(\mu_k\) = centroid of cluster \(k\)

---

# 🔢 Selecting the Number of Clusters

The number of clusters was not selected arbitrarily.

Candidate values of \(K\) were evaluated using clustering diagnostics such as:

- Elbow method
- Silhouette analysis
- Cluster stability
- Domain interpretability

The final solution was selected by considering **both quantitative clustering quality and football interpretability**.

---

# 📏 Cluster Validation

A major focus of the project was validating whether the discovered clusters were meaningful.

Instead of relying on a single metric, multiple complementary validation methods were used.

---

## 1. Silhouette Score

The final clustering achieved a silhouette score of approximately:

### **0.298**

The silhouette coefficient is:

\[
s(i)=
\frac{b(i)-a(i)}
{\max(a(i),b(i))}
\]

where:

- \(a(i)\) = average distance to observations within the same cluster
- \(b(i)\) = minimum average distance to observations in another cluster

A value close to 1 indicates strong separation, while values close to 0 indicate overlapping clusters.

### Interpretation

A score of **0.298 indicates moderate cluster separation**.

This is reasonable for football data because player abilities exist on continuous dimensions. Players can naturally share characteristics with multiple playing styles.

Therefore, the silhouette score was interpreted together with the other validation techniques rather than in isolation.

---

# 2. Hopkins Statistic

The Hopkins statistic was approximately:

### **0.7903**

The Hopkins statistic evaluates whether the dataset contains a tendency toward clustering.

Values closer to 1 indicate stronger evidence of non-random cluster structure.

A value of approximately **0.79** therefore supports the use of clustering techniques on the dataset.

---

# 3. Adjusted Rand Index

Cluster stability was evaluated using the **Adjusted Rand Index (ARI)**.

The obtained ARI values were approximately:

### **0.994 – 1.000**

ARI measures the similarity between two cluster assignments while correcting for agreement expected by chance.

A value close to 1 indicates extremely high agreement.

### Interpretation

The very high ARI values indicate that the discovered clusters were **highly stable across repeated clustering solutions**.

This is particularly useful because clustering can sometimes be sensitive to initialization.

---

# 📊 Why Silhouette and ARI Tell Different Stories

An important observation from the project is that:

- Silhouette ≈ **0.298**
- ARI ≈ **0.994–1.000**

These values are not contradictory.

They measure different properties.

### Silhouette

Measures:

> How well-separated are the observations geometrically?

### ARI

Measures:

> How similar or stable are the resulting cluster assignments?

Therefore, clusters can have moderate overlap in feature space while still being highly reproducible.

---

# 🧪 Statistical Validation

Statistical testing was performed to determine whether the discovered clusters were significantly different.

## Kruskal-Wallis Test

The Kruskal-Wallis test was used as a non-parametric alternative to one-way ANOVA.

The test statistic was:

### **H = 4536.9518**

with:

### **p < 0.001**

Therefore, the null hypothesis that the cluster distributions are identical was rejected.

This provides strong evidence that the clusters differ significantly across the evaluated player attributes.

---

# 🔬 Dunn's Post-Hoc Test

The Kruskal-Wallis test establishes that differences exist, but it does not identify which cluster pairs differ.

Therefore, **Dunn's post-hoc test** was applied.

The pairwise comparisons showed significant differences between the cluster pairs after multiple-comparison adjustment.

This provides additional evidence that the clusters are not simply arbitrary partitions.

---

# 👥 Cluster Profiling

After obtaining the cluster assignments, the clusters were profiled using the original football attributes.

Additional variables such as:

- Overall rating
- Potential
- Age
- Market value

were used for interpretation rather than for generating the clusters.

An example of the final cluster summary is:

| Cluster | Profile | Players | Avg. Rating | Avg. Potential | Avg. Age | Avg. Value |
|--------:|---------|--------:|------------:|---------------:|---------:|-----------:|
| 0 | Attacking Players | 5,149 | 64.06 | 70.89 | 23.98 | 1,285,677 |
| 1 | Goalkeepers | 2,071 | 64.45 | 69.81 | 26.44 | 1,622,328 |

> The complete final cluster table should be generated directly from the final notebook output.

---

17,954 Players
        ↓
29 Football Attributes
        ↓
StandardScaler
        ↓
PCA — 8 Components
        ↓
~90% Variance Retained
        ↓
K-Means Clustering
        ↓
Hopkins = 0.7903
        ↓
Silhouette = 0.298
        ↓
ARI = 0.994–1.000
        ↓
Kruskal-Wallis H = 4536.9518
        ↓
p < 0.001
        ↓
Interpretable Football Archetypes
