# ScholarGuide MY — Malaysian Student Funding Information Assistant

> A document-grounded assistant that helps Malaysian students understand public education funding information using official sources and source-linked answers.

## Problem Statement
Information about education funding is published across different agency pages. Students may need to navigate and compare eligibility requirements, application steps, required documents, and deadlines. ScholarGuide MY will retrieve information from a controlled collection of official public sources and provide concise answers with source links.

The prototype is informational only. It does not determine final eligibility, guarantee funding, or submit applications.

## Evidence of the Problem
- MOHE's official domestic sponsorship directory links to multiple funding routes and agencies, including JPA, MARA, and PTPTN: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions
- Lewis et al. (2020) discuss limitations of language models in knowledge-intensive tasks, including hallucination and provenance challenges: https://arxiv.org/abs/2005.11401

These sources provide initial evidence of distributed information and the need for grounded responses. A student survey/interview should later validate the user problem.

## Initial Related Work / Existing Approaches
- **MOHE sponsorship directory:** central entry point, but students may still need to compare individual programme pages. https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions
- **Individual agency pages (JPA, MARA, PTPTN):** official programme details, but separated by agency and page structure.
- **Retrieval-Augmented Generation (Lewis et al., 2020):** combines retrieved documents with generation, but still needs evaluation and reliable sources. https://arxiv.org/abs/2005.11401
- **Ragas evaluation metrics:** measures include faithfulness, response relevancy, context precision and context recall. https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/

## Engineering Gap / Limitation
Official funding information is spread across multiple pages, while a general-purpose chatbot may answer without evidence. This project will retrieve answers from a limited collection of official sources, show citations, and decline to make claims when the sources do not support an answer.

## Project Objectives
- O1: Collect and organise at least three public official funding sources.
- O2: Build a retrieval-augmented Q&A prototype that provides source-linked answers.
- O3: Evaluate at least 30 test questions for answer correctness, citation correctness, abstention quality, and response time.

## Target Users
Malaysian pre-university and undergraduate students exploring public education funding options.

## Data Sources
- MOHE sponsorship directory: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions
- MOHE JPA sponsorship information: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/program-penajaan-jabatan-perkhidmatan-awam
- MOHE MARA sponsorship information: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/kemudahan-penajaan-majlis-amanah-rakyat-mara
- MOHE PTPTN loan information: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/pinjaman-pendidikan-perbadanan-tabung-pendidikan-tinggi-nasional

## Responsible AI Risks
- Hallucination or incorrect eligibility information.
- Outdated deadlines or requirements.
- Privacy risk if users enter personal or financial details.
- Misleading suitability claims that imply guaranteed eligibility.

## Initial Scope
**In scope:** a small collection of official sources, natural-language Q&A, source citations, and a clear “not found in sources” response.

**Out of scope:** predicting award outcomes, guaranteeing eligibility, personalised financial advice, collecting sensitive data, and submitting applications.

## Weekly Progress
| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | System map + project scope | README + docs | In progress |
| W2 | Requirements, architecture, evaluation criteria | Updated scope and design | Planned |
| W3+ | Prototype and evaluation | Code, tests, evaluation report | Planned |
