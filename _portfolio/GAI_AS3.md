---
layout: portfolio
title: "Generating DOOM Levels Using Wave Function Collapse"
authors: "Ruzanna Baghdasaryan, Eefje Karremans, Kasper Notebomer, Stan Ruijters, Ryan Sleeuwaegen, Abygail Stegenga"
image: "img/portfolio/GAI/doomgen_A3.png"
description: "Procedural generation of DOOM levels using Wave Function Collapse (WFC)."
category: "Game AI / Wave Function Collapse / Generative AI"
date: October 29, 2025
permalink: /portfolio/GAI_AS3/
---

# Generating DOOM Levels Using Wave Function Collapse

## 1 Introduction

DOOM, released in 1993, is one of the earliest first-person shooter games. Many original maps are available online and in text format. Procedural content generation (PCG) allows automatic creation of game levels, reducing manual design effort.  

While Generative Adversarial Networks have been used to generate realistic DOOM levels, Wave Function Collapse (WFC) has not yet been applied. This project implements a **procedural DOOM map generator using WFC** that respects the structure of original levels.  

The pipeline involves:
1. Preprocessing maps to extract essential spatial tiles.
2. Identifying local patterns to guide WFC.
3. Applying structural repair to ensure connectivity and playability.
4. Placing gameplay elements and converting output into `.WAD` files for the game.
5. Evaluating the generated levels using metrics and user studies.

---

## 2 Methodology

The generator uses the **Overlapping Wave Function Collapse algorithm** [Gumin, 2016] to synthesize playable DOOM maps.

### 2.1 Preprocessing

- **Goal:** Retain only structural tiles.
- **Tiles used:**  
  - `X`: wall  
  - `.`: floor  
  - `-`: out-of-bounds  
- **Removed:** enemies, start, exit, etc.  
- **Purpose:** WFC focuses purely on map layout patterns.

### 2.2 Pattern Extraction and Rule Construction

- A **sliding window** of size N×N moves across all maps.  
- Each window yields a **pattern** representing a small local layout.  
- Duplicate patterns are merged, and their **frequencies** are stored.  
- **Adjacency rules** are computed: patterns can only neighbor if bordering rows/columns match.

> This forms a statistical model of map structure: pattern vocabulary, weights, and legal transitions.

### 2.3 Overlapping Wave Function Collapse

- **Initialization:** Grid cells start in a superposition of all possible patterns.  
- **Collapse:** Iteratively select the cell with **lowest entropy** and assign a pattern based on weights.  
- **Constraint propagation:** Update neighboring cells to respect adjacency rules.  
- **Output:** Bare map layout with walls, floors, and out-of-bounds tiles.

**Entropy formula:**

{% raw %}
$$
H = - \sum_{x \in X} p(x) \log p(x)
$$
{% endraw %}

Where \(p(x)\) is the probability of each pattern at a given cell.

### 2.4 Structural Repair

Generated maps may contain disconnected rooms or isolated tiles. Repairs include:

1. **Removing small rooms:** Floors < 12 tiles filled in with walls.  
2. **Sealing map boundaries:** Floor tiles adjacent to out-of-bounds are converted to walls.  
3. **Pruning isolated walls:** Remove walls with no adjacent floor tiles.  
4. **Connecting disjoint rooms:** Use Manhattan distances + Kruskal’s algorithm to create corridors.

> After repair, maps are fully connected, enclosed, and nearly playable.

### 2.5 Gameplay Element Placement

- Identify two furthest points on floor tiles for **player spawn** and **exit**.  
- Randomly place enemies, weapons, health packs, and explosives, avoiding overcrowding.  
- Ensures clear pathways and balanced gameplay.

### 2.6 Text-to-Game Conversion

- Generated `.txt` maps are converted to `.WAD` files.  
- Map sections (walls, floor, ceiling, entities) assigned using DOOM’s **THINGS** and **SECTOR** lumps.  
- Textures chosen randomly from original maps to maintain consistency.

---

## 3 Evaluation

### 3.1 Metrics

1. **Structural Similarity:** Shannon entropy difference between generated and original maps.  
2. **Smoothness:** Number of corners in the layout.  
3. **Interactability:** Relative number of items per walkable tile.  
4. **Killability:** Number of enemies per ammunition.  
5. **Survivability:** Health packs per enemy.

### 3.2 Playability Study

- 10 participants tested 18 maps (9 generated, 9 original).  
- Metrics computed: Accuracy, Precision, Recall, F1-score.  
- Users familiar with DOOM performed better; unfamiliar players struggled to distinguish maps.  

**Accuracy formula:**

{% raw %}
$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
$$
{% endraw %}

Other metrics: Precision, Recall, F1-score follow standard definitions.

---

## 4 Results & Discussion

- Generated maps are structurally similar to original maps (entropy close to zero).  
- **Smoothness:** Slightly higher number of corners, resulting in angular layouts.  
- **Interactability/Killability/Survivability:** Balanced according to map size.  
- Playtests confirmed **WFC-generated maps are challenging but playable**, especially for new players.

### Observations

- Angular corridors and small hallways indicate procedural generation.  
- Item placement can be improved for better intuitiveness.  
- Texture glitches were minor and did not affect gameplay.  

---

## 5 Conclusion

This project demonstrates that **Wave Function Collapse** can generate DOOM levels that are structurally coherent, playable, and engaging.  
Future improvements may include:
- Smoother map layouts.  
- Verticality in levels.  
- Optimized item/enemy placement.  
- Reduction of visual artifacts.  

The study confirms the viability of WFC for procedural content generation in games.

---

## References

1. Adam James Summerville et al. *The VGLC: The Video Game Level Corpus*. 2016. [arXiv:1606.07487](https://arxiv.org/abs/1606.07487)  
2. Edoardo Giacomello, Pier Luca Lanzi, and Daniele Loiacono. *DOOM Level Generation using Generative Adversarial Networks*, IEEE GEM, 2018.  
3. Maxim Gumin. *Wave Function Collapse Algorithm*. [GitHub](https://github.com/mxgmn/WaveFunctionCollapse), 2016.
