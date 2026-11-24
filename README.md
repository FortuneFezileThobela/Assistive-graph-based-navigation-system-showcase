# Assistive Graph-Based Navigation System for Blind Pedestrians Using Tactile Paving Recognition

A desktop-based assistive pedestrian navigation prototype developed as a group project to help blind and visually impaired pedestrians navigate public walkways more safely and independently. Rather than relying solely on ground-level tactile tiles that may be obstructed or discontinuous, the system analyzes ground imagery using **computer vision**, **graph modeling**, **graph neural networks (GCN)**, and **structural similarity matching** to classify tactile patterns and assess path safety.

> **Repository Notice:** This repository serves as a public overview, system architecture guide, and demonstration hub. The full source code is kept private to respect group project ownership—please reach out via [your email / LinkedIn] for a walkthrough or inquiries.

---

## Project Team
* **Fortune Fezile Thobel *(Me)*:** Data Extraction & Database Ingestion, Ray-Casting Preprocessing, Multi-Stage Image Filtering, Vertex Extraction & Adjacency List Graph Construction
* **Morris Manganhe:** Graph Convolutional Neural Network (GCN) Classification & k-NN Structural Similarity Module
* **Sibusisiwe Mbatha:** User Interface (UI) & System Integration / Demo Visualization

---

## System Demo

[![Watch the Assistive Navigation System Demo](https://img.youtube.com/vi/dMtqbKz7Xk8/hqdefault.jpg)](https://www.youtube.com/watch?v=dMtqbKz7Xk8)

*Click the preview image above to watch the full demonstration on YouTube.*

---

## End-to-End System Architecture

### 1. Data Ingestion, Preprocessing & Graph Construction *(Fortune Fezile Thobela
)*
* **Dataset & Database Setup:** Parses COCO JSON labels (annotations, categories, and segmentation data) into a relational database and loads image metadata into an in-memory lookup dictionary for fast access.
* **Bounding-Box Partitioning & Ray-Casting Mask:** Splits images by bounding box regions and uses **ray casting** to build a virtual polygon mask that isolates tactile areas and reduces background noise.
* **Multi-Stage Filtering & Vertex Extraction:** Applies stacked image filters (intensity conversion, smoothing, and gradient/edge detection) followed by **8-neighbor recursive peak finding** with minimum spacing checks to place unique vertices without duplicate nodes on the same feature.
* **Adjacency List Graph:** Connects nearby vertices into a graph when distance and directional axis constraints are satisfied, forming the structural representation of the detected tactile layout.

### 2. Tactile Pattern Classification & Structural Similarity *(Morris Manganhe)*
* **GCN Classification:** Uses a lightweight **Graph Convolutional Neural Network** (with four input features/channels) to aggregate neighborhood graph structure, pool node embeddings into a graph-level representation, and classify tactile patterns such as directional/straight paths and warning indicators.
* **k-NN Structural Similarity:** Compares extracted graph topology metrics (such as node scale, edge density, node degree, center distance, and horizontal edge ratio) using distance-based similarity against known reference patterns to rank how closely a path matches a safe standard.

### 3. User Interface & Integration *(Sibusisiwe Mbatha)*
* **Interactive Demo & Visualization:** Provides an interface with a sidebar and image analysis view to select images, inspect highlighted tactile regions and category color legends, run classification and similarity checks, and view overlaid graph structures and match rankings.
* **Cross-Module Integration:** Connects the UI, database/graph logic, and ML inference pipeline so detected tactile paving patterns and similarity results can be translated into clear navigation cues (such as directional guidance or caution alerts).

---

## Tech Stack
* **Graph, CV & Data Pipeline:** COCO JSON, SQLite, Ray-Casting Masking, Multi-Stage Image Filtering, Recursive Peak Finding, Adjacency List Graphs
* **Machine Learning & Similarity:** Graph Convolutional Neural Networks (GCN), k-Nearest Neighbors (k-NN) Structural Similarity
* **Interface & Integration:** Interactive Desktop UI & Cross-Language Module Integration *(designed for future adaptation to wearable/smart-glasses camera setups)*
