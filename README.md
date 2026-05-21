# Academic Paper Review & Integrity Check

> A structured post-draft quality assurance pipeline for academic manuscripts.  
> 面向已完成论文初稿的结构化同行评审、完整性检查与声明审计流水线。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**English** · [简体中文](README.zh-CN.md)

---

## Overview | 概述

This project provides a complete **post-draft quality assurance pipeline** for academic papers. It adapts the methodology from the [Academic Research Skills](https://github.com/Imbad0202/academic-research-skills) project (v3.8.2) into a structured workflow that covers:

- ✅ **Multi-Perspective Peer Review** — 5 reviewer personas (Editor-in-Chief, Methodology, Domain, Perspective, Devil's Advocate)
- ✅ **Editorial Synthesis** — Weighted scoring, decision letter, and revision roadmap
- ✅ **Reference Integrity Verification** — Web-based sampling verification + 7-mode checklist
- ✅ **Claim Faithfulness Audit** — (Optional) Cross-check central claims against actual sources

**Core philosophy:** AI is the reviewer's assistant, not the author. The goal is to catch errors a human reviewer would spot — fabricated references, logical gaps, and methodological flaws — not to rewrite the paper.

该项目提供论文初稿完成后的**质量保障流水线**，涵盖多角色同行评审、参考文献完整性验证、声明可信度审计等环节。

---

## Features | 功能特性

### 🔬 Phase 0 — Setup & Configuration
- Auto-detect paper discipline, methodology, and target venue tier
- Configure 5 reviewer personas with domain-specific expertise

### 📝 Phase 1 — Multi-Perspective Review
- **Sequential mode** (recommended): 5 in-depth reviews with full paper context
- **Parallel mode** (short papers < 3000 words): Concurrent streamed reviews via sub-agents
- Each report includes: score, verdict, strengths/weaknesses, line-by-line comments, detailed scoring matrix

### 📊 Phase 2 — Editorial Synthesis
- Weighted score aggregation (EIC 25%, Methodology 25%, Domain 20%, Perspective 15%, Devil's Advocate 15%)
- Consensus/disagreement analysis
- Prioritized revision roadmap

### 🔍 Phase 3 — Integrity Gate
- Reference sampling strategy (30% sample, min 10)
- Web-based reference existence verification
- 7-mode integrity checklist:
  - REF-1: Reference Existence
  - REF-2: Citation Relevance
  - REF-3: Data Integrity
  - REF-4: Logical Consistency
  - REF-5: Methodology Alignment
  - REF-6: Figure/Table Accuracy
  - REF-7: Disclosure Completeness

### 🧪 Phase 4 — Claim Audit (Optional)
- Identify 3–5 central claims with cited sources
- Fetch actual source content via web extraction
- Judge faithfulness: SUPPORTED / PARTIAL / NOT-SUPPORTED / FABRICATED

---

## Usage | 使用方法

### As a Hermes Agent Skill

This project is designed as a [Hermes Agent](https://hermes-agent.nousresearch.com) skill. Copy `SKILL.md` to your Hermes skills directory:

```bash
cp SKILL.md ~/.hermes/skills/research/academic-paper-review/
```

Then trigger it with phrases like:
- "review my paper" / "审一下我的论文"
- "check my references" / "帮我检查引用"
- "run integrity check on this paper"

### As a Standalone Reference

The `SKILL.md` file is fully self-contained and can be used as a protocol document for any LLM-based coding agent (Claude Code, Codex CLI, etc.). Simply feed the file as system context when asking your agent to review a paper.

---

## Workflow Diagram | 工作流程

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

## Review Report Example | 审阅报告示例

### Per-Reviewer Report Structure

Each review produces a structured report with:

| Section | Description |
|---------|-------------|
| Score & Verdict | 0–100 score, Accept/Minor/Major/Reject |
| Summary | 2–3 sentence overall impression |
| Strengths | Specific strengths tied to paper content |
| Weaknesses | Specific weaknesses with evidence |
| Specific Comments | Location-based, severity-graded feedback |
| Detailed Scoring | 5 criteria × 0–100 with comments |

### Editorial Decision Package

- **Decision Letter** — Formatted as formal journal editor's letter
- **Cross-Review Analysis** — Points of consensus and disagreement
- **Critical Issues** — Issues flagged as Critical by any reviewer
- **Revision Roadmap** — Prioritized (P1/P2/P3) action items

---

## Integrity Checklist | 完整性检查清单

| # | Check | What It Validates |
|---|-------|-------------------|
| REF-1 | Reference Existence | Do sampled references actually exist? |
| REF-2 | Citation Relevance | Does the paper's claim match the source? |
| REF-3 | Data Integrity | Are numerical results internally consistent? |
| REF-4 | Logical Consistency | Does the argument flow logically? |
| REF-5 | Methodology Alignment | Does the method answer the research question? |
| REF-6 | Figure/Table Accuracy | Do visuals match text descriptions? |
| REF-7 | Disclosure Completeness | Are limitations and conflicts acknowledged? |

---

## License

MIT License — see [LICENSE](LICENSE).

This project is adapted from [Imbad0202/academic-research-skills](https://github.com/Imbad0202/academic-research-skills) (v3.8.2) under MIT License. Significant modifications include:

- Restructured as a Hermes Agent skill with YAML frontmatter
- Added bilingual (Chinese/English) trigger phrases and configuration
- Replaced Claude Code-specific tool references with Hermes Agent equivalents
- Added Devil's Advocate reviewer persona
- Integrated weighted editorial synthesis with specific thresholds
- Added detailed 7-mode integrity checklist with evidence requirements
---

## Author

[ZZZZRD](https://github.com/ZZZZRD) — Adapted and maintained for open-source use.
