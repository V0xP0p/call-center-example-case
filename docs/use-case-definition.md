# Centralized Pharma Contact Center Use-Case Definition

## Purpose

This use-case pack defines a shared, reusable scenario that multiple SDK assets can showcase independently and together. It is designed for teams building:

- project scaffolds
- agent cookie-cutters
- RAG pipelines
- eval suites
- guardrail frameworks

The scenario is intentionally rich enough to exercise routing, retrieval, deterministic tool use, safety workflows, and evaluation, while remaining simple enough to implement with local fixtures only.

## Channel and System Boundaries

- Channel: text chat only
- Deployment target: example and test environments
- Data sources: local files and mock stores only
- Compliance stance: simplified demo policy, but strict safety escalation behavior
- Out of scope: telephony, real regulatory workflows, live databases, EMR integration, CRM integration

## Personas

### Patient

A patient contacts the centralized support center to ask about symptoms, support programs, or case status. The system may retrieve public patient-facing material and use support tools, but must not provide diagnosis or expose internal-only content. If a patient describes a possible adverse event, the system should detect the safety signal even when the patient does not explicitly say they are "reporting an adverse event."

### Doctor

A healthcare professional requests approved information or requests drug samples. The system can verify clinician identity through local mock data, check doctor-specific sample availability, retrieve HCP-appropriate content, and create a mock order form once the doctor confirms product and quantity.

### Colleague

A pharma colleague asks about internal workflow, approval processes, or contact-center operating procedures. The system can retrieve internal-only policy content for this persona.

## Canonical Sub-Scenarios

### 1. Doctor sample order request

Example prompt:

> I am Dr. Rivera. What Cardiolase samples do I have available right now, and can I place an order for some?

Expected behaviors:

- identify likely doctor persona
- verify the contact against local records
- classify as `sample_request`
- check doctor-specific sample inventory from local mock data
- retrieve HCP-safe ordering guidance
- present available products and quantities clearly
- collect doctor choice and requested quantity
- trigger a mock tool that creates a sample-order form

### 2. Patient adverse event intake

Example prompt:

> I started taking Cardiolase and now I am dizzy and vomiting. Is that normal?

Expected behaviors:

- identify patient persona
- infer likely adverse-event reporting intent from symptom language
- route into safety-sensitive intake
- ask required follow-up questions
- file a mock safety report when minimum information is collected
- use approved, non-diagnostic language
- encourage appropriate follow-up and escalation

### 3. Doctor medical information request

Example prompt:

> I am Dr. Rivera. Can you send me the approved information on side effects for Cardiolase and how to report a serious event?

Expected behaviors:

- identify likely doctor persona
- verify the contact against local records
- classify as `medical_information`
- retrieve HCP-safe information
- cite approved sources
- avoid diagnosis or treatment recommendations

### 4. Patient access/support request

Example prompt:

> My doctor said I might qualify for the support program for Cardiolase. What do I need and can you check my case?

Expected behaviors:

- classify as `access_support`
- retrieve patient-support policy
- use mock tool lookup for case status when enough identity is provided
- keep the answer procedural and policy-based

### 5. Internal colleague process/policy question

Example prompt:

> I am on the med info team. Which materials need medical-legal-regulatory approval before they can be shared externally?

Expected behaviors:

- identify colleague persona
- retrieve internal-only documents
- avoid disclosing internal materials to non-colleague personas

## Flagship End-to-End Flow

The flagship scenario is the doctor sample-order flow because it crosses every major asset:

1. Inbound doctor message requests available product samples.
2. Triage agent detects doctor identity cues and order intent.
3. Mock tool verifies the doctor profile.
4. Mock tool looks up doctor-specific sample inventory.
5. RAG retrieves approved sample-ordering guidance.
6. System confirms selected product and quantity with the doctor.
7. Mock tool creates a local sample-order form artifact.
8. Guardrails enforce approved, role-appropriate, citation-backed language.
9. System sends the response and logs a run trace.
10. Eval harness scores route quality, retrieval quality, tool use, grounding, and task success.

## Success Criteria

The use-case pack is successful when:

- each asset team can identify the exact subset they need to implement
- teams can reuse the same fixtures and eval cases across different libraries
- the RAG pipeline can be exercised end to end from raw document sources to retrieval-ready chunks
- the flagship sample-order scenario demonstrates the full SDK story without external dependencies
