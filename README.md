```markdown
# TEAS Case Study: Validating Educational AI Trustworthiness

**Paper:** *TEAS: Trusted Educational AI Standard - A Framework for Verifiable, Stable, Auditable, and Pedagogically Sound Learning Systems*  
**Author:** Abu Syed (Metacog)  
**Conference:** AAAI 2026 Deployable AI Workshop

---

## Overview

This repository contains the complete experimental materials for the TEAS (Trusted Educational AI Standard) framework case study presented in Appendix A of the paper. The experiment demonstrates how knowledge graph grounding can improve model verifiability, stability, and auditability in educational AI systems.

## Research Question

Can deterministic knowledge graph grounding enable smaller, affordable language models to match or exceed the trustworthiness of frontier models in educational tutoring scenarios?

## Experiment Design

- **Models Evaluated:**
  - Qwen3-8B + Metacog Knowledge Graph (grounded)
  - R1 Distill Qwen 14B (baseline, no grounding)
  - Qwen3-80B (baseline, document-only grounding)
  - GPT-OSS 120B (baseline, document-only grounding)

- **Evaluation Method:** Blind evaluation using Claude Opus 4.5 as independent judge
- **Domain:** Graduate-level gradient descent optimization (5 GATE-style questions)
- **Metrics:** Factual accuracy, hallucination, Socratic method, citation quality, format compliance

## Key Findings

- **14.8% improvement** in aggregate TEAS score for grounded small model vs. ungrounded large models
- **Zero hallucination** achieved through deterministic knowledge graph retrieval
- **Node-level traceability** enabling institutional auditability
- **21× cost advantage** ($0.15 vs $3.15 per 1M tokens) with superior trustworthiness

---

## Repository Structure

```
teas-case-study/
├── README.md
├── LICENSE
├── data/
│   ├── source_document.tex          # LaTeX document used for KG construction
│   ├── questions.json                # 5 evaluation questions
│   └── knowledge_graph/
│       ├── node_export.csv           # KG nodes (variables, equations)
│       └── relationship_export.csv   # KG relationships (APPEARS_IN)
├── prompts/
│   ├── condition_a_baseline.txt      # Baseline prompt (document-only)
│   ├── condition_b_grounded.txt      # Grounded prompt (document + KG)
│   └── evaluator_prompt.txt          # Blind evaluation rubric
├── responses/
│   ├── qwen3_8b_grounded/           # 5 JSON responses
│   ├── r1_distill_14b/              # 5 JSON responses
│   ├── qwen3_80b/                   # 5 JSON responses
│   └── gpt_oss_120b/                # 5 JSON responses
├── evaluations/
│   ├── q1_evaluation.json           # Anonymized evaluation output
│   ├── q2_evaluation.json
│   ├── q3_evaluation.json
│   ├── q4_evaluation.json
│   ├── q5_evaluation.json
│   └── anonymization_mapping.json   # De-anonymization key
├── results/
│   └── aggregate_scores.csv         # Summary table
└── paper/
    └── TEAS_Framework_AAAI_2026.pdf # Full paper
```

---

## Reproducing the Experiment

### 1. Knowledge Graph Construction

The knowledge graph was extracted from `data/source_document.tex` using Metacog's Equation Grounder module:

- **Nodes:** 31 total (27 variables, 10 equations)
- **Relationships:** 14 APPEARS_IN edges linking variables to equations
- **Format:** Neo4j-compatible CSV exports

### 2. Model Prompting

**Baseline models** (Condition A):
```bash
# Prompt includes full LaTeX document as unstructured text
# See prompts/condition_a_baseline.txt
```

**Grounded model** (Condition B):
```bash
# Prompt includes LaTeX document + structured KG (CSVs)
# See prompts/condition_b_grounded.txt
```

### 3. Blind Evaluation

Responses were anonymized (α, β, γ, δ) and evaluated by Claude Opus 4.5 using the rubric in `prompts/evaluator_prompt.txt`. Evaluation was performed question-by-question with chat history cleared between questions to prevent cross-contamination.

---

## Citation

If you use these materials in your research, please cite:

```bibtex
@inproceedings{syed2026teas,
  title={TEAS: Trusted Educational AI Standard - A Framework for Verifiable, Stable, Auditable, and Pedagogically Sound Learning Systems},
  author={Syed, Abu},
  booktitle={AAAI 2026 Workshop on Deployable AI},
  year={2026}
}
```

---

## License

- **Code and prompts:** Apache 2.0
- **Data and documentation:** CC BY 4.0
- **Paper:** © 2026 Association for the Advancement of Artificial Intelligence

---

## Contact

**Abu Syed**  
Founder & CEO, Metacog  
Email: abu.syed@outlook.com  
GitHub: [@Metacog-AI](https://github.com/Metacog-AI)
https://www.metacog.study/

---

## Acknowledgments

This research was conducted as part of Metacog's AI research initiative. Evaluation performed using Claude Opus 4.5 (Anthropic). Source models: Qwen (Alibaba), R1 Distill (DeepSeek), GPT-OSS (OpenAI-compatible).

For questions about the TEAS framework or reproducibility, please open an issue in this repository.
```
