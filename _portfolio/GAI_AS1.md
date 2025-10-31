---
layout: portfolio
title: "Adaptive Minecraft Observatory Using GDPC"
authors: "Eefje Karremans"
image: "img/portfolio/GAI/heightmap_A1.jpeg"
description: "Procedural generation of an adaptable cartoon-style observatory in Minecraft using GDPC."
category: "Game AI / Procedural Content Generation"
date: February 2025
permalink: /portfolio/GAI_AS1/
---

# Adaptive Minecraft Observatory Using GDPC

## 1 Introduction

Minecraft is a game celebrated for its adaptability and procedural randomness. This project focuses on the **creative construction aspect** in Minecraft: creating a structure that can adapt to any environment, with randomized variations in each build.  

The goal was to design an **observatory with a high tower and sphere**, inspired by real-life observatories but adapted to a cartoonish style. The structure automatically adjusts to its surroundings, ensuring stability, environmental fit, and aesthetic coherence.

---

## 2 Background

The project uses the **GDPC (Generative Design Python Client)** [GDPC, 2024], combined with **Minecraft Forge**, to manipulate Minecraft worlds programmatically.  

- GDPC allows placing blocks in any position, including floating or environment-adapted placements.  
- Python libraries used: `JSON`, `NumPy`, `random`, and `matplotlib`.  

The combination enables procedural adaptation of structures to terrain and environment.

---

## 3 Idea and Architecture

The observatory is inspired by **real observatories** such as the GST Dome [Wikipedia, 2020] and the **Skinakas Observatory** [Wikipedia, 2025], with a cartoonish style:

- High white tower topped with a sphere.  
- Telescope protruding from the sphere.  
- A small attached house inspired by a **modernized Viking house** with an overhanging roof.  

The structure adapts to its environment:
- Observatories are placed at **highest points**.  
- If the house or tower hovers, a **stability beam** (wood/stone) is added.  
- The design adapts to jungles, mountains, and water areas.

> The aim is a believable, adaptive structure that visually integrates with Minecraft's procedural world.

---

## 4 Methods

### 4.1 Randomized Environment Adaptation

- Randomized elements for **interior and exterior decorations**:
  - Furniture, books, flowers, and colored blocks are randomly selected.  
  - Items with a `facing` attribute are oriented toward the **least obstructed direction**.  

- **Building dimensions** (house extension, tower height, sphere size) are randomized but stay **proportional and believable**.  
- Door and window placement is randomized for visual variety.

### 4.2 Placement

- Structure searches for the **highest point** within a build area.  
- Adapts height differences using terrain scanning to locate **soil or solid ground** (dirt, grass, stone).  
- Special cases:
  - If the build area is in water, a small **island** is constructed first.  
  - Floating extensions use **single pillars** for stability and aesthetics.

### 4.3 Internal and External Decoration

- **Sphere and tower interiors** are filled using a **randomized palette**, with the sphere slightly more decorated than the tower.  
- A central platform in the sphere holds the **telescope lens**; a lectern displays randomized astronomy facts.  
- House interior is randomized with furniture placement, but **a bed is always included**.  
- Complex decorations may vary based on the house dimensions.

### 4.4 Pre-Built Components

Certain elements are pre-built for complexity:
- Telescope lens (quartz and purpur blocks).  
- Staircases and floor patterns.  
- These are stored in **JSON files** with block positions relative to the starting coordinates.  
- Stairs are cleared of blocks before placement and use **biome-appropriate materials** (oak, dark oak, crimson, etc.).

---

## 5 Results

The structure successfully adapts to various environments:

- Placed on mountains, water islands, and mixed terrain.  
- Interior items vary in each generation, ensuring **unique builds**.  
- Stability pillars maintain a believable floating effect.  
- The observatory tower and sphere remain proportionally consistent.

> Figures illustrate variations in observatory placement, interior layout, and environment integration.

---

## 6 Conclusions

- GDPC allows significant **adaptability and randomness**, enabling structures to conform to their environment.  
- Randomizing dimensions and decorations adds variety without breaking believability.  
- Future improvements:
  - Implement **Wave Function Collapse (WFC)** for interior decoration.  
  - Add multi-level tower layers with different interior themes.  
  - Enhance environmental blending (flora, surrounding blocks).

> The project demonstrates that procedural design in Minecraft can create visually engaging, adaptable structures with minimal manual intervention.

---

## References

1. GDPC Contributors. *GDPC Documentation* (2024). URL: [https://gdpc.readthedocs.io](https://gdpc.readthedocs.io/en/stable/index.html)  
2. Wikipedia Contributors. *GST Dome*. (2020). URL: [GST Dome Image](https://upload.wikimedia.org/wikipedia/commons/b/b0/GST_dome.jpg)  
3. Wikipedia Contributors. *Skinakas Observatory - Main Telescope Building*. (2025). URL: [Skinakas Observatory](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e4/Skinakas_observatory_-_main_telescope_building.JPG/1200px-Skinakas_observatory_-_main_telescope_building.JPG)  
4. Freepik. *Isometric Astronomical Observatory Telescope Cartoon Illustration*. (2025). URL: [Freepik Illustration](https://www.freepik.com/premium-vector/isometric-astronomical-observatory-telescope-cartoon-illustration-flat-vector-isolated-object_24786604.htm)  
5. Forge Development Team. *Minecraft Forge 1.19.2*. (2013). URL: [Forge Download](https://files.minecraftforge.net/maven/net/minecraftforge/forge/index_1.19.2.html)
