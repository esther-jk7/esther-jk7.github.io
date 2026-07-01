---
layout: page
title: Healthcare Agentic RAG
description: Production multi-agent clinical Q&A system — deployed on HuggingFace Spaces
img: assets/img/prof_pic.jpg
importance: 1
category: work
---

A production-grade multi-agent clinical question-answering system that retrieves from PubMed literature, validates source quality, and generates cited answers. Built with LangGraph, ChromaDB, FastAPI, and Docker. Live at [estherrjk-healthcare-agentic-rag.hf.space](https://estherrjk-healthcare-agentic-rag.hf.space/docs).

## Architecture

The system uses a three-agent LangGraph pipeline:

- **Retriever** — Semantic search over 2,918 PubMed chunks using `all-MiniLM-L6-v2` embeddings and ChromaDB
- **Validator** — Llama 3.3 70B checks if retrieved chunks contain sufficient evidence to answer the question
- **Synthesizer** — Generates a cited answer with inline PubMed references; flags `low_confidence=true` when evidence is insufficient

The conditional edge between Validator and Retriever enables retry logic (max 2 retries) before falling back to synthesis with a low confidence flag.

## Tech Stack

- **Orchestration:** LangGraph (state machine with typed AgentState)
- **Vector Store:** ChromaDB with sentence-transformers embeddings
- **LLM:** Llama 3.3 70B via Groq
- **API:** FastAPI with async handlers, Pydantic validation, structured logging
- **Observability:** LangSmith — full trace visibility per query
- **Deployment:** Docker (multi-stage build) → HuggingFace Spaces
- **CI/CD:** GitHub Actions — ruff lint + pytest on every push

## Key Results

- **Recall@5 = 1.00** on 10 PubMedQA evaluation samples
- **Low confidence detection** correctly flags out-of-distribution queries (pizza question returns `low_confidence: true` with no hallucination)
- **Latency:** 1.2–2.5s per query end-to-end
- **CI:** green on every push to main

## Links

- **Live API:** [estherrjk-healthcare-agentic-rag.hf.space/docs](https://estherrjk-healthcare-agentic-rag.hf.space/docs)
- **GitHub:** [github.com/esther-jk7/healthcare-agentic-rag](https://github.com/esther-jk7/healthcare-agentic-rag)
- **Architecture rationale:** [ARCHITECTURE.md](https://github.com/esther-jk7/healthcare-agentic-rag/blob/main/ARCHITECTURE.md)
- **Engineering decisions:** [DECISIONS.md](https://github.com/esther-jk7/healthcare-agentic-rag/blob/main/DECISIONS.md)