# 📊 RAG Pipeline Evaluation Framework

## 📘 Overview

This script computes **RAGAs metrics** — `faithfulness`, `answer_relevancy`, and `context_precision` — for each item in a structured LLM log JSON file. It evaluates how well the LLM’s output (expected response) aligns with the user query and the provided system context.

The script processes the input log and produces a list of JSON objects in the following format:

```json
[
  {
    "id": "example-item-id",
    "faithfulness": 0.67,
    "answer_relevancy": 0.58,
    "context_precision": 0.42
  }
]
```
## ⚙️ Approach

Each log entry contains:

- **System context** → `input[0]["context"]`  
- **User query** → `input[1]["context"]`  
- **Expected output** → `expectedOutput[].content`

These are used to compute the three metrics as follows:

| Metric              | Computation Logic                                                                  |
|---------------------|--------------------------------------------------------------------------------------|
| `faithfulness`       | Measures token overlap between the system context and the model’s response         |
| `answer_relevancy`   | Cosine similarity between the embeddings of the query and the response             |
| `context_precision`  | Measures how many response sentences are substrings of the context                 |
## 📦 Libraries Used

- `json`: Standard library for JSON parsing  
- `pathlib`: For cross-platform file path handling  
- `sentence-transformers`: To compute semantic sentence embeddings  

**Model used:**  
- `all-mpnet-base-v2`

---

## 🚀 Features

* **Automated Metric Calculation**: Computes three critical RAG metrics:
    * **Faithfulness**: Measures how well the response is grounded in the retrieved context.
    * **Answer Relevancy**: meaningfulness of the response to the user's query.
    * **Context Precision**: Evaluates the signal-to-noise ratio in the retrieved context.
* **Dual Evaluation Modes**:
    * **Basic Mode**: Fast computation using token overlap and cosine similarity.
    * **Advanced Mode**: High-precision evaluation using NLI (Natural Language Inference) models and Cross-Encoders.
* **Data Visualization**: Generates histograms and distribution plots for all metrics using `Seaborn` and `Matplotlib`.
* **JSON Reporting**: Exports detailed per-interaction scores to `ragas_scores.json`.

## 📊 Metrics Explained

### 1. Faithfulness
Determines if the answer is factually consistent with the provided context.
* *Basic*: Token overlap ratio between context and response.
* *Advanced*: Uses an NLI model (`roberta-large-mnli`) to check textual entailment.

### 2. Answer Relevancy
Assesses how pertinent the response is to the given prompt.
* *Basic*: Cosine similarity between the query embedding and response embedding.
* *Advanced*: Uses a Cross-Encoder (`stsb-roberta-large`) to predict a semantic similarity score.

### 3. Context Precision
Evaluates whether the retrieved context contains the necessary information to answer the prompt.
* *Basic*: Sentence-level overlap analysis.
* *Advanced*: Semantic similarity check between context sentences and response clauses.



## 📥 Installation

Install the required Python package:

```bash
pip install sentence-transformers
```

## 🎯 Assumptions & Simplifications

- The full system prompt is treated as **context**.
- The assistant response is **flattened into one string** before scoring.
- `faithfulness` and `context_precision` rely on **exact token/sentence overlap** (no deep semantic matching).
- `answer_relevancy` is calculated using **cosine similarity** between sentence embeddings.
- There is **no use of golden reference answers or Cypher templates** — evaluation is done **only relative to context and query**.

