# Brief: RAG Pipeline

## Objective

Implement the first-release RAG example using the complete local corpus and stage definitions in this repo.

## Required Inputs

- `fixtures/knowledge/sources/`
- `fixtures/knowledge/registrations/source_registry.json`
- `fixtures/knowledge/retrieval_cases/*.json`
- `fixtures/evals/rag_pipeline_eval_cases.json`
- `contracts/retrieval-contracts.schema.json`

## Required Stages

1. source registration
2. document parsing
3. normalization
4. structured enrichment
5. chunking
6. index preparation
7. retrieval with persona and policy filters
8. selection or reranking
9. grounded response packaging
10. evaluation hooks

## Minimum Done Definition

- pipeline can ingest every source in the local corpus
- retrieval cases return the expected document families
- internal-only content is excluded for non-colleague personas
- outputs conform to the shared retrieval contract
- the main colleague RAG-policy flow can consume the produced evidence package
