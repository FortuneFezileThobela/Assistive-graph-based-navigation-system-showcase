# Assistive Graph-Based Navigation System for Blind Pedestrians Using Tactile Paving Recognition

An assistive pedestrian navigation system that pairs computer vision **tactile paving recognition** with **graph-based spatial modeling** to help blind and visually impaired pedestrians navigate walkways safely and independently.

> **Repository Notice:** This public repository showcases the system architecture, image-to-graph pipeline, and demonstration video. The underlying source code is kept in a private repository—please reach out via [your email / LinkedIn] for a technical walkthrough or collaboration inquiries.

---

## System Demo

[![Watch the Assistive Navigation System Demo](https://img.youtube.com/vi/dMtqbKz7Xk8/hqdefault.jpg)](https://www.youtube.com/watch?v=dMtqbKz7Xk8)

*Click the preview image above to watch the full demonstration on YouTube.*

---

## System Pipeline & Architecture

The system transforms raw camera imagery of tactile ground surface indicators into a structured, navigable graph through four main stages:

### 1. Pre-Extraction & Data Ingestion
* **Dataset & Annotation Storage:** Uses a Roboflow dataset annotated in **COCO JSON** format (bounding boxes, category IDs, and segmentation masks).
* **SQLite Indexing:** Annotations, category IDs, and segmentation data are extracted from COCO JSON into a lightweight **SQLite** database for fast querying inside the Java application.
* **In-Memory Lookup:** At startup, the Java backend loads the database into a hash dictionary keyed by `image_id` for low-latency retrieval during processing.

### 2. Machine Learning Detection *(Group Module — Placeholder)*
* *(Placeholder for your teammate's ML detection/segmentation model details—e.g., model architecture, tactile tile classes detected, and inference output.)*

### 3. Image Processing & Vertex Extraction
Each detected region is isolated using its bounding box and processed independently through a three-stage computer vision pipeline:
1. **Preprocessing & Ray-Casting Mask:** To eliminate background noise inside each bounding box, **ray casting** generates a 2D virtual polygonal mask isolating the segmented tactile surface.
2. **Stacked Noise & Edge Filtering:**
   * **Grayscale Conversion:** Collapses RGB channels into a single intensity channel.
   * **Gaussian Blur:** Suppresses high-frequency sensor and surface texture noise to prevent false positives.
   * **Scharr Edge Detection:** Computes image gradients and applies binary thresholding (`gradient > 0.3` → White, otherwise Black).
3. **Recursive Peak Finding & Uniqueness Validation:**
   * Scans bright pixels and applies **8-neighbor recursion**, climbing toward local maximum brightness so each vertex centers accurately on the tactile feature.
   * Enforces a minimum spatial separation of **5 units** between detected points to prevent duplicate vertices on the same feature.

### 4. Adjacency List Graph Construction
Validated coordinates are inserted into an **Adjacency List** graph representing walkable tactile structure:
* **Vertex Insertion:** Each unique coordinate becomes a node in the spatial graph.
* **Edge Insertion Criteria:** Two vertices are connected by an edge only when both conditions hold:
  * **Proximity:** Euclidean distance between the two vertices is **less than 15.0 units**.
  * **Directional Alignment:** Either the horizontal offset ($\Delta x$) is **less than 3.0** or the vertical offset ($\Delta y$) is **less than 5.0**, ensuring edges follow coherent tactile paths rather than noisy cross-connections.

---

## Tech Stack
* **Core Application & Graph Engine:** Java (Adjacency List Graph, HashMap indexing, Ray Casting)
* **Image Processing:** Grayscale, Gaussian Blur, Scharr Gradient Operator, Recursive Peak Finding
* **Data & Annotations:** Roboflow, COCO JSON, SQLite
