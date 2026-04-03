# Release Notes: v0.1.0

## Summary

Initial publication of the centralized pharma contact-center use-case pack for SDK example builders.

This release establishes a shared scenario, reusable local fixtures, neutral contracts, and first-release RAG artifacts that multiple asset teams can build against independently.

## Included

- Canonical scenario definition covering patient, doctor, and colleague personas
- Shared flow diagrams and asset mapping for scaffold, agents, RAG, guardrails, and evals
- Local mock fixtures for contacts, tool stores, transcripts, and acceptance cases
- Complete RAG-first corpus package with source registration, retrieval cases, and evaluation rows
- Golden intermediate RAG artifacts for parsing, normalization, chunking, and grounded retrieval packaging
- Contributor guidance for extending scenarios, fixtures, and contracts consistently

## Highlights

- Flagship end-to-end scenario: doctor sample-order request with verification, sample lookup, retrieval, mock order creation, guardrails, and evaluation
- Main RAG scenario: colleague question about internal RAG policy using internal PDF policy documents
- Retrieval cases covering doctor, patient, colleague, and out-of-corpus behavior
- JSON-schema contracts that different stacks can reuse without coupling to one implementation

## Intended Use

- Asset teams can use this repo as the canonical truth source for examples and test cases
- RAG teams can implement the full pipeline from raw documents through grounded evidence packaging
- Guardrails and eval teams can reuse the same transcripts, retrieval cases, and expected outcomes
