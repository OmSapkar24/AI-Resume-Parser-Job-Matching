# AI-powered Resume Parser & Job Matching

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![spaCy](https://img.shields.io/badge/spaCy-3.x-green)](https://spacy.io/)
[![FastAPI](https://img.shields.io/badge/FastAPI-API-green)](https://fastapi.tiangolo.com/)
[![Docker](https://img.shields.io/badge/Docker-Ready-informational)](https://www.docker.com/)
[![CI](https://img.shields.io/badge/CI-GitHub%20Actions-blue)](./actions)

An end-to-end system that parses resumes (PDF/DOCX), extracts structured candidate profiles, and matches them to job descriptions using hybrid NLP + graph-based recommendations.

## Overview
- Document ingestion: PDF/DOCX -> text with OCR fallback
- Information extraction: NER for skills, entities; regex for contacts; embeddings for experience
- Matching: bi-encoder (SBERT) + cross-encoder re-ranker; graph features from skill-job ontology
- Serving: FastAPI microservice with queue-based async parsing; React demo client

## Business Context
- Reduce recruiter screening time by 70%
- Increase candidate-job fit and diversity with bias checks
- Multi-tenant support for agencies

## Tech Stack
- NLP: spaCy, Transformers, PyTorch, sentence-transformers
- Parsing: pdfminer, pymupdf, docx2python, Tesseract OCR
- Recsys: LightFM, Faiss ANN index, NetworkX for skill graph
- Ops: FastAPI, Celery/RQ, Redis, Docker, MLflow

## Repository Structure
```
resume-matching/
  data/
  notebooks/
  src/
    parsing/
      extract_text.py
      parse_docx.py
    nlp/
      ner.py
      embed.py
      re_rank.py
    recsys/
      graph_features.py
      faiss_index.py
    api/
      main.py
    ui/
      demo_app/
  configs/
  tests/
  docker/
```

## Sample Results (synthetic)
- Top-1 match accuracy vs. human labels: 84%
- Top-3 recall: 96%
- Average parsing time/document: 1.8s

## Installation
```
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python -m spacy download en_core_web_lg
```

## Usage
Build FAISS index for jobs:
```
python -m src.recsys.faiss_index --jobs data/jobs.jsonl
```
Parse a resume and get matches:
```
uvicorn src.api.main:app --reload
# POST /match with resume file and optional job ids
```

## Roadmap
- Bias and fairness audit with metrics (SPD, EOD)
- Fine-tuned cross-encoder for domain-specific roles
- Multi-language support
