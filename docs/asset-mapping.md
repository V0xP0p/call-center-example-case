# Asset Mapping

This document maps the centralized contact-center scenario to the reusable assets under development. Each section defines the scope of responsibility, the shared artifacts to consume, and the acceptance definition.

## Project Scaffold

### Goal

Generate a starter project that mounts the shared fixtures and demonstrates the core orchestration path without requiring live services.

### Must Consume

- `contracts/*.schema.json`
- `fixtures/contacts/contact_profiles.json`
- `fixtures/tools/*.json`
- `fixtures/transcripts/`
- `fixtures/evals/e2e_acceptance_cases.json`

### Showcase Responsibilities

- wire config loading for a local-only environment
- register mock tools
- expose example entrypoints for each sub-scenario
- demonstrate how traces and evals attach to a run

### Acceptance

An adopter can generate a starter and run one scenario against local data with no external dependencies.

## Agent Cookie-Cutters

### Goal

Provide reusable templates for common roles in the scenario.

### Required Templates

- `triage_agent`
- `support_agent`
- `safety_intake_agent`
- `escalation_agent`

### Shared Inputs

- route decisions from `contracts/route-decision.schema.json`
- tool contracts from `contracts/tool-contracts.schema.json`
- retrieval packages from `contracts/retrieval-contracts.schema.json`
- transcripts from `fixtures/transcripts/`

### Acceptance

A team can instantiate one template and match the expected behavior in a shared transcript or eval row.

## RAG Pipeline

### Goal

Ship the most complete first-release example, covering every stage from source registration through grounded response packaging.

### Must Consume

- `fixtures/knowledge/sources/`
- `fixtures/knowledge/registrations/source_registry.json`
- `fixtures/knowledge/retrieval_cases/`
- `fixtures/evals/rag_pipeline_eval_cases.json`
- `contracts/retrieval-contracts.schema.json`

### Acceptance

A team can implement ingestion, parsing, normalization, chunking, indexing, retrieval, selection, and grounded packaging from the provided corpus and meet the expected retrieval cases.

The main RAG showcase should be the colleague question about internal RAG policy using the internal PDF policy corpus.

## Guardrails

### Goal

Demonstrate policy-aware response control in a medically sensitive setting.

### Must Detect

- unsafe medical advice
- unsupported or uncited claims
- internal-content leakage
- missing safety intake information
- PII leakage
- wrong persona access

### Acceptance

Negative examples in the shared evals produce a deterministic block, revise, or escalate outcome.

## Evals

### Goal

Provide layer-specific and end-to-end scoring inputs that all assets can reuse.

### Shared Scoring Dimensions

- routing accuracy
- persona handling
- retrieval precision and relevance
- citation and grounding quality
- correct tool selection
- safety workflow completeness
- guardrail adherence
- end-to-end task success

### Acceptance

Each asset team can run the same eval rows at their level and compare results consistently.
