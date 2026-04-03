# Centralized Pharma Contact Center Use-Case Pack

This repository is a canonical source of truth for example builders across a wider SDK. It is intentionally implementation-agnostic and organized around reusable specifications, fixtures, contracts, and acceptance cases rather than a single runnable app.

GitHub repository description:
`Repo-ready use-case pack for a centralized pharma contact center, with shared fixtures, contracts, RAG pipeline artifacts, guardrails, and eval cases.`

## What This Repo Contains

- A single end-to-end contact-center scenario spanning patient, doctor, and colleague personas.
- Modular sub-scenarios that map cleanly onto scaffold, agent templates, RAG, guardrails, and eval assets.
- Shared local fixtures for contacts, mock tools, knowledge sources, transcripts, and expected outcomes.
- RAG-first artifacts covering the full pipeline from document registration and parsing through retrieval and grounded response packaging.
- JSON schemas and neutral contracts that teams in different stacks can implement consistently.

## Repo Layout

- `docs/`
  - Canonical scenario definitions, architecture notes, and flow diagrams.
- `briefs/`
  - Short implementation briefs for each asset team.
- `contracts/`
  - JSON schemas and interface definitions shared across all examples.
- `fixtures/`
  - Local contacts, mock tool stores, knowledge sources, transcripts, and eval cases.

## Core Scenario

An inbound text user contacts a centralized pharma support center. The system identifies the persona (`patient`, `doctor`, or `colleague`), classifies intent, routes to the appropriate agentic flow, retrieves approved knowledge when needed, uses local mock tools for deterministic actions, applies safety and compliance guardrails, and either responds or escalates.

The flagship flow is a doctor sample-order request that requires:

1. Persona verification
2. Lookup of doctor-specific sample availability
3. Retrieval of approved sample-order guidance
4. Sample selection and quantity capture
5. Mock order-form creation
6. Guardrailed, grounded response generation
7. Shared evaluation of the full trace

## Five Canonical Flows

- [Doctor sample order request](docs/use-case-definition.md#1-doctor-sample-order-request)
- [Patient adverse event intake](docs/use-case-definition.md#2-patient-adverse-event-intake)
- [Doctor medical information request](docs/use-case-definition.md#3-doctor-medical-information-request)
- [Patient clinical-trial eligibility check](docs/use-case-definition.md#4-patient-clinical-trial-eligibility-check)
- [Internal colleague personal-data and RAG-policy requests](docs/use-case-definition.md#5-internal-colleague-personal-data-and-rag-policy-requests)

## How Teams Should Use This Repo

- Treat `fixtures/` and `contracts/` as the shared integration boundary.
- Use `docs/asset-mapping.md` to identify which subset belongs to your asset.
- Use `fixtures/transcripts/` and `fixtures/evals/` as the starting point for examples and regression tests.
- Use the golden response examples in `fixtures/transcripts/*.golden_response.json` when you want concrete assistant wording in addition to trait-based expectations.
- Use `fixtures/knowledge/` as the first-release RAG corpus and pipeline contract.

## Featured Golden Responses

- Doctor sample-order assistant wording: `fixtures/transcripts/doctor_sample_order_flagship.golden_response.json`
- Patient implicit adverse-event assistant wording: `fixtures/transcripts/patient_implicit_adverse_event.golden_response.json`
- Colleague company-policy assistant wording: `fixtures/transcripts/colleague_company_policy.golden_response.json`

## Recommended Starting Points

- RAG teams: start with [`docs/rag-pipeline-spec.md`](docs/rag-pipeline-spec.md)
- Agent template teams: start with [`briefs/agent-cookie-cutters.md`](briefs/agent-cookie-cutters.md)
- Scaffold teams: start with [`briefs/project-scaffold.md`](briefs/project-scaffold.md)
- Evals and guardrails teams: start with [`docs/asset-mapping.md`](docs/asset-mapping.md)

## Initial Release Notes

See [`docs/release-notes-v0.1.0.md`](docs/release-notes-v0.1.0.md) for a GitHub-ready first release summary.
