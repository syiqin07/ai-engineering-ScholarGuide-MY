# Week 1 Project Scope — ScholarGuide MY

## Project Name
**ScholarGuide MY — Malaysian Student Funding Information Assistant**

## Domain
Higher Education / Student Financial Aid Information.

## Problem Statement
Malaysian students may need to check several official websites to understand education funding options, eligibility conditions, required documents, application procedures, and deadlines. The Ministry of Higher Education (MOHE) sponsorship directory links to different agencies and programmes, including JPA, MARA, and PTPTN, so students may need to compare information across separate pages.

ScholarGuide MY will be a document-grounded question-answering assistant that retrieves information from a small collection of official public sources, answers in clear language, and provides source links. It will explain published requirements only; it will not decide whether a student will be awarded funding or submit applications.

## Evidence of the Problem
- **Evidence 1:** MOHE's official “Sponsorship by Domestic Agencies / Institutions” page lists multiple funding routes and links to individual agency information, including JPA, MARA, and PTPTN. This shows that relevant information is distributed across multiple official pages.
  - Source: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions
- **Evidence 2:** Lewis et al. (2020) describe limitations of language models in knowledge-intensive tasks, including hallucination and provenance challenges. This supports using retrieved official sources and evaluating groundedness instead of relying on an ungrounded chatbot.
  - Source: https://arxiv.org/abs/2005.11401

**Evidence limitation:** These sources establish that information is distributed and that grounded answers are an engineering concern. They do not prove how often students experience difficulty. Validate the user problem later with a small survey or interviews.

## Initial Related Work / Existing Approaches (at least 3)

1. **MOHE domestic sponsorship directory**
   - Source: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions
   - Strength: Central directory linking to several official funding routes.
   - Key limitation: Students may still need to open and compare separate agency or programme pages.

2. **Individual official agency pages (JPA, MARA and PTPTN)**
   - JPA: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/program-penajaan-jabatan-perkhidmatan-awam
   - MARA: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/kemudahan-penajaan-majlis-amanah-rakyat-mara
   - PTPTN: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/pinjaman-pendidikan-perbadanan-tabung-pendidikan-tinggi-nasional
   - Strength: Official programme and funding information.
   - Key limitation: Information is separated by agency and may have different page layouts, terminology, and update schedules.

3. **Retrieval-Augmented Generation (RAG), Lewis et al. (2020)**
   - Source: https://arxiv.org/abs/2005.11401
   - Contribution: Describes combining a language model with retrieved external documents for knowledge-intensive question answering.
   - Key limitation: Retrieval does not guarantee correctness; the system still needs reliable sources, citations, testing, and safe handling of questions not answered by the documents.

4. **Ragas evaluation metrics**
   - Source: https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/
   - Contribution: Describes measures such as faithfulness, response relevancy, context precision, and context recall.
   - Key limitation: Automated metrics do not replace checking answers against official rules; a human-reviewed test set is still needed.

## Engineering Gap / Limitation
Official funding information is available but spread across multiple pages. A general-purpose chatbot may also provide unsupported or outdated claims. The proposed gap is a small assistant that retrieves answers from a controlled collection of official sources, cites evidence for its answers, displays source/update dates where available, and says when the answer cannot be found in the selected sources.

## Initial Project Objectives
- **O1:** Collect, clean, and organise at least three publicly accessible official funding information sources from MOHE, JPA, MARA, and/or PTPTN.
- **O2:** Develop a retrieval-augmented question-answering prototype that answers common funding questions and provides source links or citations.
- **O3:** Evaluate the prototype using at least 30 human-checked questions, measuring answer correctness, citation correctness, unanswered-question handling, and response time.

## Target Users
Malaysian pre-university and undergraduate students exploring public education funding options, eligibility requirements, application steps, and required documents.

## Data Sources (at least 2)
1. **MOHE — Sponsorship by Domestic Agencies / Institutions**
   - URL: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions
2. **MOHE — JPA Sponsorship Programme information**
   - URL: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/program-penajaan-jabatan-perkhidmatan-awam
3. **MOHE — MARA Sponsorship information**
   - URL: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/kemudahan-penajaan-majlis-amanah-rakyat-mara
4. **MOHE — PTPTN Education Loan information**
   - URL: https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list/pinjaman-pendidikan-perbadanan-tabung-pendidikan-tinggi-nasional

Record each source title, URL, access date, and displayed update date. Check permission and terms before redistributing downloaded documents.

## Similar to EduAdvisor
- Both use retrieval from documents and natural-language questions.
- Both should ground answers in controlled sources and use guardrails when information is missing.

## Different from EduAdvisor
- ScholarGuide MY focuses on student funding and sponsorship, not course prerequisites, academic policies, or graduation requirements.
- Answers should include agency/programme links and distinguish scholarships, sponsorships, and loans.
- It will not make funding decisions, guarantee eligibility, or submit applications.

## Responsible AI Risks
1. **Hallucination / incorrect eligibility information:** A student may misunderstand a requirement or miss an opportunity. Mitigation: ground answers in official text, show citations, test against verified answers, and abstain when evidence is missing.
2. **Outdated information:** Deadlines and requirements can change. Mitigation: display source/update dates, check sources regularly, and direct users to official pages for confirmation.
3. **Privacy:** Students may enter personal or financial details. Mitigation: do not request identity numbers, passwords, bank details, or confidential documents; avoid storing chat history or personal profiles.
4. **Fairness / misleading suitability:** A broad answer may omit conditions relevant to a student. Mitigation: present published requirements, ask only non-sensitive clarifying questions, and never guarantee qualification.

## Initial Evaluation Plan
- Prepare at least 30 questions with reference answers checked against official sources.
- Include answerable questions, questions whose answers are absent from the documents, and questions about similar programmes.
- Measure answer correctness, citation correctness, abstention quality, and latency.
- Review failures manually and document examples in the repository.

## Initial Scope Boundaries
**In scope:** information about a limited set of publicly documented Malaysian funding programmes; natural-language Q&A; source citations; clear “not found in sources” responses.

**Out of scope:** predicting scholarship success, personalised financial advice, autonomous application submission, collecting sensitive personal data, or guaranteeing that a programme is currently open.
