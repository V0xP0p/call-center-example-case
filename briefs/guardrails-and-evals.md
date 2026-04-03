# Brief: Guardrails And Evals

## Objective

Build reusable policy checks and scoring harnesses against the shared transcripts, retrieval cases, and trace contracts.

## Guardrail Focus Areas

- block diagnosis or treatment recommendations
- require citations for knowledge-grounded content
- prevent internal-only content leakage
- escalate incomplete adverse-event intake
- catch unsupported synthesis
- avoid PII leakage in summaries or responses

## Eval Focus Areas

- routing accuracy
- retrieval relevance
- forbidden-document exclusion
- tool correctness
- safety completeness
- groundedness
- response-policy compliance

## Minimum Done Definition

- at least one positive and one negative case for each guardrail category
- ability to score the flagship end-to-end path
- output summary that references shared scenario IDs from the eval fixtures