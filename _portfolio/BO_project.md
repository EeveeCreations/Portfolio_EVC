---
layout: portfolio
title: "Exploring Multi-Scale and Multi-Nonzero Embedding Strategies in BAxUS"
category: "Research / Machine Learning"
image: "../img/portfolio/BO/BAXUS_f2.png"
description: "A comparative study of embedding strategies—Multi-Scale, Random, and Multi-Nonzero—against the original BAxUS framework for high-dimensional Bayesian optimization."
date: December 2024
---

# Enhancing BAxUS: Exploring Embedding Strategies

**Team:** 2 members  
**Focus:** High-dimensional Bayesian Optimization  
**Date:** October 2024  

---
## Overview

This project investigates several **embedding strategies** designed to enhance the **BAxUS (Bayesian Optimization in Adaptive Subspaces)** framework.  
The goal is to evaluate whether new embedding methods **Multi-Scale Embedding**, **Random Embedding**, and **Multi-Nonzero Embedding** can overcome BAxUS’s limitations in high-dimensional search spaces.

---

## Introduction

In this project, I explored **embedding strategies** to improve the **BAxUS framework** (Bayesian Optimization in Adaptive Subspaces).  

BAxUS is powerful but faces challenges in **high-dimensional optimization** due to its sparse embedding matrices. My goal was to investigate whether alternative strategies could improve exploration, flexibility, and overall performance.  

The strategies I worked with included:  

- **Multi-Scale Embedding**  
- **Random Embedding**  
- **Multi-Nonzero Embedding**  
- **PCA-based Embedding**  
- **KPCA-based Embedding**  

I compared them against the **baseline BAxUS algorithm** on five BBOB benchmark functions: **F1, F8, F12, F15, and F21**, across dimensions **2, 10, and 40**.

---

## Step 1: Setting Up the Experiment

We started by using the original BAxUS as the baseline. The main idea was to see if changing the way the low-dimensional space is embedded into the high-dimensional input space could improve performance.  

I worked closely with my teammate to implement the embedding strategies and run the experiments.  

**Experiment details:**  

- **Benchmark functions:** F1, F8, F12, F15, F21  
- **Dimensions tested:** 2, 10, 40  
- **Instances per function:** 0, 1, 2  
- **Repetitions:** 5 per function-instance  
- **Evaluation budget:** 10 × D per run  
- **Performance metric:** Absolute loss (distance from global optimum)  

---

## Step 2: Observing the Results

### Dimension D = 2
- **Multi-Nonzero Embedding** was very close to BAxUS, and in some cases slightly **outperformed it** (e.g., F12).  
- **Multi-Scale** and **Random Embedding** performed similarly but slightly weaker.  

> **Observation:** Small changes to the embedding matrix can noticeably influence performance.

---

### Dimension D = 10
- **Multi-Nonzero Embedding** outperformed BAxUS on **F8, F12, and F15**.  
- **PCA-based** and **KPCA-based Embeddings** struggled because the low initial target dimensionality failed to capture enough high-dimensional structure.  
- BAxUS still performed best on F1 and F21.  

> **Observation:** Combining multiple non-zero entries per column increases flexibility in high-dimensional search.

---

### Dimension D = 40
- **Multi-Nonzero Embedding** clearly outperformed on **F15 and F21**.  
- **Multi-Scale** and **Random Embedding** also showed competitive results on multimodal functions.  
- **PCA** and **KPCA** embeddings lagged due to low target dimensionality and static matrices.  

> **Observation:** Embedding flexibility is increasingly important in higher dimensions.

---

## Step 3: Analyzing and Reflecting

From these experiments, I could conclude3:  

- **Multi-Nonzero Embedding** is the most promising improvement over BAxUS in high dimensions.  
- **Multi-Scale Embedding** can help, but sparsity issues may appear when the dimension ratio is uneven.  
- **PCA-based** and **KPCA-based Embeddings** need **adaptive updating** during optimization to remain effective.  

A potential enhancement is to **resample and update the embedding matrices dynamically** as optimization progresses, improving adaptability across bins and reducing stagnation.

---

## Step 4: Key Takeaways

- Embedding strategy design is crucial for high-dimensional optimization.  
- Small modifications in embedding matrices can significantly affect exploration and convergence.  
- Flexibility (e.g., multiple non-zero entries) is more impactful than simply increasing dimensionality.  
- This project deepened my understanding of **how theory translates into practical performance improvements** in Bayesian optimization.  

---

## Results and Observations
<img src="/img/portfolio/BO/BAXUS_f2.png" 
     alt="Embedding Results Overview"
     style="width:100%; height:auto; object-fit:cover;">
*Figure 1: Comparison of embedding strategies across BBOB functions.*

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
