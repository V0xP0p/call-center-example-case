# Brief: Agent Cookie-Cutters

## Objective

Provide reusable templates for agents that appear in the centralized contact-center scenario.

## Required Templates

### Triage Agent

- input: latest user message plus session state
- output: `RouteDecision`
- tools: none by default
- responsibilities: infer persona, classify intent, detect risk flags, choose next step

### Support Agent

- input: route decision, retrieval package, optional tool results
- output: response draft plus trace steps
- tools: lookup tools only when route allows
- responsibilities: answer support, information, or sample-order requests using approved content and allowed tool results

### Safety Intake Agent

- input: safety-sensitive route and current conversation state
- output: required follow-up questions or structured adverse-event record
- tools: `file_safety_report`
- responsibilities: collect minimum reportable information and trigger escalation-safe response behavior, including when a patient implies a possible adverse event without naming it explicitly

### Escalation Agent

- input: route or guardrail decision
- output: handoff-ready summary
- tools: optional support-case creation
- responsibilities: summarize why the case cannot be fully handled automatically

## Minimum Done Definition

- each template references at least one transcript in `fixtures/transcripts/`
- each template aligns with one or more eval rows in `fixtures/evals/`
- tool permissions and guardrail hooks are explicit in the template contract
