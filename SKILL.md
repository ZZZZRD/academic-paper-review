---
name: academic-paper-review
description: 对已完成论文初稿进行结构化同行评审、完整性检查和声明审计。5角色多视角审阅、参考文献验证和可选的引用声明审计。
version: 1.0.0
author: Hermes Agent (adapted from Imbad0202/academic-research-skills)
license: MIT
domain: research
homepage: https://github.com/ZZZZRD/academic-paper-review
metadata:
  tags:
    - academic
    - paper-review
    - peer-review
    - integrity
    - citation
    - research
---

# Academic Paper Review & Integrity Check

## Overview

Complete post-draft quality assurance pipeline adapted from the [Academic Research Skills](https://github.com/Imbad0202/academic-research-skills) project (v3.8.2). Covers structured multi-perspective peer review (5 reviewer personas), reference/citation integrity verification, and optional claim-faithfulness audit.

**Core philosophy:** AI is the reviewer's assistant, not the author. The goal is to catch errors a human reviewer would spot — fabricated references, logical gaps, methodological flaws — not to rewrite the paper.

## When to Use

**Trigger phrases (Chinese):**
- "审一下我的论文" / "帮我审稿" / "review my paper"
- "帮我检查引用" / "帮我查一下引用是否有问题"
- "帮我做论文审阅" / "完整性检查"
- "run integrity check on this paper"

**Trigger phrases (English):**
- "review my paper" / "peer review" / "manuscript review"
- "check my references" / "verify citations"
- "integrity check" / "claim audit"

**Do NOT use for:**
- Writing the paper from scratch → use general writing capabilities
- Literature search / research → use search + arxiv tools
- Format conversion (MD→LaTeX→PDF) → handle separately
- Proofreading grammar/spelling only → use general text review
- Full research-to-paper pipeline → out of scope

## Workflow Overview (4 Phases)

```
Phase 0: Setup → Read paper, identify field/methodology, configure 5 reviewer personas
    ↓ (user confirmation gate)
Phase 1: Multi-Perspective Review → 5 structured reviews (sequential recommended)
    ↓
Phase 2: Editorial Synthesis → Weighted score, decision letter, revision roadmap
    ↓
Phase 3: Integrity Gate → Reference verification (30% sample, min 10) + 7-mode checklist
    ↓
Phase 4 (opt-in): Claim Audit → Fetch sources for central claims, judge faithfulness
```

---

## Phase 0: Setup & Configuration

### Step 1: Read the Paper

If user pastes text inline, use that directly. If providing a file path, read the file. If the paper is very long (>500 lines), read in sections.

### Step 2: Identify Paper Characteristics

Analyze the paper to determine:
- **Primary discipline** (e.g., Computer Science, Physics, Medicine, Education)
- **Sub-field** (e.g., Fiber Optics, NLP, Cardiology, Higher Education)
- **Research paradigm** (quantitative / qualitative / mixed / theoretical / simulation)
- **Methodology** (experiment / survey / case study / simulation / meta-analysis / review)
- **Paper structure** (IMRaD / thematic review / case study / theoretical analysis)
- **Target venue tier** (top journal / mid-tier / conference / preprint / thesis)
- **Maturity level** (early draft / pre-submission / post-revision / camera-ready)

### Step 3: Configure 5 Reviewer Personas

Generate a reviewer configuration card. Default roles:

| # | Reviewer | Role | Expertise Focus |
|---|----------|------|-----------------|
| 1 | Editor-in-Chief | Overall assessment | Field fit, novelty, significance, presentation |
| 2 | Methodology Reviewer | Technical rigor | Research design, statistics, reproducibility |
| 3 | Domain Reviewer | Literature fit | Related work coverage, theoretical grounding |
| 4 | Perspective Reviewer | Broader impact | Cross-disciplinary relevance, practical implications, ethics |
| 5 | Devil's Advocate | Critical challenge | Core assumptions, logical fallacies, counter-arguments |

Customize each reviewer's expertise area based on the paper's actual domain and sub-field. Present the configuration to the user for confirmation before proceeding.

**User options:**
- Confirm → proceed to Phase 1
- Request adjustments → modify reviewer foci
- Skip review → jump to Phase 3 (integrity check only)
- Add custom requirements

---

## Phase 1: Multi-Perspective Review

### Mode Selection

| Paper Length | Recommended Mode | Why |
|---|---|---|
| < 3000 words | Parallel (sub-agents) | Fast, 3 concurrent streams |
| 3000-8000 words | Sequential | Better depth, full context |
| > 8000 words | Sequential required | Context limit concerns |

### Sequential Mode (default, recommended)

Run each review one at a time. Do each review in order: EIC → Methodology → Domain → Perspective → Devil's Advocate. Use the report template below for each.

### Parallel Mode (short papers only, < 3000 words)

Use sub-agent delegation with 3 parallel streams:
1. **Stream A** — Produce both EIC + Methodology reports
2. **Stream B** — Produce both Domain + Perspective reports
3. **Stream C** — Produce Devil's Advocate report

Pass the full paper text and reviewer instructions. Collect all 3 results, then proceed to Phase 2.

### Review Report Template (per reviewer)

Each report uses this structure:

```
## [Reviewer Name] — Review Report

**Score:** [0-100]
**Verdict:** [Accept / Minor Revision / Major Revision / Reject]

### 1. Summary (2-3 sentences)
Overall impression.

### 2. Strengths
- Specific strength, tied to paper content
- ...

### 3. Weaknesses
- Specific weakness, with evidence from paper
- ...

### 4. Specific Comments
| Location | Issue | Severity | Suggested Fix |
|----------|-------|----------|---------------|
| §2.3, para 2 | ... | Critical/Major/Minor | ... |

### 5. Detailed Scoring

| Criterion | Score (0-100) | Comments |
|-----------|---------------|----------|
| Originality/Novelty | | |
| Technical Rigor | | |
| Clarity/Presentation | | |
| Literature Coverage | | |
| Significance/Impact | | |
| **Overall** | **Average** | |
```

### Reviewer-Specific Guidance

#### Editor-in-Chief (EIC)
Focus on: fit to field, novelty of contribution, overall quality, presentation standards. Consider: Does this advance the field? Who is the audience? Is this worth publishing? Be the gatekeeper — assess the big picture.

#### Methodology Reviewer
Focus on: research design validity, statistical methods, sample size/power justification, reproducibility, control variables, potential confounds. Scrutinize the Methods/Experimental section most carefully. Flag missing methodological details that would prevent replication.

#### Domain Reviewer
Focus on: related work coverage, theoretical framework, domain-specific conventions. Does the paper properly cite and engage with key prior work? Are claims appropriately contextualized? Is the contribution clearly distinguished from existing literature? Know the field's canonical references.

#### Perspective Reviewer
Focus on: cross-disciplinary relevance, practical implications, ethical considerations, generalizability, societal impact. What would someone outside this sub-field think? Are limitations and future work properly discussed?

#### Devil's Advocate
Focus on challenging core assumptions, identifying logical fallacies (straw man, false dichotomy, hasty generalization, circular reasoning, post-hoc ergo propter hoc), checking for confirmation bias, testing alternative explanations, proposing the strongest counter-argument to the central claim. Ask: "If we assume the opposite conclusion, what would the evidence look like?" and "So what — even if true, does this matter?"

> **Key principle:** Every critique must be grounded in specific paper content. Generic criticism ("lack of novelty") without location or evidence is not acceptable.

---

## Phase 2: Editorial Synthesis

After all 5 reviews are complete, produce the editorial decision package.

### Score Aggregation

Weighted average:
- Editor-in-Chief: 25%
- Methodology Reviewer: 25%
- Domain Reviewer: 20%
- Perspective Reviewer: 15%
- Devil's Advocate: 15%

**Decision thresholds:**
| Weighted Score | Decision |
|----------------|----------|
| ≥ 80 | Accept |
| 65-79 | Minor Revision |
| 50-64 | Major Revision |
| < 50 | Reject |

### Editorial Decision Package Structure

```
# Editorial Decision

**Decision:** [Accept / Minor Revision / Major Revision / Reject]
**Weighted Score:** [XX/100]
**Consensus Level:** [Strong / Moderate / Divided]

## Decision Letter
[Formatted as a formal journal editor's letter to the authors]

## Cross-Review Analysis

### Points of Consensus
- What all or most reviewers agree on (strengths and weaknesses)

### Points of Disagreement
- Where reviewers diverged and possible reasons

### Critical Issues (must address)
- Issues flagged as Critical by any reviewer, with editorial judgment

### Revision Roadmap (for Revise decisions)

| Priority | Issue | Suggested Action | Section |
|----------|-------|------------------|---------|
| P1 | ... | ... | ... |
| P2 | ... | ... | ... |
| P3 | ... | ... | ... |

## Score Summary

| Reviewer | Score | Verdict |
|----------|-------|---------|
| Editor-in-Chief | | |
| Methodology Reviewer | | |
| Domain Reviewer | | |
| Perspective Reviewer | | |
| Devil's Advocate | | |
| **Weighted Total** | | |
```

---

## Phase 3: Integrity Gate (Reference Verification)

### Sampling Strategy

- ≤ 30 references: check ALL
- > 30 references: sample 30% (min 10, max 30)
- Prioritize: references supporting central claims, recent (< 5 years), non-mainstream venues (harder to verify = more suspicious)

### Verification Method

For each sampled reference, use web search to verify existence. Search by author, year, title key phrase, and venue.

For each reference, record:
```
REF: [Author(s), Year, Title, Venue]
STATUS: VERIFIED / NOT_FOUND / PARTIAL_MATCH
CONFIDENCE: High / Medium / Low
NOTE: Any discrepancies (wrong author, wrong year, wrong venue, fabricated title)
```

### 7-Mode Integrity Checklist

After reference verification, run this full checklist. **Each check must produce PASS or FAIL with evidence.**

1. **REF-1: Reference Existence** — Do all sampled references actually exist? Fabricated refs = automatic FAIL.
2. **REF-2: Citation Relevance** — Does the paper's claim about the source align with what the source actually says? (Use your best judgment; if the claim seems misaligned, flag it.)
3. **REF-3: Data Integrity** — Are numerical results internally consistent? Check: do p-values match stated significance, do sample sizes in Methods match Results, do percentages add up?
4. **REF-4: Logical Consistency** — Does the argument flow logically? Any contradictions between sections (e.g., Abstract says X but Discussion argues Y)?
5. **REF-5: Methodology Alignment** — Does the method used actually answer the research question posed? Are there obvious methodological mismatches?
6. **REF-6: Figure/Table Accuracy** — Do figures and tables match the text descriptions? Are axis labels, units, and captions consistent?
7. **REF-7: Disclosure Completeness** — Are limitations, conflicts of interest, funding sources, and (if applicable) ethics approval acknowledged?

### Integrity Report Template

```
# Integrity Verification Report

**Mode:** Pre-Review

## Reference Verification (Sampled [X]/[Total])

| # | Reference | Status | Confidence | Note |
|---|-----------|--------|------------|------|
| 1 | Smith et al. 2023 ... | VERIFIED | High | |
| ... | | | | |

Summary: [X] verified, [Y] not found, [Z] partial match

## 7-Mode Checklist

| Check | Status | Evidence |
|-------|--------|----------|
| REF-1 Existence | PASS/FAIL | ... |
| REF-2 Relevance | PASS/FAIL | ... |
| REF-3 Data | PASS/FAIL | ... |
| REF-4 Logic | PASS/FAIL | ... |
| REF-5 Methodology | PASS/FAIL | ... |
| REF-6 Figures | PASS/FAIL | ... |
| REF-7 Disclosure | PASS/FAIL | ... |

**Overall:** PASS / FAIL
**SUSPECTED:** [List items requiring user attention]
```

### On FAIL

If the integrity gate FAILS:
1. Present the full report to the user
2. Do NOT block — let the user decide whether to act on findings
3. Offer to re-run after user fixes the flagged issues

---

## Phase 4: Claim Audit (Optional, Opt-In)

### Trigger Conditions

Only run if:
1. User explicitly requests it ("verify claims", "check citations", "做声明审计")
2. OR Phase 3 found > 3 SUSPECTED items
3. OR user asks for "full audit" / "完整审计"

### Method

Identify the paper's 3-5 central claims from Abstract and Conclusion that cite a specific reference. For each:

1. Attempt to fetch the source using web extraction:
   - Search for the paper title/DOI to find the URL
   - If the full text is behind a paywall, extract the abstract
2. Compare the paper's claim against the source content
3. Judge faithfulness level

### Claim Audit Report Template

```
## Claim Audit Report

| # | Claim in Paper | Cited Source | Actual Source Content | Verdict | Confidence |
|---|----------------|--------------|----------------------|---------|------------|
| 1 | "Smith (2023) found that X causes Y" | Smith et al. 2023, Nature | "X is correlated with Y under Z conditions" | PARTIAL | High |

### HIGH-WARN Findings
- [ ] CLAIM-NOT-SUPPORTED — Claim goes beyond what source actually says
- [ ] FABRICATED-REFERENCE — Source does not exist at all
- [ ] MISATTRIBUTION — Claim attributed to wrong source
- [ ] OVERGENERALIZATION — Source supports narrower claim
- [ ] OUTDATED-EVIDENCE — Source superseded by newer findings
- [ ] UNVERIFIABLE — Source not accessible (paywall/dead link)
```

---

## Common Pitfalls

1. **Skipping Phase 0 user confirmation.** Always present the reviewer configuration and get explicit confirmation before Phase 1. The user may want to adjust reviewer foci.

2. **Reviewing without reading the full paper.** Do not start Phase 1 until the entire paper has been read — including abstract, all sections, figure captions, and references.

3. **Devil's Advocate being too polite.** The DA must genuinely challenge core assumptions, not just add mild critiques. If all reviews are positive and similar, the DA isn't doing its job. A proper DA review should feel slightly uncomfortable.

4. **Claim audit without actual source fetching.** If you cannot access the original source, note "UNVERIFIABLE" — do NOT assume support based on the paper's own description of the source.

5. **Insufficient reference sampling.** Always check at least 10 references. For papers with 50+ references, increase sample to 20+.

6. **Mixing integrity check with review content.** Phase 1 (review) and Phase 3 (integrity) are separate processes. Do not pre-judge reference quality during the review phase.

7. **Using sub-agents for long papers in Phase 1.** If paper > 3000 words, use sequential mode. Sub-agent context limits mean they won't have full paper context and will produce shallow reviews.

8. **Forgetting to save the final report.** After completing all phases, offer to save the combined report to a file. The report is ephemeral in the chat otherwise.

---

## Verification Checklist

- [ ] Phase 0: Paper fully read and understood; field/methodology/venue identified
- [ ] Phase 0: Reviewer configuration presented to user and confirmed
- [ ] Phase 1: All 5 reviews produced with specific, grounded feedback (not generic)
- [ ] Phase 2: Weighted score calculated correctly, editorial decision with clear rationale
- [ ] Phase 3: At least 10 references verified via web search; 7-mode checklist completed
- [ ] Phase 4 (if triggered): Claims compared against actual source content
- [ ] Final report delivered to user in structured format
- [ ] Offer to save report to file for record-keeping
