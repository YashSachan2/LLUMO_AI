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
