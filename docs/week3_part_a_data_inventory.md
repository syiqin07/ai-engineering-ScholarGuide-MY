# ScholarGuide-MY — Week 3 Part A: Data Source Selection, Ingestion and Inspection

**Project:** ScholarGuide-MY  
**Part:** Topic 3 — Part A  
**Prepared:** 16 November 2026  
**Selected data pathway for the next stage:** JSON / structured records  
**Related design:** Topic 2 requirements and V1 Retrieval-Augmented Generation (RAG) architecture

---

## 1. Project Context and Purpose

ScholarGuide-MY is a Malaysian student funding information assistant designed to help pre-university students and undergraduates discover and understand scholarships, education sponsorships and education loans. Relevant information is distributed across ministry and agency websites, which may present programme details in different formats and may be updated at different times.

The purpose of Part A is to identify suitable public information sources, inspect representative content, and document each source's ownership, format, relevance, freshness, accessibility and limitations. The findings will guide the data preparation pathway selected for Part B and the executable data pipeline developed in Part C.

ScholarGuide-MY follows the Topic 2 Retrieval-Augmented Generation (RAG) architecture. Therefore, the data collected for V1 must preserve source identity and evidence so that later answers can be linked back to official information. The system is informational only; funding agencies remain the authority for current programme requirements and eligibility decisions.

### 1.1 Topic 2 requirement alignment

| Topic 2 requirement | How Part A supports it |
|---|---|
| FR-02 — Retrieve relevant passages | Identifies the official content from which searchable records and passages can later be prepared. |
| FR-03 — Generate grounded answers | Prioritises source material that contains relevant funding facts and conditions. |
| FR-04 — Display citations | Records source titles and URLs for traceability. |
| FR-05 — Handle insufficient evidence | Documents sources that are incomplete, general, inaccessible or unsuitable for detailed claims. |
| FR-06 — Preserve metadata | Defines source identity, owner, URL and date information to retain. |
| FR-07 — Update the index | Identifies source pages that can be revisited when information changes. |
| NFR-06 / NFR-07 | Supports reproducibility and traceability through a documented source inventory. |
| RAI-01 / RAI-02 / RAI-06 | Supports grounded answers, freshness checks and user verification through official sources. |

## 2. Source Selection Criteria

The following criteria are used to select sources for ScholarGuide-MY V1:

1. **Authority:** Prefer official ministry or funding-agency websites over unofficial summaries, blogs or third-party directories.
2. **Relevance:** The source should contain information related to programme descriptions, study levels, eligibility, financing, application procedures or official programme notices.
3. **Traceability:** Each source must have an identifiable owner, page title and URL.
4. **Inspectability:** The page should be publicly viewable and its relevant content should be readable or structurally inspectable.
5. **Freshness:** Record publication dates, last-modified dates or retrieval/check dates where available. Do not treat a retrieval date as a publication date.
6. **Data structure:** Identify whether the source contains paragraphs, tables, lists, links or other structures that must be preserved during preparation.
7. **Responsible use:** Public access does not automatically grant permission to republish content. Retain attribution, link to the original page and check applicable copyright notices or terms before redistributing copied material.

## 3. Data Source Inventory

The following sources were selected because they are relevant to Malaysian student funding and are published on official agency or ministry websites. The three primary sources inspected are MOHE, PTPTN and MARA. JPA is retained as a candidate source because its page could not be retrieved successfully during this inspection.

| Source ID | Owner | Source title | Format / content type | Intended use | Date / version evidence | Access and rights notes |
|---|---|---|---|---|---|---|
| DS-01 | Ministry of Higher Education (MOHE / KPT), Malaysia | Domestic Sponsorship List | Public HTML directory containing a list/table of notice titles, published dates and links. | Discovery source for domestic scholarships and sponsorship notices; helps users locate the responsible funding agency. | Page displays a last-modified date of 28 September 2026. Visible entries include BYDPA 2026 dated 18 February 2026 and multiple notices dated 27 November 2025. Checked 16 November 2026. | Publicly viewable. The portal displays a copyright notice. No open-data licence was established during this inspection. Retain attribution and source links; do not assume unrestricted republication rights. |
| DS-02 | National Higher Education Fund Corporation (PTPTN) | Pinjaman Pendidikan | Public HTML page containing explanatory text, study-level/institution tables, application conditions and links to related services. | Primary source for PTPTN education-loan purpose, study-level coverage and published application conditions. | No clear publication/version date was identified in the inspected page content. Checked 16 November 2026. | Public information page is viewable. Transactional services such as myPTPTN may require authentication; private accounts are outside this project. No open-data licence was confirmed. |
| DS-03 | Majlis Amanah Rakyat (MARA) | Education Sector | Public HTML page with headings and explanatory paragraphs describing education sponsorship, target group, study levels and division functions. | Source for MARA education-financing context, target group and study-level coverage. It is an overview, not a substitute for programme-specific advertisements. | No explicit publication date, version number or last-modified date was visible in the inspected content. Checked 16 November 2026. | Publicly viewable. No open-data licence was confirmed. Attribute MARA and retain the original URL. |
| DS-04 | Public Service Department (JPA), Malaysia | Education & Training Sponsorship Information Gateway | Official URL was attempted, but the page retrieval returned a server error during this inspection. | Candidate source for JPA sponsorship and education-training information, subject to successful inspection of the page or specific programme notices. | Checked 16 November 2026; page content could not be retrieved, so no publication/version date was verified. | Access could not be verified in this inspection. Do not ingest content based only on a search-result description. Retry later and inspect the actual official page before use. |

### 3.1 Initial source priority

- **Priority 1 — MOHE:** Useful as a central discovery directory for domestic sponsorship notices. It is an aggregator and should not be treated as the final authority for programme-specific eligibility.
- **Priority 1 — PTPTN:** Provides more detailed education-loan information, including study-level coverage and application conditions.
- **Priority 1 — MARA:** Provides an official overview of education sponsorship and the scope of study levels, but detailed requirements must be obtained from the relevant current MARA advertisement.
- **Priority 2 — JPA:** Relevant candidate source, but its page could not be retrieved during this inspection. It should remain pending until its content can be inspected.

Where MOHE lists a programme owned by another agency, the responsible agency's current programme notice should be preferred for detailed conditions. A directory listing should be used for discovery and navigation, not as a replacement for the programme owner's latest terms.

## 4. Source Inspection Evidence

**Inspection date:** 16 November 2026.  
The observations below describe the content visible on the official pages during this inspection. They do not mean that the pages have been downloaded, archived, fully extracted or ingested into a vector database.

### 4.1 IR-01 — MOHE Domestic Sponsorship List

**Source ID:** DS-01  
**Official URL:** https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list

**Observed content:**
- The page presents a directory with the columns **Title** and **Published Date**.
- A visible entry is the Yang di-Pertuan Agong Scholarship (BYDPA) 2026 notice, dated 18 February 2026.
- Other visible entries dated 27 November 2025 include notices associated with Yayasan Sarawak, the Sabah and Selangor state governments, Yayasan Bank Rakyat, Yayasan Peneraju, Telekom Malaysia, Bank Negara Malaysia, Yayasan Tenaga Nasional, Yayasan Khazanah, PETRONAS, MARA, PTPTN and JPA.
- The page displays **Last Modified: 28 September 2026**.

**Inspection assessment:**
- The directory is useful for identifying funding opportunities and navigating to relevant notices.
- Each notice should be stored with its own title, published date and destination URL where available.
- The listing's published date and the page's last-modified date are separate metadata fields and must not be combined.
- The MOHE listing alone may not contain enough detail to answer eligibility, award amount or application-deadline questions. The linked programme notice or responsible agency page should be inspected.

**Potential extraction fields:** `source_id`, `listing_title`, `published_date`, `detail_url`, `listing_page_url`, `retrieved_at`.

**Inspection outcome:** Suitable as a discovery/index source. Detailed programme facts require inspection of linked notices.

### 4.2 IR-02 — PTPTN Pinjaman Pendidikan

**Source ID:** DS-02  
**Official URL:** https://www.ptptn.gov.my/pinjaman-pendidikan/

**Observed content:**
- The page explains that PTPTN education loans support students studying at local higher education institutions and may cover tuition fees and living costs.
- A table associates study levels with institution categories. Visible categories include full-time and part-time diploma, distance-learning diploma, advanced diploma, first degree, master's, doctoral and professional courses.
- The application conditions displayed include Malaysian citizenship, an age limit, income conditions for parents or guardians, an offer from an eligible institution, an approved and registered course, at least one remaining semester and a Simpan SSPN Prime or Simpan SSPN Plus account.
- The page also provides related loan information and links to PTPTN services.

**Inspection assessment:**
- The page contains both explanatory paragraphs and structured tables.
- Table headings and row relationships must be preserved so that a study level is not incorrectly associated with an institution category.
- Eligibility conditions should be represented as separate, traceable statements where practical.
- Numerical thresholds and conditions may change. They must be checked again against the official page before being presented as current.
- The assistant must not make an individual eligibility decision from a partial source record.

**Potential extraction fields:** `source_id`, `section_heading`, `study_level`, `eligible_institution_type`, `eligibility_condition`, `loan_category`, `source_url`, `retrieved_at`.

**Inspection outcome:** Suitable as a primary source for PTPTN loan information, subject to structured extraction and freshness checks.

### 4.3 IR-03 — MARA Education Sector

**Source ID:** DS-03  
**Official URL:** https://www.mara.gov.my/en/index/education-sector/

**Observed content:**
- The Education Sponsorship Division describes educational loan facilities for eligible Bumiputera students.
- The page lists foundation, diploma, bachelor's degree, master's degree, doctorate/PhD, medical specialist and professional-certification levels.
- Local and international institutions are mentioned, subject to MARA advertisements, terms and conditions.
- The page also describes the functions of the Education Sponsorship Division and Higher Education Division.

**Inspection assessment:**
- The page is useful for general agency and education-financing context.
- The stated Bumiputera target group must be retained accurately and must not be generalised to other funding agencies.
- Programme-specific eligibility, deadlines and award amounts require the relevant current MARA advertisement or programme page.
- No publication or last-modified date was visible in the inspected page content. The retrieval/check date should therefore be stored separately.

**Potential extraction fields:** `source_id`, `division`, `funding_type`, `target_group_as_stated`, `study_levels`, `institution_scope`, `conditions_note`, `source_url`, `retrieved_at`.

**Inspection outcome:** Partially suitable. Use for overview information; obtain programme-specific notices before answering detailed application questions.

### 4.4 IR-04 — JPA Education & Training Sponsorship Information Gateway

**Source ID:** DS-04  
**Official URL:** https://www.jpa.gov.my/en/loan-repayment-compensation?catid=26&id=863%3Amicrosite-bmi&view=article

**Inspection result:**
- An attempt was made to retrieve the official page on 16 November 2026.
- The page retrieval returned a server error, so the page content could not be independently inspected.
- No programme details, eligibility conditions or publication date are claimed in this inventory.

**Inspection assessment:**
- JPA remains a relevant candidate source for the project.
- It should not be included in the prepared knowledge records until the actual official page or a specific JPA programme notice can be accessed and inspected.
- A search-result description is not sufficient evidence for extracting programme facts.

**Inspection outcome:** Not ready for ingestion; access verification is pending.

### 4.5 Inspection Status Summary

| Inspection ID | Source | Content inspected | Outcome | Ingestion decision |
|---|---|---|---|---|
| IR-01 | MOHE Domestic Sponsorship List | Listing titles, published dates, linked notices and last-modified date | Useful discovery directory; detailed conditions require linked notices. | Suitable for listing metadata after capture and validation. |
| IR-02 | PTPTN Pinjaman Pendidikan | Loan purpose, study-level table and application conditions | Useful detailed content with table relationships to preserve. | Suitable for structured extraction and validation. |
| IR-03 | MARA Education Sector | Sponsorship overview, target group, study levels and division functions | Useful overview but not a complete programme application notice. | Partially suitable; supplement with current programme notices. |
| IR-04 | JPA gateway | Retrieval attempt only; page returned a server error | Content could not be independently inspected. | Not ready; retry access before ingestion. |

## 5. Proposed Data Ingestion Plan

The ingestion plan follows the Topic 2 RAG architecture and separates source discovery from later data preparation and indexing.

1. **Discover:** Identify official programme pages and relevant notices from the source inventory.
2. **Capture:** Save or extract only content that may be used under applicable website terms. Record the original URL, page title, retrieval timestamp and publication/modified date where available.
3. **Inspect:** Check language, encoding, missing sections, tables, links, repeated content and whether the page is an overview or a programme-specific notice.
4. **Clean:** Remove irrelevant navigation and repeated page elements while retaining meaningful headings, tables, dates, conditions and attribution.
5. **Structure:** Convert tables into labelled records or clearly labelled text. Preserve the relationship between each table heading and its values.
6. **Prepare for retrieval:** Organise content into meaningful sections such as programme overview, eligibility, study level, benefits, required documents, deadlines and application steps. The exact chunking method will be decided and documented during the later preparation stage.
7. **Attach metadata:** Retain `source_id`, `source_owner`, `source_title`, `source_url`, `section`, `language`, `published_date` (if available), `last_modified_date` (if available), `retrieved_at` and `content_type`.
8. **Validate:** Compare prepared records with the official source and verify important numbers, dates, conditions and qualifiers.
9. **Index later:** Generate embeddings and load validated content into a vector index during the subsequent RAG implementation. This is not claimed as completed in Part A.
10. **Refresh:** Revisit official sources before evaluation and whenever a source update is identified. Update affected records and downstream retrieval content.

### 5.1 Proposed Data Organisation

```text
ScholarGuide-MY/
├── data/
│   ├── raw/
│   │   ├── mohe/
│   │   ├── ptptn/
│   │   ├── mara/
│   │   └── jpa/
│   ├── processed/
│   │   ├── source_records.jsonl
│   │   └── scholar_programme_records.jsonl
│   └── metadata/
│       └── source_inventory.csv
└── docs/
    └── week3_data_inventory.md
```

This is a proposed organisation only. Create folders and retain source copies only when they match the actual implementation and applicable access/usage conditions. Do not commit private application records, personal information, credentials or source material that cannot be redistributed.

## 6. Data Quality Checks

The following checks will be used when the source material is captured and prepared.

| Check ID | Quality dimension | Inspection method | Acceptance condition |
|---|---|---|---|
| DQ-01 | Source authority | Verify the domain and page owner. | Official agency/ministry page or clearly identified official programme notice. |
| DQ-02 | Source traceability | Check title, URL and retrieval date. | Every prepared record can be traced to its source URL and retrieval date. |
| DQ-03 | Completeness | Compare extracted sections and table rows with the source. | Key eligibility conditions, dates, amounts and qualifiers are not omitted. |
| DQ-04 | Accuracy | Manually compare representative extracted facts with the original page. | Critical facts in the inspected sample match the source. |
| DQ-05 | Freshness | Revisit source pages and inspect date information. | Time-sensitive details are flagged for re-check and are not presented as guaranteed current. |
| DQ-06 | Duplicate handling | Compare repeated sections and notices. | Duplicates are removed or identified without losing distinct programme conditions. |
| DQ-07 | Table integrity | Compare headings and row relationships. | Each value remains associated with the correct study level, institution category or condition. |
| DQ-08 | Language and encoding | Inspect English/Malay text, symbols and currency values. | Text is readable and key terms, dates and values are preserved. |

## 7. Limitations and Responsible Data Use

- The MOHE domestic list is a discovery directory. Its listing dates do not establish that every linked programme's detailed terms remain current.
- PTPTN and MARA conditions may change. Retrieval dates must not be mistaken for publication or effective dates.
- The MARA page is an overview and should not be used as the sole evidence for programme-specific deadlines or award amounts.
- The JPA page could not be retrieved during this inspection and must be checked again before ingestion.
- Publicly viewable information does not grant access to private application portals or user accounts.
- No open-data licence was confirmed for the inspected pages. Retain attribution and official links, and check copyright notices and terms before redistributing copied content.
- The source inventory is not exhaustive. Other federal, state, university, foundation and private-sector funding providers may be considered after the core V1 workflow is established.
- A programme missing from ScholarGuide-MY must never be interpreted as evidence that a student is ineligible.
- The assistant must communicate uncertainty and direct users to the relevant agency for final confirmation.

## 8. Reflection

The inspection demonstrates that Malaysian student-funding information is distributed across sources with different structures and levels of detail. MOHE provides a useful central directory of sponsorship notices, PTPTN provides more detailed loan information in both paragraphs and tables, while MARA provides an overview of its education-financing scope and target group. JPA is relevant to the project but could not be inspected because the page retrieval returned a server error.

A key data-engineering consideration is preserving the original meaning and structure of official information. For example, PTPTN's study-level table must retain the relationship between each study level and its eligible institution category. Similarly, dates shown in an MOHE listing must remain distinct from the page's last-modified date. The project should also distinguish general agency overviews from programme-specific notices.

For V1, source authority, traceability and freshness are more important than collecting a large quantity of data. The next step is to capture the permitted source information, prepare structured records, validate extracted facts and preserve source metadata. Only after those steps should the validated information be used for the later RAG retrieval pipeline.

## 9. References

1. Ministry of Higher Education (MOHE). *Domestic Sponsorship List*. https://www.mohe.gov.my/en/services/higher-education-funding/sponsorship-by-domestic-agencies-institutions/domestic-scholarship-list
2. National Higher Education Fund Corporation (PTPTN). *Pinjaman Pendidikan*. https://www.ptptn.gov.my/pinjaman-pendidikan/
3. Majlis Amanah Rakyat (MARA). *Education Sector*. https://www.mara.gov.my/en/index/education-sector/
4. Public Service Department (JPA). *Education & Training Sponsorship Information Gateway*. https://www.jpa.gov.my/en/loan-repayment-compensation?catid=26&id=863%3Amicrosite-bmi&view=article

**Inspection date:** 16 November 2026.
