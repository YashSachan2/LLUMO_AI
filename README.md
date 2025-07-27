RAGAs Metrics Integration

1. Overview
This script computes RAGAs metrics — faithfulness, answer_relevancy, and context_precision — for each item in a structured LLM log JSON file. It analyzes how well the LLM’s output (expected response) aligns with the user query and the provided system context.

The results are saved as a list of JSON objects with the following structure:

[
  {
    "id": "example-item-id",
    "faithfulness": 0.67,
    "answer_relevancy": 0.58,
    "context_precision": 0.42
  },
  ...
]

2. Approach
Each log entry contains:

System context (input[0]['context']) → used as background context

User query (input[1]['context']) → used as the primary query

Expected output (expectedOutput[].content) → used as the LLM’s answer

The script uses the following heuristics:

Metric	Computation Logic
faithfulness	Measures overlap of response tokens with context tokens
answer_relevancy	Cosine similarity between sentence embeddings of query and response
context_precision	Fraction of response sentences that appear (as substrings) in the system context

Libraries Used
json: standard library for JSON handling

pathlib: clean path handling

sentence-transformers: to compute semantic embeddings

Model used: all-mpnet-base-v2

Install dependencies with:

bash
Copy
Edit
pip install sentence-transformers


🎯 Assumptions & Simplifications
The entire system message is treated as the “context” for faithfulness and precision scoring.

The entire assistant output is concatenated into a single string before scoring.

Faithfulness and context precision use exact token/sentence overlap; no deep semantic checks.

Relevancy is computed using pretrained sentence embeddings and cosine similarity.

No ground-truth Cypher templates or golden answers are used — evaluation is relative to context/query only.




