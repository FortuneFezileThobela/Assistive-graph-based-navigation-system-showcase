# Assistive Graph-Based Navigation System for Blind Pedestrians Using Tactile Paving Recognition

An assistive pedestrian navigation system that combines machine learning **tactile paving recognition**, computer vision **feature/vertex extraction**, **graph-based spatial modeling**, and an accessible **user interface** to help blind and visually impaired pedestrians navigate walkways safely and independently.

> **Repository Notice:** This repository serves as a public overview, architecture showcase, and demo hub. The full source code is kept private to respect group project ownership—please reach out via [your email / LinkedIn] for a technical walkthrough or inquiries.

---

## Project Team
* **Fortune Fezile Thobel *(Me)*:** Data Ingestion (COCO JSON → SQLite), Ray-Casting Preprocessing, Multi-Stage CV Filtering, Vertex Extraction & Adjacency List Graph Construction *(Java)*
* **Morris Manganhe:** Machine Learning Tactile Paving Detection Algorithm
* **Sibusisiwe Mbatha:** User Interface (UI)

---

## System Demo

[![Watch the Assistive Navigation System Demo](https://img.youtube.com/vi/dMtqbKz7Xk8/hqdefault.jpg)](https://www.youtube.com/watch?v=dMtqbKz7Xk8)

*Click the preview image above to watch the demonstration on YouTube.*

---

## System Architecture & Pipeline

### 1. Pre-Extraction & Data Ingestion *(Fortune Fezile Thobel)*
* **Dataset & Annotation Storage:** Uses a Roboflow dataset annotated in **COCO JSON** format (bounding boxes, category IDs, and segmentation masks).
* **SQLite Indexing:** Extracts bounding box annotations, category IDs, and segmentation data from COCO JSON into a **SQLite** database for efficient querying inside the Java application.
* **In-Memory Lookup:** Fetches the SQLite database at startup and caches records in a hash dictionary keyed by `image_id` for rapid retrieval during image processing.

### 2. Machine Learning Detection *(Morris Manganhe)*
* *(Placeholder — add Morris's ML detection algorithm details here once received.)*

### 3. Preprocessing, Filtering & Vertex Extraction *(Fortune Fezile Thobel)*
Each annotated region is partitioned using its bounding box and processed independently through three stages:
1. **Preprocessing & Ray-Casting Mask:** Applies **ray casting** to construct a 2D virtual polygonal mask inside each bounding box, isolating the tactile surface and eliminating surrounding background noise.
2. **Stacked Noise & Edge Filtering:**
   * **Grayscale Filter:** Reduces RGB channels into a single intensity channel.
   * **Gaussian Blur Filter:** Suppresses high-frequency noise to prevent false-positive detections.
   * **Scharr Edge Detection:** Computes image gradients and applies binary thresholding (`gradient > 0.3` marked White, otherwise Black).
3. **Recursive Peak Finding & Uniqueness Validation:**
   * Scans bright pixels and applies **8-neighbor recursion**, climbing toward local maximum brightness so each coordinate centers accurately on the tactile feature.
   * Enforces a minimum separation of **5 units** between detected points to prevent duplicate vertices on the same feature.

### 4. Adjacency List Graph Construction *(Fortune Fezile Thobel)*
Validated coordinates are inserted into an **Adjacency List** graph representing walkable tactile pathways:
* **Vertex Insertion:** Every unique coordinate is inserted as a vertex in the graph.
* **Edge Insertion Criteria:** An edge is added between two vertices only when both conditions are met:
  * **Euclidean Distance:** The distance between the two vertices is **less than 15.0**.
  * **Directional Alignment:** Either the horizontal change ($\Delta x$) is **less than 3.0** or the vertical change ($\Delta y$) is **less than 5.0**, ensuring edges follow coherent tactile paths.

### 5. User Interface *(Sibusisiwe Mbatha)*
* *(Optional — add a brief description of Sibusisiwe's UI features and interaction flow if desired.)*

---

## Tech Stack
* **Core Processing & Graph Engine:** Java (Adjacency List Graph, HashMap Indexing, Ray Casting)
* **Computer Vision & Image Filtering:** Grayscale, Gaussian Blur, Scharr Gradient Operator, Recursive Peak Finding
* **Data & Annotations:** Roboflow, COCO JSON, SQLite
* **Machine Learning & UI Modules:** *(To be updated with Morris's ML framework and Sibusisiwe's UI stack)*
