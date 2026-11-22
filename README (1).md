# ScholarGuide MY — Malaysian Student Funding Information Assistant

> A document-grounded assistant that helps Malaysian students understand public education funding information using official sources and source-linked answers.

## Problem Statement

Information about education funding is published across different agency pages. Students may need to navigate and compare eligibility requirements, application steps, required documents, and deadlines. ScholarGuide-MY will retrieve information from a controlled collection of official public sources and provide concise answers with source links.

The prototype is informational only. It does not determine final eligibility, guarantee funding, or submit applications.

## Evidence of the Problem

- MOHE's official domestic sponsorship directory links to multiple funding routes and agencies, including JPA, MARA, and PTPTN: [MOHE Domestic Scholarship List](https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list).
- Lewis et al. (2020) discuss limitations of language models in knowledge-intensive tasks, including hallucination and provenance challenges: [Retrieval-Augmented Generation](https://arxiv.org/abs/2005.11401).

These sources provide initial evidence of distributed information and the need for grounded responses. A student survey/interview should later validate the user problem.

## Initial Related Work / Existing Approaches

- MOHE sponsorship directory: central entry point, but students may still need to compare individual programme pages.
- Individual agency pages (JPA, MARA, PTPTN): official programme details, but separated by agency and page structure.
- Retrieval-Augmented Generation (Lewis et al., 2020): combines retrieved documents with generation, but still needs evaluation and reliable sources.
- [Ragas evaluation metrics](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/): includes faithfulness, response relevancy, context precision and context recall.

## Engineering Gap / Limitation

Official funding information is spread across multiple pages, while a general-purpose chatbot may answer without evidence. This project will retrieve answers from a limited collection of official sources, show citations, and decline to make claims when the sources do not support an answer.

## Project Objectives

- O1: Collect and organise at least three public official funding sources.
- O2: Build a retrieval-augmented Q&A prototype that provides source-linked answers.
- O3: Evaluate at least 30 test questions for answer correctness, citation correctness, abstention quality, and response time.

## Target Users

Malaysian pre-university and undergraduate students exploring public education funding options.

## Data Sources

- [MOHE Domestic Scholarship List](https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list)
- [PTPTN — Pinjaman Pendidikan](https://www.ptptn.gov.my/pinjaman-pendidikan/)
- [MARA — Education Sector](https://www.mara.gov.my/en/index/education-sector/)
- JPA remains a candidate source and should be added only after its page and programme information are successfully inspected.

## Responsible AI Risks

- Hallucination or incorrect eligibility information.
- Outdated deadlines or requirements.
- Privacy risk if users enter personal or financial details.
- Misleading suitability claims that imply guaranteed eligibility.

## Initial Scope

**In scope:** a small collection of official sources, natural-language Q&A, source citations, and a clear “not found in sources” response.

**Out of scope:** predicting award outcomes, guaranteeing eligibility, personalised financial advice, collecting sensitive data, and submitting applications.

## Week 4 — Data Quality & Responsible Data Practices

**Report:** [`docs/week4_data_quality_report.md`](docs/week4_data_quality_report.md)  
**Audited output:** `data/pipeline_v1/jsonl/` (prepared source and opportunity JSONL files).  
**Matching manifest:** `data/pipeline_v1/jsonl/pipeline_manifest.json`  
**Audit date:** 22 November 2026  
**Quality-gate decision:** **GO WITH CONDITIONS** — suitable for controlled prototype data preparation only; not approved as a complete or production-ready source of student funding advice.

### Evidence summary

- Seven available JSONL records were inspected (3 source records and 4 opportunity/catalogue records).
- The Topic 3 pipeline reported 9/9 validation checks passed.
- Topic 4 source-link integrity passed for 4/4 opportunity/catalogue records.
- Topic 4 evidence/verification-flag check passed for 4/4 opportunity/catalogue records.
- Completeness, timeliness and coverage limitations remain; the MOHE directory currently displays more notices than are individually represented in the prepared dataset.

### Outstanding actions

1. Reconcile source accuracy, freshness metadata and reuse permissions against the current official MOHE, PTPTN and MARA pages before final evaluation or public-facing use.
2. Expand verified programme-level scholarship/sponsorship coverage beyond the single BYDPA record, add other providers as inspected, then rerun validation and update the manifest.

Missing programme information must not be interpreted as student ineligibility. Users must be directed to official sources for current requirements and decisions.

## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | System map and project scope | README and project documentation | Documented |
| W2 | Requirements, architecture and evaluation criteria | Requirements and design records | Documented |
| W3 | Data inventory, JSONL preparation and V1 pipeline | Part A-C artefacts, executed outputs and manifest | Prepared |
| W4 | Data quality and responsible data practices | `docs/week4_data_quality_report.md` and PDF submission copy | Report prepared; GitHub commit/verification and e-Learning submission pending |

## About

A focused AI assistant that helps students find and understand scholarship, sponsorship and education loan information from official sources.
