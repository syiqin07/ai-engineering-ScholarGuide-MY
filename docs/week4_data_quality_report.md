# ScholarGuide-MY — Data Quality & Bias Report

**Audit date:** 22 November 2026  |  **Stage:** Topic 3 V1 JSONL pipeline  |  **Decision:** **GO WITH CONDITIONS** (prototype only)

## 1. Project and Data Overview

ScholarGuide-MY is an informational assistant for Malaysian pre-university and undergraduate students exploring scholarships, sponsorships and education loans. This audit checks whether the prepared data can support source-linked answers and responsible handling of missing evidence.

| Topic 2 requirement | Requirement and data needed |
|---|---|
| **FR-04 — Source citations** | Factual funding answers must display citations/clickable source links. Needed: `record_id`, `source_id`, `source_url`, and matching source metadata (`publisher`, `source_title`, canonical `url`). |
| **FR-05 — Insufficient evidence** | The system must indicate insufficient evidence and not present unverified answers as fact. Needed: `eligibility_summary`, `award_summary`, `published_date`, `application_status`, `evidence_note`, and `verification_required`; unknown values must remain explicit. |

**Pathway:** Topic 3 Pathway D (JSON/API), implemented as structured JSONL. **Audited folder:** `data/pipeline_v1/jsonl/`. It contains `prepared_source_records.jsonl` (3 records) and `prepared_opportunity_records.jsonl` (4 records). **Matching manifest:** `pipeline_manifest.json` in the same folder.

The manifest links the outputs to Part B inputs in `data/raw/part_b_jsonl_snapshot/` and upstream files in `data/processed/`, with counts, SHA-256 hashes, settings and source-to-output mapping. It records the pipeline run at `2026-11-15T16:43:41+00:00` (UTC). Sources are MOHE, PTPTN and MARA; JPA is excluded because its candidate page was not successfully inspected in Topic 3.

**Population/sample:** 7 records (3 source + 4 opportunity/catalogue); unit = one JSONL record. All 7 were inspected (census), because fewer than the recommended 10–20 existed. This is not a statistical sample and cannot represent uncollected programmes.

## 2. Five-Dimension Quality Scorecard

| Dimension | Finding, evidence and impact | Decision |
|---|---|---|
| **Completeness** | **Observed:** no missing required keys in 3/3 sources or 4/4 opportunity records. But `eligibility_summary` is null in 2/4, `award_summary` in 3/4 and `published_date` in 3/4. Source `publication_date` is null in 3/3 and `page_last_modified` in 2/3. Exact scheme questions may therefore lack evidence. | **Warning** |
| **Consistency** | **Observed:** JSONL parses; 3/3 source IDs and 4/4 record IDs are unique; all 4 opportunity records resolve to a source, match its URL, use HTTPS, retain `verification_required: true` and have non-empty `retrieval_text`. | **Pass** for structure/linkage |
| **Accuracy** | **Observed / partial:** 22 Nov live-page checks support the high-level MOHE BYDPA, PTPTN loan and MARA financing summaries. But no archived copy of the exact source version used to curate the 16 Nov data exists. Exact historical text fidelity and every detailed eligibility/award claim are **N.V.** | **Warning** |
| **Timeliness** | **Observed:** all source records show access date 16 Nov. Stored MOHE `page_last_modified` is 28 Sep, while the live page showed 13 Nov 2026 on audit. PTPTN/MARA have no recorded page date; 3/4 opportunity `published_date` values are null. Access date alone does not prove currency. | **Review** |
| **Uniqueness** | **Observed:** duplicate-ID lists are empty for both record types. The two MOHE records are intentional (BYDPA notice and directory catalogue). Semantic/content-level duplicate detection was not run (**N.V.**). | **Pass** for IDs; content N.V. |

## 3. Validation Findings

The Topic 3 pipeline reports **9/9 checks passed, 0 failed**. Two project-specific rules were applied to the four opportunity/catalogue records:

| Rule | Checkable condition / requirement | Result | Failure response |
|---|---|---|---|
| **V1 — Source-link integrity** | Every record's `source_id` exists and its `source_url` equals the linked source's `url` (**FR-04**). | **4 tested: 4 pass, 0 fail.** No unresolved IDs or URL mismatches. | Quarantine for correction; do not show factual answers with unresolved/mismatched links. |
| **V2 — Evidence/verification flag** | Each record has non-empty `evidence_note` and `retrieval_text`, and `verification_required == true`; unknown fields remain null, not inferred (**FR-05 / RAI-01**). | **4 tested: 4 pass, 0 fail.** Manual inspection confirms evidence notes; pipeline report confirms text and flag. | Keep for review; do not convert missing details into claims. |

These are data-level checks. They do not test future model abstention, citation correctness for generated claims, or retrieval quality; those remain for later evaluation.

## 4. Duplicate, Anomaly and Coverage Findings

No duplicate IDs or silently dropped records were found (sources 3 → 3; opportunities 4 → 4). Main anomalies are missing scheme details and date metadata, not malformed JSON. Content-level duplicate checking is **N.V.**

| Category | Expected evidence | Observed coverage and likely effect |
|---|---|---|
| **Scholarship/sponsorship notices** | Individual notice, eligibility, study level, award terms and dates. | One individual notice: `OPP-MOHE-BYDPA-2026`. The live MOHE directory showed 14 notices on 22 Nov; only 1/14 appears as an individual opportunity record. `CAT-MOHE-DOMESTIC-LIST` is a directory record, not another scheme. Search may omit most visible notices. |
| **Education loans/financing** | Provider, study levels, eligibility and scheme terms. | Two broad pathways: `OPP-PTPTN-EDULOAN` and `OPP-MARA-EDUFIN`. Both are high-level summaries, not complete scheme specifications. Loan discovery is better represented than scholarship detail. |

The dataset covers only three inspected agencies/pages. It does not represent all Malaysian federal, state, university, foundation or private opportunities.

## 5. Bias Assessment

**Inferred risk — representation/source-selection bias; not proven unfairness.** Sources that were readily inspected were selected. Two records describe loan/financing pathways, while one individual scholarship notice is captured; the MOHE directory currently shows 14 notices but the output contains one individual notice. Other providers and programme groups are absent or unverified.

A later retriever may therefore surface loan information more often and omit scholarships or less-visible providers, potentially making the funding landscape appear narrower than it is. No model fairness or system accuracy test was performed. Mitigation: expand by provider and funding type, track coverage by agency/scheme/study level, disclose collection limits, and never infer ineligibility from a missing result.

## 6. Privacy, Rights and Governance

- **Provenance — Observed:** three source IDs (MOHE, PTPTN, MARA); the matching manifest records input/output paths, counts, SHA-256 hashes and source-to-output mapping. The Part A inventory documents source inspection and limitations.
- **Rights — N.V.:** pages are publicly viewable, but no open-data licence or explicit permission to redistribute copied content in a public GitHub repository was established. Public access is not assumed to grant redistribution rights. Keep attribution and links; do not upload full source documents unless terms allow it.
- **Privacy — Observed:** the seven records describe public agency/programme information; no individual applicant data, personal identifiers or financial account details were identified. Consent for personal-data collection is not applicable to this public-page dataset; this does not resolve content-reuse rights.
- **Safeguard:** keep only non-personal programme/source metadata in the public repository; exclude applicant data and credentials; retain official links and verification flags; show a reminder/abstention where evidence is missing or time-sensitive.
- **Maintenance:** the project maintainer should re-check agency pages before final evaluation and after material notice/date changes, then update records and the manifest.

## 7. Mitigation and Impact on Requirements

1. **Reconcile source accuracy, freshness and rights before indexing.** Revisit MOHE, PTPTN and MARA; compare every summary with current official notices; correct the MOHE modified date; record check dates when sites do not display dates; and review reuse terms. This protects **FR-04, FR-05, FR-06 and RAI-02**. Until complete, exact eligibility/deadline claims remain a risk.
2. **Expand programme-level coverage and re-audit.** Add verified scholarship/sponsorship notices beyond BYDPA, broaden provider coverage and add JPA only after successful inspection. Record scheme-level dates, conditions and links, then rerun V1/V2 and update the manifest. This affects **FR-02, FR-03, FR-04, RAI-01 and RAI-03**. Gaps remain until the additional sources are inspected.

## 8. Quality-Gate Decision

**GO WITH CONDITIONS — for controlled prototype data preparation and development only.** Evidence for limited progress: (1) the pipeline reports 9/9 structural checks passed, including unique IDs, valid source links and record preservation; and (2) V1 and V2 passed for all 4 opportunity/catalogue records. The seven records are usable as a traceable starter dataset for controlled pipeline/retrieval development.

**Conditions:** label the collection incomplete; retain `verification_required: true`; do not present missing/unverified eligibility, award or deadline details as fact; link users to official sources; and finish source-date/rights review and coverage expansion before student-facing funding guidance. This is **not** approval for production use or individual eligibility decisions.

**Re-review trigger:** source dates reconciled, reuse terms documented, additional notices verified, and updated outputs/manifest passing V1/V2. Material agency-page changes also trigger revalidation.

---

## Evidence Appendix — Items and live source checks

**All 7 inspected IDs:** `SRC-MOHE-001`, `SRC-PTPTN-001`, `SRC-MARA-001`, `OPP-MOHE-BYDPA-2026`, `OPP-PTPTN-EDULOAN`, `OPP-MARA-EDUFIN`, `CAT-MOHE-DOMESTIC-LIST`.

**Official pages spot-checked 22 November 2026:**

1. [MOHE — Domestic Scholarship List](https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list): BYDPA 2026 notice dated 18 Feb 2026; 14 notices visible; page reports last modified 13 Nov 2026.
2. [PTPTN — Pinjaman Pendidikan](https://www.ptptn.gov.my/pinjaman-pendidikan/): current page describes the education-loan purpose, study categories and application conditions; the prepared record is a high-level summary only.
3. [MARA — Education Sector](https://www.mara.gov.my/en/index/education-sector/): current page describes education loan facilities for eligible Bumiputera students and study levels from foundation through professional certification.

**Reused evidence:** `data/pipeline_v1/jsonl/inspection_report.json`, `validation_report.json`, `repeatability_report.json` and `pipeline_manifest.json`. No new pipeline or model was created for Topic 4.
