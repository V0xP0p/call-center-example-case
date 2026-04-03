# RAG Pipeline Specification

## Goal

Define a complete, first-release RAG pipeline using only local artifacts in this repo. The pipeline must be detailed enough for a team to implement the full lifecycle from raw source files through retrieval-ready evidence and grounded answer packaging.

## Stage 1: Source Registration

Input:

- raw documents under `fixtures/knowledge/sources/`

Required output:

- `fixtures/knowledge/registrations/source_registry.json`

Each registry row should include:

- `doc_id`
- `title`
- `source_path`
- `source_type`
- `audience`
- `product`
- `region`
- `approved_status`
- `effective_date`
- `content_section`
- `sensitivity`
- `citation_label`

## Stage 2: Document Parsing

Goal:

Extract text and structure from heterogeneous local source files.

Supported source patterns in this repo:

- markdown policy docs
- HTML snippets
- TXT exports simulating PDF extraction noise
- CSV tables
- form-like text documents

Expected parser behavior:

- preserve headings and section boundaries
- preserve bullet and numbered list structure
- preserve table rows in a structured or serialized form
- retain form-field labels and values
- capture parser warnings when extraction quality is degraded

Expected output shape:

- parsed document object with `doc_id`, `content_blocks`, `tables`, `warnings`, and `source_metadata`

## Stage 3: Normalization

Goal:

Transform parsed output into a cleaner, retrieval-ready intermediate representation without changing medically meaningful wording.

Normalization expectations:

- collapse repeated whitespace
- repair line breaks that split headings or sentences
- normalize quote characters and punctuation variants
- standardize list markers
- preserve dosage, symptom, seriousness, and reporting terminology exactly where possible
- preserve internal/public sensitivity markers

Expected output shape:

- normalized document object with cleaned `content_blocks`, `normalized_text`, and `normalization_notes`

## Stage 4: Structured Enrichment

Goal:

Attach or infer metadata used later in filtering and ranking.

Required enrichment dimensions:

- audience
- product
- document family
- approved status
- section type
- sensitivity
- citation label

Expected output:

- enriched document record with source metadata plus inferred tags

## Stage 5: Chunking

Goal:

Produce chunks that preserve semantic coherence and citation integrity.

Chunking rules:

- prefer section-aligned chunks over fixed-size slicing
- keep tables or form sections intact when possible
- use overlap only when needed to preserve context
- retain parent document and section lineage
- assign stable `chunk_id` values

Expected chunk fields:

- `chunk_id`
- `doc_id`
- `section_path`
- `text`
- `metadata`
- `citation_label`
- `source_span`

Example chunking targets in this repo:

- HCP side-effect guidance sections
- adverse-event minimum information checklist
- patient support eligibility table
- colleague-only approval workflow steps

## Stage 6: Index Preparation

Goal:

Produce retrieval-ready records independent of any specific vector store or search backend.

Required outputs:

- chunk text
- normalized metadata
- citation metadata
- provenance fields
- ranking/filtering attributes

The repo does not prescribe embeddings or backend choice, but implementations must preserve the shared retrieval contract.

## Stage 7: Retrieval

Goal:

Return chunks relevant to the user request after applying persona and policy filters.

Required behaviors:

- apply audience and sensitivity restrictions before final selection
- allow product-aware filtering
- support top-k retrieval
- surface retrieval rationale when useful for debugging

Examples:

- doctors may retrieve HCP-safe medical information and public reporting policy
- patients may retrieve patient-safe support and safety guidance, but not internal SOPs
- colleagues may retrieve internal policy documents

## Stage 8: Selection / Reranking

Goal:

Choose the evidence set used to support the answer.

Required behaviors:

- prioritize approved and in-scope sources
- reduce near-duplicate chunks
- keep evidence sets compact enough for citation-backed answers
- avoid mixing internal and patient-facing sources in a patient response

## Stage 9: Grounded Response Packaging

Goal:

Produce an answer-ready package that downstream agents can use without having to repeat retrieval logic.

Required output:

- answer outline or synthesis-ready notes
- selected evidence objects
- citation labels
- grounding notes or warnings

This stage should not add unsupported claims; it should package the retrieved evidence for a grounded response.

## Stage 10: Evaluation Hooks

Implementations should expose enough intermediate state to support evaluation at multiple layers:

- parser quality
- normalization fidelity
- chunk quality
- metadata correctness
- retrieval relevance
- forbidden-document exclusion
- grounded response packaging quality

Use `fixtures/evals/rag_pipeline_eval_cases.json` as the baseline validation set.