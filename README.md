# LlamaIndex Query Engine — SubQuestionQueryEngine

A Jupyter Notebook implementing an advanced question-answering agent using **LlamaIndex's SubQuestionQueryEngine**, which decomposes complex, multi-part queries into targeted sub-questions, retrieves relevant information for each, and synthesises a unified grounded answer.

---

## Table of Contents

- [Overview](#overview)
- [Background](#background)
- [SubQuestionQueryEngine Architecture](#subquestionqueryengine-architecture)
- [Notebook Contents](#notebook-contents)
- [Technologies Used](#technologies-used)
- [Setup and Installation](#setup-and-installation)
- [Usage](#usage)
- [References](#references)
- [Contact](#contact)

---

## Overview

Standard RAG pipelines retrieve a single set of documents and generate one response to a query. This works well for simple, focused questions but struggles with complex queries that span multiple topics or require information from multiple sources. The `SubQuestionQueryEngine` addresses this by decomposing a complex question into a set of targeted sub-questions, routing each to the most appropriate index or document source, and then synthesising the individual answers into a coherent final response.

---

## Background

### Standard RAG vs. SubQuestionQueryEngine

| Approach | How it handles complex queries |
|---|---|
| **Standard RAG** | Embeds the full query, retrieves top-K chunks, generates one response |
| **SubQuestionQueryEngine** | Decomposes the query into N sub-questions, retrieves targeted context per sub-question, synthesises all answers |

This distinction matters most when a question requires integrating information across multiple documents, time periods, or topic areas — for example: *"Compare the revenue growth and customer retention strategies of Company A and Company B over the last two years."* A standard RAG pipeline would retrieve mixed chunks; the SubQuestionQueryEngine generates separate sub-questions for each company and each metric, answers them independently, and then combines the results.

---

## SubQuestionQueryEngine Architecture

```
Complex User Query
        │
        ▼
┌────────────────────────────────────┐
│  Question Decomposer (LLM)         │
│  Breaks query into N sub-questions │
│  Routes each to appropriate index  │
└──────────┬─────────────────────────┘
           │
    ┌──────┴──────┐
    │             │
    ▼             ▼
Sub-Q 1       Sub-Q 2  ...  Sub-Q N
    │             │
    ▼             ▼
Index 1       Index 2  ...  Index N
(retrieve)    (retrieve)
    │             │
    ▼             ▼
Answer 1      Answer 2  ...  Answer N
    │             │
    └──────┬──────┘
           ▼
┌────────────────────────────────────┐
│  Response Synthesiser (LLM)        │
│  Combines all sub-answers into     │
│  a single coherent final answer    │
└────────────────────────────────────┘
           │
           ▼
    Final Grounded Answer
```

---

## Notebook Contents

| Section | Description |
|---|---|
| Setup & Index Construction | Loading documents, building vector indices with LlamaIndex |
| SubQuestionQueryEngine Setup | Configuring the engine with multiple index sources and an LLM synthesiser |
| Query Decomposition | Demonstrating how a complex query is automatically broken into sub-questions |
| Multi-Source Retrieval | Each sub-question routed to its corresponding document index |
| Answer Synthesis | Final response assembled from all sub-question answers |
| Examples | Sample complex queries and their decomposed, synthesised answers |

---

## Technologies Used

| Library | Purpose |
|---|---|
| `llama-index` | Query engine, index construction, SubQuestionQueryEngine |
| `openai` | LLM backbone for question decomposition and answer synthesis |
| `numpy` / `pandas` | Data handling |
| `python 3.10+` | Runtime environment |

---

## Setup and Installation

```bash
git clone https://github.com/chetnapriyadarshini/Llama_Index_Query_Engine.git
cd Llama_Index_Query_Engine
pip install llama-index openai pandas numpy
```

Set your OpenAI API key:

```bash
export OPENAI_API_KEY="your-api-key-here"
```

Launch the notebook:

```bash
jupyter notebook llama_QA_agent.ipynb
```

---

## References

- LlamaIndex Documentation — SubQuestionQueryEngine: https://docs.llamaindex.ai/en/stable/examples/query_engine/sub_question_query_engine/
- Liu, J. (2022). *LlamaIndex*. https://github.com/jerryjliu/llama_index

---

## Contact

Created by [@chetnapriyadarshini](https://github.com/chetnapriyadarshini) — feel free to reach out with questions or suggestions.
