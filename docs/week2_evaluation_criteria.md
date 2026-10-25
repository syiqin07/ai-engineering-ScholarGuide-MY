```
# ScholarGuide-MY — Topic 2 Evaluation Criteria
```

```
## 1. Evaluation purpose
```

```
This plan checks whether ScholarGuide-MY retrieves useful official evidence,
produces grounded answers, cites sources, handles unknown questions safely, and
responds within a practical time. Evaluation will use a fixed benchmark of at
least 30 questions covering common funding topics and different answerability
levels.
```

# `## 2. Benchmark design` 

```
Create at least 30 questions using the curated MOHE, JPA, MARA, and PTPTN public
sources where applicable. Include:
- **Answerable questions:** The source collection contains enough evidence to
answer.
- **Unanswerable questions:** The collection does not contain the requested
fact.
- **Ambiguous questions:** The question lacks important context or uses unclear
wording.
```

```
- **Source-sensitive questions:** Questions about deadlines, eligibility,
required documents, or application steps where a wrong answer could mislead a
student.
```

```
For each question, record an ID, question text, expected answerability, relevant
source/document, expected evidence or key facts, and observed system output. Use
the same benchmark when comparing changes to retrieval or prompting.
```

# `## 3. Evaluation criteria table` 

```
| ID | Criterion | Measurement method | Metric | Target threshold | Expected
outcome |
|---|---|---|---|---|---|
```

```
| EC-01 | Retrieval relevance | Manually label whether a relevant evidence chunk
appears in the top 5 retrieved chunks for each answerable question. | Recall@5 =
answerable questions with relevant evidence in top 5 / total answerable
questions | ≥ 0.80 | Most answerable questions retrieve evidence that can
support a response. |
```

```
| EC-02 | Answer correctness | Compare each answerable response with the
expected facts in the source; score key facts as correct or incorrect. |
Correctness rate = responses meeting the expected-fact checklist / answerable
questions | ≥ 0.80 | Answers communicate the main supported facts without
important errors. |
```

```
| EC-03 | Groundedness / unsupported claims | Review factual claims and mark
whether each is supported by retrieved evidence. | Supported-claim rate =
supported factual claims / all factual claims reviewed | ≥ 0.90 | Most factual
claims can be traced to retrieved evidence; unsupported claims are rare. |
| EC-04 | Citation validity | Open each citation and verify that it points to a
relevant, accessible source supporting the answer. | Valid citation rate =
answers with valid supporting citations / answers requiring citations | ≥ 0.90 |
Users can verify most funding-related answers through official sources. |
| EC-05 | Safe abstention | Test unanswerable and ambiguous questions and check
whether the system clearly admits insufficient evidence rather than guessing. |
Safe-abstention rate = unanswerable tests correctly declined or qualified / all
unanswerable tests | ≥ 0.90 | The assistant is transparent when it cannot
reliably answer. |
```

```
| EC-06 | Source coverage and traceability | Inspect the indexed-source
inventory and metadata for each source used in the benchmark. | Metadata
completeness = sources with title, URL, and check/retrieval date / indexed
sources | 100% for required fields | Sources used by the prototype can be
identified and revisited. |
```

```
| EC-07 | Response time | Measure end-to-end time from submitting a question to
receiving the answer over the benchmark; report median and 95th percentile. |
Median latency and P95 latency in seconds | Median ≤ 8 seconds under normal test
conditions; report P95 without hiding outliers | The prototype is responsive
enough for a demonstration and performance limitations are visible. |
```

```
| EC-08 | Robustness | Submit empty input, unsupported topics, malformed
requests, and simulate missing/retrieval-error cases where feasible. |
Successful graceful handling = tests with helpful error/limitation message /
negative tests | ≥ 0.90 | Common failure cases do not expose raw errors or cause
avoidable crashes. |
```

```
| EC-09 | Plain-language usability | Ask 3–5 volunteer reviewers, if available,
to rate clarity and usefulness on a 1–5 scale after trying representative
questions. | Mean clarity rating; report number of reviewers | Mean ≥ 4.0/5; if
fewer than 3 reviewers, label findings as preliminary | Answers are
understandable to the intended student audience. |
```

```
| EC-10 | Responsible AI disclosure | Inspect the interface and sample answers
for an AI/limitation notice, official-source reminder, and absence of
unsupported eligibility guarantees. | Checklist pass rate | 100% of required
disclosure checks pass | Users understand that the tool is informational and
that agencies remain authoritative. |
```

# `## 4. Scoring procedure` 

`1. Freeze and document the benchmark before the final evaluation run.` 

```
2. Label answerability and expected evidence before reviewing the model output.
```

```
3. Run each question using the same version of the application and source index.
4. Save the question, retrieved evidence, answer, citations, latency, and
evaluation labels. Avoid recording unnecessary personal information.
```

```
5. Calculate each metric using the definitions in the table. Report the
numerator and denominator as well as the percentage.
```

```
6. Investigate failures, group them by cause (retrieval, source quality,
generation, citation, or interface), and record changes made.
```

```
7. Re-run the benchmark after changes. Keep the earlier result so improvements
and regressions remain visible.
```

# `## 5. Result reporting template` 

```
Complete this table after running the prototype; do not fill it with invented
results.
```

```
| Criterion | Actual result | Pass/needs work | Evidence or notes |
|---|---|---|---|
```

```
| EC-01 Retrieval Recall@5 | To be measured | Pending | Record relevant-hit
count and answerable-question count. |
```

```
| EC-02 Answer correctness | To be measured | Pending | Record correct responses
and common errors. |
```

```
| EC-03 Supported-claim rate | To be measured | Pending | Record claims reviewed
and claims supported. |
```

```
| EC-04 Valid citation rate | To be measured | Pending | Record valid citations
and answers requiring citations. |
```

```
| EC-05 Safe-abstention rate | To be measured | Pending | Record unanswerable
tests correctly handled. |
```

```
| EC-06 Metadata completeness | To be measured | Pending | Record complete
source records / indexed sources. |
```

```
| EC-07 Latency | To be measured | Pending | Report median and P95 in seconds
and test environment. |
```

```
| EC-08 Robustness | To be measured | Pending | Record negative tests passed /
total negative tests. |
```

```
| EC-09 Usability | To be measured | Pending | Record reviewer count and average
score, if collected. |
```

```
| EC-10 Responsible AI disclosure | To be measured | Pending | Attach checklist
or screenshots as evidence. |
```

# `## 6. Reflection` 

```
The evaluation plan measures more than whether the assistant produces fluent
text. Retrieval quality, factual correctness, groundedness, citation validity,
and safe abstention are especially important because funding information can
affect a student's decisions. A small benchmark of 30 or more questions is
feasible for a semester project, but it cannot prove that the system works for
every programme or user. Manual review also introduces judgement, so expected
facts and scoring rules should be written down before outputs are scored. The
```

```
thresholds are targets, not claimed results; actual performance must be reported
after testing.
```

