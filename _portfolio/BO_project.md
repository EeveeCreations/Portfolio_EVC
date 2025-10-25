---
layout: portfolio
title: "Exploring Multi-Scale and Multi-Nonzero Embedding Strategies in BAxUS"
category: "Research / Machine Learning"
image: "/img/3.jpg"
permalink: /portfolio/baxus-embedding/
description: "A comparative study of embedding strategies—Multi-Scale, Random, and Multi-Nonzero—against the original BAxUS framework for high-dimensional Bayesian optimization."
---

## Overview

This project investigates several **embedding strategies** designed to enhance the **BAxUS (Bayesian Optimization in Adaptive Subspaces)** framework.  
The goal is to evaluate whether new embedding methods **Multi-Scale Embedding**, **Random Embedding**, and **Multi-Nonzero Embedding** can overcome BAxUS’s limitations in high-dimensional search spaces.

---

## Experiment Summary
For this Project I worked in a team of  2. We  used the BAxUS algorithm as baseline, as the combionatiosn used in this paper inspired us to  see what elsec ould be used to enhance its capability.
1. **BAxUS (baseline)**  
2. **Multi-Scale Embedding**  
3. **Random Embedding**  
4. **Multi-Nonzero Embedding**  
5. **PCA-based Embedding**  
6. **KPCA-based Embedding**

Experiments were conducted on five BBOB benchmark functions (F1, F8, F12, F15, F21) across dimensions **2, 10, and 40**.

---

## Results and Observations

![Embedding Results Overview](/img/portfolio/BO/BAXUS_f2.png)
### Dimension D = 2
- **Multi-Nonzero Embedding** achieved performance close to **BAxUS**.
- In some cases (F12), Multi-Nonzero slightly **outperformed BAxUS** due to increased variation across search directions.
- **Multi-Scale** and **Random Embedding** showed similar but slightly weaker results.

> **Figure 2:** Loss values for 2D functions. BAxUS converges quickly on F1; Multi-Nonzero achieves similar or better performance on F12.

---

### Dimension D = 10
- **Multi-Nonzero Embedding** outperformed BAxUS on **F8, F12, and F15**, benefiting from its ability to combine multiple variables for flexible search directions.
- **PCA-based** and **KPCA-based Embeddings** performed poorly due to insufficient capture of high-dimensional features.
- BAxUS still performed best on **F1** and **F21**.

> **Figure 3:** For D=10, BAxUS remains strong on F1 and F21, while Multi-Nonzero dominates F8, F12, and F15.

---

### Dimension D = 40
- **Multi-Nonzero Embedding** outperformed all others on **F15** and **F21**.
- **Multi-Scale Embedding** and **Random Embedding** also performed competitively on multimodal functions.
- **PCA-based** and **KPCA-based** methods again failed to scale effectively due to low target dimensionality.

> **Figure 4:** In 40D, Multi-Nonzero and Multi-Scale Embedding show significant advantages in higher dimensions.

---

## Discussion

The experiments reveal that:
- **Multi-Nonzero Embedding** is the most promising improvement over BAxUS, particularly in high dimensions.
- **Multi-Scale Embedding** provides benefits but may introduce sparsity issues when the dimensionality ratio is uneven.
- **PCA-based** and **KPCA-based** methods require adaptive updating to remain effective during progressive optimization.

A potential enhancement is to **resample and update embedding matrices** dynamically as optimization progresses—improving adaptability across bins and reducing stagnation.

---

## Conclusion

- Multi-Scale, Random, and especially **Multi-Nonzero Embedding** outperform BAxUS on certain benchmark functions, demonstrating their potential to improve high-dimensional Bayesian optimization.
- However, each has trade-offs regarding scalability, computational cost, and adaptability.
- **Future work** includes dynamic resampling of embeddings and testing on additional BBOB or real-world datasets.

---

## References

1. Leonard Papenmeier, Luigi Nardi, and Matthias Poloczek. *Increasing the Scope as You Learn: Adaptive Bayesian Optimization in Nested Subspaces*, 2023.  
2. Ziyu Wang et al. *Bayesian Optimization in a Billion Dimensions via Random Embeddings*, 2016.  
3. Kirill Antonov et al. *High Dimensional Bayesian Optimization with Kernel PCA*, 2022.  
4. Nikolaus Hansen et al. *COCO: A Platform for Comparing Continuous Optimizers*, *Optimization Methods and Software*, 2020.  
5. Carola Doerr et al. *IOHProfiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics*, 2018.  
6. Maria Laura Santoni et al. *Comparison of High-Dimensional Bayesian Optimization Algorithms on BBOB*, 2024.  
7. Balandat. *BO with BAxUS and TS/EI*, [GitHub Repository](https://github.com/Balandat/botorch/blob/3f2e2c7572ff41c9cfeeb6dea7a55776b238dc03/tutorials/baxus.ipynb#L7), 2024.

---

**Keywords:** Bayesian Optimization, BAxUS, Embedding Strategies, Multi-Scale Embedding, Random Embedding, Multi-Nonzero Embedding, PCA, KPCA
