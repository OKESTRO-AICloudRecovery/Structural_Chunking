# Structural_Chunking


This repository provides an **end-to-end benchmarking framework for document chunking methods** on BEIR datasets.  
It supports multiple semantic, statistical, and structural chunking strategies and evaluates their impact on retrieval quality in a controlled and reproducible manner.

---

## Core Files

### main.py

Entry point of the benchmarking pipeline.

This script orchestrates the entire experimental workflow, including BEIR dataset loading, chunk generation, retrieval quality evaluation, and result visualization.

**Key responsibilities:**

- Load BEIR datasets and corresponding ground-truth relevance judgments  
- Initialize HuggingFace embedding models  
- Apply multiple chunking methods in a unified execution flow  
- Cache and reuse pre-generated chunks to ensure reproducibility  
- Evaluate retrieval performance using BEIR relevance data  
- Save and visualize experimental results  

Execution modes are controlled via CLI arguments (`chunk`, `eval`) to flexibly switch between chunk generation and evaluation-only runs.

---

### model_utils.py

Utility module for model and embedding configuration.

This file encapsulates reusable logic related to embedding initialization and model-level configuration to keep the main execution logic clean and modular.

**Key responsibilities:**

- Initialize HuggingFace-based embedding models with consistent parameters  
- Centralize embedding-related configuration (batch size, normalization, etc.)  
- Provide shared utilities used across chunking and evaluation modules  
- Reduce coupling between experimental logic and model implementation details  

---


### Who We Are
회사 홈페이지:
http://okestro.com/
