# Contributing

This repo is a shared use-case pack for multiple asset teams. The goal is to keep one coherent scenario and one consistent set of contracts, fixtures, and eval cases that separate libraries can implement against.

## Contribution Principles

- Prefer extending shared fixtures over inventing asset-local variants.
- Keep the repo implementation-agnostic.
- When adding new scenarios, make sure they map back to the centralized pharma contact-center world.
- Treat `contracts/` as the interoperability boundary. If a contract changes, update every dependent fixture and document in the same change.
- Preserve local-only assumptions. Do not add real service dependencies, credentials, or live endpoints.

## Directory Responsibilities

- `docs/`: human-readable source of truth for scenario and architecture.
- `briefs/`: team-facing instructions for what to build from this repo.
- `contracts/`: neutral schemas and shared data shapes.
- `fixtures/contacts/`: persona and identity fixtures.
- `fixtures/tools/`: deterministic mock backing stores and tool definitions.
- `fixtures/knowledge/sources/`: raw corpus inputs for RAG implementations.
- `fixtures/knowledge/parsed/`: golden parser outputs.
- `fixtures/knowledge/normalized/`: golden normalization outputs.
- `fixtures/knowledge/chunks/`: golden chunking outputs.
- `fixtures/knowledge/retrieval_cases/`: retrieval prompts, constraints, and expected outputs.
- `fixtures/transcripts/`: scenario-aligned conversation seeds.
- `fixtures/transcripts/*.golden_response.json`: example assistant wording aligned to the transcript scenarios.
- `fixtures/evals/`: reusable eval datasets and acceptance cases.

## When Adding A New Fixture

1. Use a stable `scenario_id`, `doc_id`, `chunk_id`, or `contact_id`.
2. Keep names descriptive and domain-consistent.
3. Update the relevant docs if the fixture introduces a new behavior or scenario branch.
4. If the fixture is meant for evaluation, add or update an eval row under `fixtures/evals/`.
5. If the fixture depends on a shared shape, validate it against the corresponding schema in `contracts/`.
6. If the fixture introduces an important conversation path, consider adding a matching `*.golden_response.json` example under `fixtures/transcripts/`.

## When Changing Contracts

1. Update the schema file in `contracts/`.
2. Update all affected fixtures to match the new shape.
3. Update any docs or briefs that mention the changed fields.
4. If the change affects the RAG pipeline, ensure the stage descriptions in `docs/rag-pipeline-spec.md` still match the fixtures.
5. Include at least one example fixture showing the new field or behavior.

## RAG Fixture Expectations

RAG is the first-release priority, so contributions here should be especially disciplined.

When adding a new corpus document:

- add the raw source file under `fixtures/knowledge/sources/`
- add a registry row in `fixtures/knowledge/registrations/source_registry.json`
- add or update a parsed golden artifact if the file introduces a new source pattern
- add or update a normalized artifact if normalization behavior is important
- add or update chunk outputs when chunking expectations change
- add or update retrieval cases and evals if the document should be discoverable

When adding a new retrieval case:

- specify persona and allowed retrieval scope
- list expected relevant docs
- list forbidden docs when scope boundaries matter
- describe expected response traits and citation behavior
- add a golden output if the case is important enough to anchor regressions

## Review Checklist

Before submitting changes, confirm:

- JSON fixtures parse cleanly
- docs and fixtures agree on scenario names and IDs
- no internal-only documents are accidentally expected in patient-facing cases
- no examples require external services
- new scenarios have at least one transcript or eval row
- golden response fixtures still match the intended transcript behavior when present

## Recommended Workflow

- start from the relevant brief in `briefs/`
- make the smallest coherent change that updates contracts, fixtures, docs, and evals together
- validate JSON locally
- keep commit messages descriptive, such as `Add golden chunk outputs for RAG regression cases`
