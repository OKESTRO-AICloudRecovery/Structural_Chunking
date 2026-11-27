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


### Usage

```bash
pip install langchain-core langchain-huggingface sentence-transformers numpy
pip install spacy        # sentence splitter backend (or: pip install kiwipiepy for --nlp kiwi)

# chunk + evaluate (existing chunks are reused if present)
python main.py --mode chunk --datasets nfcorpus scifact --method percentile structural recursive --nlp spacy

# evaluate only, using previously generated chunks
python main.py --mode eval --datasets nfcorpus scifact arguana scidocs fiqa
```

| Option | Values (default) |
|---|---|
| `--mode` | `chunk` (generate + evaluate) / `eval` (evaluate only), default `chunk` |
| `--datasets` | BEIR test splits, default `nfcorpus scifact arguana scidocs fiqa` |
| `--method` | `percentile`, `stddeviation`, `interquartile`, `gradient` (semantic breakpoints), `structural` (document structure), `recursive` (fixed-length baseline); default all |
| `--nlp` | sentence splitter backend `spacy` / `kiwi`, default `spacy` |

Embeddings: `all-MiniLM-L6-v2` (normalized). Ground truths are written to `config/beir_ground_truths.json`, logs to `logs/main.log`.

### Required modules and data

`main.py` imports the following project modules, which are **not included in this repository**:

- `chunking_methods` — `PercentileChunker`, `StdDeviationChunker`, `InterquartileChunker`, `GradientChunker`, `StructuralChunker`, `FixedLenChunker`
- `utils.helpers` (`save_results`), `utils.beir_loader` (`load_beir_datasets`, `BEIRDataLoader`)
- `evaluation.retrieval_quality_evaluator_clean`, `evaluation.chunk_size_evaluator`

BEIR datasets are expected under `./data/`, and the `logs/` directory must exist before running.

---

### Recommended Hardware

Component | Recommendation
--------- | --------------
GPU | A100 80GB / 80GB
Precision | bfloat16
Batch size | 2048–4096
Embedding Model | all-MiniLM-L6-v2

---


### Related Repositories

- [universal-rag](https://github.com/OKESTRO-AICloudRecovery/universal-rag) — Multimodal RAG for cloud-operations knowledge
- [Document_Parser](https://github.com/OKESTRO-AICloudRecovery/Document_Parser) — VLM document parser producing the documents to be chunked
- [incident-retriever](https://github.com/OKESTRO-AICloudRecovery/incident-retriever) — year 2: hybrid retrieval of similar incidents
- [Project overview and all repositories](https://github.com/OKESTRO-AICloudRecovery)

---

### Third-party Code

`model_utils.py` contains code adapted from the `SemanticChunker` text splitter of
[LangChain Experimental](https://github.com/langchain-ai/langchain-experimental) (MIT License)
and from the CPython `abc` module (Python Software Foundation License). Their copyright and license terms continue to apply to those parts.

---

### Acknowledgement

This work was supported by the Institute of Information & communications Technology Planning & Evaluation (IITP) grant funded by the Korea government (MSIT) (No. RS-2025-25441560, AI Assistant-based Automation Technologies for Operations and Management in Cloud Failure Recovery).

이 소프트웨어는 2025년도 정부(과학기술정보통신부)의 재원으로 정보통신기획평가원의 지원을 받아 수행된 연구의 결과물입니다.

---

### Who We Are
회사 홈페이지:
http://okestro.com/


---
### License
MIT License
