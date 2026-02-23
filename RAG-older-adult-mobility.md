# Retrieval-Augmented Generation (RAG) for Older Adult Mobility and Health Information

**Author:** Jia Yang  
**Supervisor:** Dr. Rong Zheng  
**Institution:** McMaster University  

---

## Table of Contents

1. [Project Description and Approach](#1-project-description-and-approach)
   - [Motivation](#motivation)
   - [System Overview](#system-overview)
   - [Key Design Features](#key-design-features)
   - [Architecture Components](#architecture-components)
2. [Results](#2-results)
   - [Retrieval Performance (BEIR Benchmark)](#retrieval-performance-beir-benchmark)
   - [Local vs Web Retrieval](#local-vs-web-retrieval)
   - [Qualitative Evaluation](#qualitative-evaluation)
3. [Source Code](#3-source-code)
4. [References](#4-references)

---

## 1. Project Description and Approach

### Motivation

Older adults increasingly rely on online resources for health information related to mobility, fall prevention, balance training, and rehabilitation. However:

- Search engines often return fragmented or unreliable results.
- Large Language Models (LLMs) may generate fluent but unverified responses.
- Health misinformation can negatively impact safety and decision-making.

This project develops a reliable, citation-grounded **Retrieval-Augmented Generation (RAG)** system designed specifically for older-adult mobility and health information.

---

### System Overview

The system integrates:

- **BM25** lexical retrieval  
- **Cross-encoder** semantic reranking  
- **Relevance-gating mechanism** (CE threshold τ = 0.6)  
- **Google Search API** web fallback  
- **LLM-based** answer generation with explicit citations  

#### Pipeline Summary

1. User submits a natural-language query.  
2. BM25 retrieves top-k passages from a curated MongoDB corpus.  
3. A cross-encoder reranks candidates.  
4. A relevance gate determines:
   - High CE score → use local database evidence  
   - Low CE score → trigger web fallback  
5. The LLM generates a plain-language answer with citations.

---

### Key Design Features

- Curated corpus of trusted health documents and passages  
- Explicit source citations in every answer  
- Plain-language output tailored for older adults  
- Safety-aware prompts with medical disclaimers  
- Modular Python implementation (BM25, Sentence-Transformers, Flask, Streamlit)

---

### Architecture Components

- **Crawler Layer** (PDF + Web)  
- **Retrieval Layer** (BM25)  
- **Reranking Layer** (Cross-encoder)  
- **Decision Layer** (Relevance Gate)  
- **Generation Layer** (LLM synthesis)  
- **User Interface Layer** (CLI, Flask API, Streamlit UI)

---

## 2. Results

### Retrieval Performance (BEIR Benchmark)

Semantic retrieval outperformed BM25:

| Dataset | Method   | nDCG@10 | MAP@10 | Recall@100 |
|----------|----------|----------|----------|-------------|
| SciFact  | BM25     | 0.5178   | 0.4760   | 0.7896      |
| SciFact  | Semantic | 0.6451   | 0.5959   | 0.9250      |
| NFCorpus | BM25     | 0.3804   | 0.3415   | 0.6827      |
| NFCorpus | Semantic | 0.4686   | 0.4317   | 0.8541      |

Hybrid BM25 + cross-encoder balances efficiency and semantic precision.

---

### Local vs Web Retrieval

| Scope        | Local CE     | Web CE   |
|--------------|-------------|----------|
| In-scope     | ≈ 0.997     | ≈ 0.892 |
| Out-of-scope | ≈ 0.000018  | ≈ 0.958 |

This demonstrates strong domain sensitivity, effective relevance gating, and reliable fallback for out-of-scope queries.

---

### Qualitative Evaluation

Real-world older-adult queries demonstrate:

- Evidence-grounded responses  
- High in-domain relevance  
- Appropriate fallback behavior  
- Clear citation display  
- Safety disclaimers  

---

## 3. Source Code

The full implementation, retrieval pipeline, and deployment instructions are maintained in a private GitHub repository:

**Github Repository:**  
[View GitHub Repository](https://github.com/Gavinyanggit/RAG-older-adult-mobility)


---

## 4. References

- Lewis et al., 2020 – Retrieval-Augmented Generation  
- Izacard & Grave, 2021 – Fusion-in-Decoder (FiD)  
- Thakur et al., 2021 – BEIR Benchmark  
- WHO Healthy Aging Initiative  
- National Institute on Aging  
