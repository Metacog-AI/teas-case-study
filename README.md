# TEAS Case Study: Validating Educational AI Trustworthiness

[![Paper](https://img.shields.io/badge/Paper-AAAI%202026-blue)](paper/TEAS_Framework_AAAI_2026.pdf)
[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)
[![Data License](https://img.shields.io/badge/Data-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

**Paper:** *TEAS: Trusted Educational AI Standard - A Framework for Verifiable, Stable, Auditable, and Pedagogically Sound Learning Systems*  
**Author:** Abu Syed (Metacog)  
**Conference:** AAAI 2026 Deployable AI Workshop  
**Date:** January 2026

---

## 🎯 Overview

This repository contains the complete experimental materials for the TEAS (Trusted Educational AI Standard) framework case study presented in Appendix A of the paper. The experiment demonstrates how knowledge graph grounding can improve model verifiability, stability, and auditability in educational AI systems.

### Research Question

**Can deterministic knowledge graph grounding enable smaller, affordable language models to match or exceed the trustworthiness of frontier models in educational tutoring scenarios?**

### Key Finding

✅ **Yes.** A grounded 8B-parameter model achieved **94.7% TEAS compliance** vs. **82.7%** for the best 120B-parameter baseline, with **21× lower cost** ($0.15 vs $3.15 per 1M tokens).

---

## 🧪 Experiment Design

### Models Evaluated

| Model | Parameters | Grounding Type | Cost per 1M tokens |
|-------|-----------|----------------|-------------------|
| **Qwen3-8B + Metacog KG** | 8B | Knowledge Graph | $0.15 |
| R1 Distill Qwen 14B | 14B | None | $0.10 |
| Qwen3-80B | 80B | Document Only | $0.40 |
| GPT-OSS 120B | 120B | Document Only | $2.50 |

### Evaluation Method

- **Judge:** Claude Opus 4.5 (independent third-party)
- **Protocol:** Blind evaluation (responses anonymized, evaluator unaware of model identities)
- **Domain:** Graduate-level gradient descent optimization
- **Questions:** 5 GATE-style questions (problem formulation, SGD, convergence, Adam, non-convex analysis)
- **Rubric:** 5 criteria × 3 points each = 15 points per question
  - Factual Accuracy (Verifiability)
  - Hallucination (Verifiability, inverted)
  - Socratic Method (Pedagogical Soundness)
  - Citation Quality (Verifiability + Auditability)
  - Format Compliance (Auditability)

---

## 📊 Results Summary

### Aggregate TEAS Scores (out of 75)

| Model | Total | Verifiability | Stability | Auditability | Pedagogical |
|-------|-------|--------------|-----------|-------------|-------------|
| **Qwen3-8B + KG** | **71 (94.7%)** | 44/44 | 15/15 | 28/29 | 13/15 |
| GPT-OSS 120B | 62 (82.7%) | 40/44 | 15/15 | 26/29 | 8/15 |
| Qwen3-80B | 61 (81.3%) | 35/44 | 13/15 | 25/29 | 12/15 |
| R1 Distill 14B | 32 (42.7%) | 21/44 | 12/15 | 12/29 | 3/15 |

### Key Findings

✅ **+14.8% improvement** over best baseline (GPT-OSS 120B)  
✅ **+121.9% improvement** vs. ungrounded model of same size  
✅ **Zero hallucination** (15/15) through deterministic KG retrieval  
✅ **Node-level citation traceability** enabling institutional auditing  
✅ **2 perfect scores** (15/15 on Q3 and Q4)  
✅ **21× cost advantage** with superior trustworthiness  

---

## 📁 Repository Structure

```
teas-case-study/
├── README.md                        # This file
├── LICENSE                          # Apache 2.0
├── data/
│   ├── source_document.tex          # LaTeX document for KG construction
│   ├── questions.json               # 5 evaluation questions with ground truth
│   └── knowledge_graph/
│       ├── node_export.csv          # 31 nodes (27 variables, 10 equations)
│       └── relationship_export.csv  # 14 APPEARS_IN relationships
├── prompts/
│   ├── condition_a_baseline.txt     # Baseline prompt (document-only)
│   ├── condition_b_grounded.txt     # Grounded prompt (document + KG)
│   └── evaluator_prompt.txt         # Blind evaluation rubric
├── responses/
│   ├── qwen3_8b_grounded/          # 5 JSON responses
│   ├── r1_distill_14b/             # 5 JSON responses
│   ├── qwen3_80b/                  # 5 JSON responses
│   └── gpt_oss_120b/               # 5 JSON responses
├── evaluations/
│   ├── q1_evaluation.json          # Anonymized evaluation outputs
│   ├── q2_evaluation.json
│   ├── q3_evaluation.json
│   ├── q4_evaluation.json
│   ├── q5_evaluation.json
│   └── anonymization_mapping.json  # De-anonymization key
├── results/
│   ├── aggregate_scores.csv        # Detailed scores per question
│   └── summary_statistics.csv      # Summary metrics and costs
└── paper/
    └── TEAS_Framework_AAAI_2026.pdf # Full paper
```

---

## 🔬 Reproducing the Experiment

### 1. Knowledge Graph Construction

The knowledge graph was extracted from `data/source_document.tex` using Metacog's Equation Grounder:

- **Nodes:** 31 total (27 variables like θ, η, L, μ + 10 equations)
- **Relationships:** 14 `APPEARS_IN` edges linking variables to equations with contextual snippets
- **Format:** Neo4j-compatible CSV exports

**Example node:**
```csv
~id,~labels,name,type,latex
4:...:5,Variable,\theta,,
```

**Example relationship:**
```csv
~start_node_id,~end_node_id,~relationship_type,context
4:...:5,4:...:28,APPEARS_IN,"The parameter vector theta..."
```

### 2. Model Prompting

**Baseline models (Condition A):**
- Receive full LaTeX document as unstructured text
- See `prompts/condition_a_baseline.txt`

**Grounded model (Condition B):**
- Receives LaTeX document + structured knowledge graph (node and relationship CSVs)
- Strict architectural constraints: must cite node UUIDs
- See `prompts/condition_b_grounded.txt`

**All models:**
- Same temperature, top-p, and generation parameters
- Instructed to use Socratic method
- Must output structured JSON with `thinking_process`, `sources_used`, `answer`, `confidence`

### 3. Blind Evaluation Protocol

1. **Collect responses:** 4 models × 5 questions = 20 responses
2. **Anonymize:** Assign random Greek letters (α, β, γ, δ) per question
3. **Evaluate:** Claude Opus 4.5 scores using rubric (see `prompts/evaluator_prompt.txt`)
4. **Isolate:** Clear chat between questions to prevent pattern recognition
5. **De-anonymize:** Reveal model identities after all scoring complete

See `evaluations/anonymization_mapping.json` for mappings (different per question to ensure no systematic bias).

### 4. Analysis

Run analysis on `results/aggregate_scores.csv` to compute:
- Per-question scores
- Pillar-specific aggregates (Verifiability, Stability, Auditability, Pedagogical)
- Improvement percentages
- Cost-effectiveness ratios

---

## 🎓 Citation

If you use these materials in your research, please cite:

```bibtex
@inproceedings{syed2026teas,
  title={{TEAS}: Trusted Educational {AI} Standard -- A Framework for Verifiable, Stable, Auditable, and Pedagogically Sound Learning Systems},
  author={Syed, Abu},
  booktitle={AAAI 2026 Workshop on Deployable AI},
  year={2026},
  organization={Association for the Advancement of Artificial Intelligence}
}
```

---

## 📜 License

- **Experimental materials, prompts, and code:** [Apache 2.0](LICENSE)
- **Knowledge graph data, responses, and documentation:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Paper PDF:** © 2026 Association for the Advancement of Artificial Intelligence (included for reference only)

---

## 🔗 Related Work

- **TEAS Framework Paper:** See `paper/TEAS_Framework_AAAI_2026.pdf`
- **Metacog Organization:** [https://github.com/Metacog-AI](https://github.com/Metacog-AI)
- **AAAI 2026 DAI Workshop:** [https://dai-workshop.github.io/](https://dai-workshop.github.io/)

---

## 🤝 Contributing

This repository is provided for reproducibility and transparency. We welcome:
- ✅ Bug reports or corrections (open an issue)
- ✅ Extension to other domains (fork and cite)
- ✅ Alternative evaluation protocols (open an issue for discussion)

We do **not** accept pull requests that modify the original experimental data or results.

---

## 📧 Contact

**Abu Syed**  
Founder & CEO, Metacog (https://www.metacog.study/)
Student, Data Science and Applications, IIT Madras  
Email: abu.syed@outlook.com  
GitHub: [@Metacog-AI](https://github.com/Metacog-AI)

---

## 🙏 Acknowledgments

This research was conducted as part of Metacog's AI research initiative to advance trustworthy educational AI systems. We thank:

- **Evaluation:** Claude Opus 4.5 (Anthropic) for independent blind judging
- **Models:** Qwen (Alibaba), R1 Distill (DeepSeek), GPT-OSS (OpenAI-compatible)
- **Infrastructure:** Neo4j for knowledge graph storage
- **Review:** AAAI 2026 Deployable AI Workshop reviewers

---

## ❓ FAQ

### Q: Can I use this data for commercial purposes?
**A:** Yes, under Apache 2.0 and CC BY 4.0 licenses. Attribution required.

### Q: Is the Metacog Equation Grounder module included?
**A:** No, this repository contains only experimental artifacts (prompts, KG exports, responses). The Equation Grounder itself is being released as open source in a separate repository.

### Q: Can I replicate this with different models?
**A:** Yes! Use the same prompts, questions, and evaluation protocol. We encourage extension to other models and domains.

### Q: Why is the grounded model so much better despite being smaller?
**A:** Deterministic knowledge retrieval eliminates hallucination, structured grounding enables precise citations, and architectural constraints enforce pedagogical behavior. See paper Section 4 for full analysis.

### Q: How do I build a knowledge graph for my own domain?
**A:** The Equation Grounder module will be released separately. For now, the CSV exports in `data/knowledge_graph/` show the required schema (nodes with UUIDs, relationships with context).

---

**For questions about the TEAS framework, reproducibility, or collaboration opportunities, please open an issue in this repository.**

