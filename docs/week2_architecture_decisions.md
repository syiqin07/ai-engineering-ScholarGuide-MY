```
# ScholarGuide-MY — V1 Architecture and Design Decisions
```

# `## 1. V1 architecture overview` 

```
ScholarGuide-MY uses a Retrieval-Augmented Generation (RAG) architecture. A
curated set of official public funding sources is processed into searchable
chunks. At query time, the system retrieves relevant chunks, checks whether
evidence is adequate, and asks a language model to draft an answer constrained
by that evidence. The answer is returned with citations and an uncertainty
message when the source collection does not support a reliable response.
```

```
The diagram `week2_architecture_v1.png` shows the main offline ingestion flow
and the online question-answer flow.
```

# `## 2. Main components` 

```
1. **Official source collection:** Selected public pages/documents from relevant
bodies, initially MOHE, JPA, MARA, and PTPTN where relevant.
```

```
2. **Ingestion and cleaning:** Extract text, remove irrelevant formatting, split
content into chunks, and attach source metadata such as title, URL, and check
date.
```

```
3. **Embedding and vector index:** Convert chunks into embeddings and store them
with their metadata for similarity search.
```

```
4. **User question:** Accept a natural-language question from a student.
```

```
5. **Retriever:** Find the top relevant chunks for the question.
```

```
6. **Evidence sufficiency check:** Determine whether the retrieved material
appears relevant enough; otherwise, abstain or ask the user to clarify.
```

```
7. **Grounded answer generation:** Generate a concise answer using retrieved
evidence and instructions not to invent missing facts.
```

```
8. **Citation and response layer:** Present the answer, supporting source links,
and an explicit reminder to verify time-sensitive information on official pages.
9. **Evaluation and maintenance:** Use the fixed benchmark to measure retrieval,
correctness, groundedness, citations, abstention, latency, robustness, and
usability. Refresh the source index when sources change.
```

```
## 3. Architecture Decision Records
```

```
### ADR-01 — Use Retrieval-Augmented Generation instead of a standalone language
model
```

```
**Status:** Accepted for V1.
```

```
**Context:** Funding rules, eligibility conditions, required documents, and
deadlines are factual and can change. A standalone language model may produce
plausible but unsupported details and cannot reliably demonstrate which official
source supports an answer.
```

```
**Decision:** Use RAG: retrieve passages from the curated source collection and
provide those passages to the language model as the evidence for its response.
Display source citations and instruct the model to acknowledge when evidence is
insufficient.
```

```
**Consequences and trade-offs:**
```

```
- **Benefits:** Answers can be traced to selected sources; source content can be
updated without retraining the language model; retrieval and generation errors
can be evaluated separately.
```

```
- **Costs:** Requires source ingestion, chunking, embedding, retrieval
configuration, metadata management, and additional latency.
```

```
- **Risks:** Poor retrieval can still lead to a wrong or incomplete answer; a
citation may be present but not truly support a claim.
- **Mitigation:** Evaluate Recall@5, supported-claim rate, citation validity,
and safe abstention; inspect failed examples and improve chunking or retrieval.
```

```
**Alternatives considered:** A standalone LLM is simpler but has weaker source
traceability. Fine-tuning could teach a style or task pattern, but it is less
suitable as the primary way to keep changing funding facts current and would
```

```
require additional data and resources.
```

```
### ADR-02 — Restrict V1 to a curated collection of official public sources
```

```
**Status:** Accepted for V1.
```

```
**Context:** Funding information is spread across multiple websites. Broad,
uncontrolled web search would make it difficult to guarantee source quality,
reproduce tests, or finish within a semester.
```

```
**Decision:** Start with a documented and manageable set of official public
sources, initially including relevant MOHE, JPA, MARA, and PTPTN materials.
Record the source title, URL, and last-checked or retrieval date where
available. Expand coverage only after the core workflow is evaluated.
```

```
**Consequences and trade-offs:**
```

- `**Benefits:** Improves provenance, repeatability, and scope control; makes it easier to inspect source content and reproduce the benchmark.` 

```
- **Costs:** The system may not know about a programme that is not in the
curated collection; source updates require a refresh process.
- **Risks:** A limited source set may create an impression of complete coverage.
- **Mitigation:** Disclose coverage limits, do not interpret missing results as
ineligibility, and direct users to official agency sites.
```

```
**Alternatives considered:** Search the entire web at query time. This may
increase coverage but introduces more variable sources, harder-to-reproduce
results, and greater risk of unreliable pages. It can be considered later with
source filtering and evaluation.
```

```
### ADR-03 — Prefer abstention over unsupported eligibility or deadline claims
```

```
**Status:** Accepted for V1.
```

```
**Context:** A confident but wrong statement about eligibility or a deadline
could cause a student to miss funding or take the wrong action.
```

```
**Decision:** If the retrieved evidence is missing, weak, contradictory, or too
old to support a time-sensitive claim, the assistant should say that it cannot
confirm the detail, show any relevant source links, and recommend checking the
official agency page.
```

```
**Consequences and trade-offs:**
```

- `**Benefits:** Reduces the risk of misleading answers and communicates uncertainty honestly.` 

- `**Costs:** Some questions may receive a less complete answer even when a general model might guess.` 

```
- **Mitigation:** Improve retrieval and source freshness, and evaluate safe-
abstention rate alongside answer correctness so that the system is neither
overconfident nor unnecessarily unhelpful.
```

```
## 4. Trade-off summary
```

```
| Decision | Chosen option | Main benefit | Main trade-off |
```

- `|---|---|---|---|` 

- `| Answer generation | RAG with retrieved evidence | Traceability and updateable knowledge | More components and retrieval failure modes |` 

```
| Source coverage | Curated official sources | More reproducible and trustworthy
source set | Limited coverage and manual refresh effort |
```

```
| Insufficient evidence | Abstain or qualify answer | Safer and more transparent
responses | May decline some questions that could be answered after
clarification |
```

```
## 5. Traceability matrix
```

```
| Requirement ID | Design component or decision | Verification method |
Evaluation link |
```

```
|---|---|---|---|
```

- `| FR-01 | User question interface | Submit valid natural-language questions | EC-08 robustness; functional smoke tests |` 

- `| FR-02 | Retriever and vector index | Check whether expected evidence appears in top 5 | EC-01 Recall@5 |` 

- `| FR-03 | Grounded answer generation | Compare factual claims with retrieved evidence | EC-02 correctness; EC-03 supported-claim rate |` 

- `| FR-04 | Citation and response layer | Open links and verify that sources support claims | EC-04 citation validity |` 

- `| FR-05 | Evidence sufficiency check and abstention prompt | Run unanswerable and ambiguous questions | EC-05 safe-abstention rate |` 

- `| FR-06 | Ingestion metadata | Inspect title, URL, and date fields for each source | EC-06 metadata completeness |` 

- `| FR-07 | Source refresh and index rebuild process | Update a sample source and rebuild index | EC-06; documented maintenance test |` 

- `| FR-08 | User-facing disclosure and official-source reminder | Inspect interface and sample answers | EC-10 Responsible AI disclosure |` 

- `| FR-09 | Input validation and error handling | Run empty, unsupported, and failure-case tests | EC-08 robustness |` 

- `| FR-10 | Fixed evaluation benchmark | Inspect benchmark for at least 30 labelled questions | Evaluation reproducibility checklist |` 

- `| NFR-01 / RAI-01 | RAG, evidence check, abstention | Review claims and unsupported answers | EC-02, EC-03, EC-05 |` 

- `| NFR-02 | Retrieval/generation pipeline | Record end-to-end response times | EC-07 latency |` 

- `| NFR-03 / NFR-08 | Plain-language response and readable interface | Reviewer ratings and interface inspection | EC-09 usability |` 

- `| NFR-05 / NFR-06 | Modular code, source inventory, benchmark, setup docs | Follow documented setup and rebuild steps | Reproducibility check |` 

- `| RAI-02 | Source metadata and refresh process | Check dates and verify timesensitive facts on source pages | EC-06; source-freshness review |` 

- `| RAI-03 / RAI-06 | Coverage disclosure and official links | Inspect responses for limitations and verification guidance | EC-04 and EC-10 |` 

- `| RAI-04 / RAI-07 | Minimal data collection and secret management | Review stored data, repository, and configuration | Privacy/security checklist | | RAI-08 | Error log and benchmark review | Inspect recorded failures and corrective actions | Evaluation report |` 

# `## 6. Reflection` 

```
The V1 architecture separates the source-ingestion workflow from the user-facing
question-answer workflow. This makes the system easier to test: a wrong response
can be investigated as a source-quality problem, a retrieval problem, or a
generation problem. RAG was selected because the project needs source
traceability and the ability to refresh funding information without retraining a
model. The main trade-off is added complexity, so the first version deliberately
limits the source collection and uses a small, repeatable benchmark. The design
is not considered reliable merely because the diagram is complete; the
evaluation results must show whether citations are valid, claims are grounded,
and unsupported questions are handled safely.
```

