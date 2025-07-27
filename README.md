# 📊 RAGAs Metrics Integration (Basic)

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

## 📥 Installation

Install the required Python package:

```bash
pip install sentence-transformers


## 🎯 Assumptions & Simplifications

- The full system prompt is treated as **context**.
- The assistant response is **flattened into one string** before scoring.
- `faithfulness` and `context_precision` rely on **exact token/sentence overlap** (no deep semantic matching).
- `answer_relevancy` is calculated using **cosine similarity** between sentence embeddings.
- There is **no use of golden reference answers or Cypher templates** — evaluation is done **only relative to context and query**.

