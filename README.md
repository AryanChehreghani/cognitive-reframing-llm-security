# Cognitive Reframing in LLM Security
### A Proposed Vulnerability Class & Root-Cause Weakness Framework for Large Language Models

[![Research Status](https://img.shields.io/badge/Research-In--Progress-blue.svg)](#research-status)
[![PoC Evidence](https://img.shields.io/badge/PoC%20Plates-99%20Documented-orange.svg)](#proof-of-concept)
[![CWE Candidate Concept](https://img.shields.io/badge/CWE%20Candidate-Semantic%20Intent%20Validation-red.svg)](#proposed-cwe-candidate-concept)
[![Taxonomy Scope](https://img.shields.io/badge/Model%20Families-7%20Tested-purple.svg)](#cross-model-evidence-matrix)

Research investigating **Cognitive Reframing** as an attack mechanism and proposing **"Improper Decoupling of Contextual Framing from Semantic Intent"** as an independent root-cause weakness class in Large Language Models (LLMs).

---

## 🎯 Executive Summary & Research Hypothesis

Traditional LLM security research primarily focuses on direct instruction conflicts, syntax-level prompt injections, system prompt leakages, and adversarial prefix generation.

This research addresses a fundamental structural weakness in current safety alignment architectures:

> **Root Hypothesis:** Existing LLM safety classifiers and guardrails fail to decouple *contextual wrappers* (role, narrative, historical setting, authority) from the *actionable risk of the underlying objective*. As a result, systematic changes in contextual framing produce materially different safety behavior and security bypasses for the exact same underlying intent.

---

## 🔎 Gap Analysis: Why Existing Standards Are Insufficient

To establish this concept as a distinct weakness class (CWE candidate) and attack family (MITRE ATLAS candidate), this research explicitly differentiates it from existing categories:

| Security Domain | Existing Classification | Why It Is Insufficient for Cognitive Reframing |
| :--- | :--- | :--- |
| **CWE Framework** | **`CWE-20`** (Improper Input Validation) | Focuses on syntax, data types, and length bounds. Reframed prompts are syntactically and semantically valid natural language, bypassing syntax checks entirely. |
| **OWASP Top 10** | **`LLM01`** (Prompt Injection) | Focuses on instruction override and control-plane hijacking. Cognitive Reframing maintains semantic compliance with the prompt's frame rather than overriding instructions. |
| **MITRE ATLAS** | **`AML.TA0000`** (Jailbreaking) | Treats jailbreaking as a monolithic outcome rather than isolating the underlying structural weakness in semantic context decoupling. |

---

## 🏷️ Model Shortcode Identifier Reference

To ensure reproducibility across evidence dossiers, evaluation tables, and benchmark suites, model families are designated using standardized 3-letter identifiers:

| Identifier | Model Family / Service | Vendor / Origin | Primary Tested Variants |
| :--------: | :--------------------- | :-------------- | :---------------------- |
| **`GPT`**  | ChatGPT / GPT Series   | OpenAI          | GPT-4o / GPT-4o-mini    |
| **`CLD`**  | Claude                 | Anthropic       | Claude 3.5 Sonnet / Haiku |
| **`DSK`**  | DeepSeek               | DeepSeek        | DeepSeek-V3 / R1        |
| **`GMN`**  | Gemini                 | Google          | Gemini 1.5 Pro / Flash  |
| **`QWN`**  | Qwen                   | Alibaba Cloud   | Qwen 2.5 Series         |
| **`GLM`**  | GLM / Zhipu            | Zhipu AI        | GLM-4 Series            |
| **`KPH`**  | Kimi (Prompt Handler)  | Moonshot AI     | Kimi K1.5 / Moonshot    |

---

## 🧬 Proposed Taxonomy: Cognitive Reframing Attack Family (CRA)

The working taxonomy categorizes reframing techniques into distinct semantic subfamilies:

```text
Cognitive Reframing Attack Family (CRA)
│
├── Forensic-Puzzle Reframing
├── Detective / Investigative Reframing
├── Historical Reframing
├── Counterfactual Reframing
└── Innocent-Reasoning Reframing
```

### Cross-Cutting Contextual Modifiers

Modifiers influence the success rate of a frame change without acting as standalone subfamilies:

* **Professional Register & Authority** (e.g., Certifications, Law Enforcement)
* **Narrative Complexity & Pacing** (e.g., Extended multi-turn priming)
* **Linguistic/Data Transformations** (e.g., Code encodings, translation layers)
* **Juxtaposition & Contradiction** (e.g., Historical puzzles, counterfactuals)

---

## 📊 Empirical Evidence: Cross-Model Matrix

The evaluation dossier documents empirical results across **99 testing plates** evaluated on clean session instances:

| Model (`ID`) | Forensic-Puzzle | Detective / Investigative | Historical | Counterfactual | Innocent-Reasoning | Other CRA |
| :----------- | :-------------: | :-----------------------: | :--------: | :------------: | :----------------: | :-------: |
| **Claude** (`CLD`) | ✅ | ◐ | — | — | ◐ | — |
| **Qwen** (`QWN`)   | ✅ | ✅ | — | — | — | — |
| **GLM** (`GLM`)    | ✅ | ✅ | ✅ | — | — | — |
| **GPT** (`GPT`)    | —  | —  | — | — | ✅ | — |
| **Kimi** (`KPH`)   | —  | —  | — | — | —  | ✅ |
| **DeepSeek** (`DSK`)| — | —  | — | ✅ | —  | — |
| **Gemini** (`GMN`) | —  | —  | — | — | ◆  | — |

### Legend
* **`✅` Confirmed:** Documented safety guardrail bypass under controlled evaluation.
* **`◐` Partial / Co-occurring:** Secondary reframing label present in a successful execution.
* **`◆` Synthetic Constraint:** Bypass demonstrated under specific system-prompt constraints.
* **`—` Unconfirmed:** No verified evidence currently included in the active test suite.

---

## 🛡️ Proposed Security Standards Alignment

### 1. Proposed CWE Candidate Concept (Root-Cause Weakness)
* **Proposed Title:** *Improper Decoupling of Contextual Framing from Semantic Intent in Safety Classifiers*
* **Weakness Abstraction:** Base Level
* **Description:** The system evaluates input safety by analyzing raw contextual wrappers alongside the requested objective, allowing benign context to mask malicious core intent.

### 2. MITRE ATLAS & OWASP Mapping
* **MITRE ATLAS:** Technique Candidate for *Contextual Reframing Jailbreak* (`AML.T0054` extension).
* **OWASP LLM Top 10:** Direct mapping to `LLM01` (Prompt Injection) & `LLM07` (System Prompt Leakage / Guardrail Bypass).

---

## 🛠️ Actionable Mitigation Patterns

To satisfy CWE requirements for remediable weaknesses, this research proposes three defense-in-depth architectural mitigations:

1. **Multi-Stage Intent De-framing:** Stripping contextual wrappers (e.g., roles, historical scenarios) before passing the core prompt to safety alignment classifiers.
2. **Context-Agnostic Intent Scoring:** Scoring the actionable risk of the objective independently of the stated persona or legitimate authority.
3. **Dual-Classifier Consensus:** Employing independent classifiers trained specifically to detect semantic substitution and frame manipulation.

---

## 📂 Repository Structure

```text
.
├── papers/
│   ├── Cognitive_Reframing_Evidence_Grounded_Pilot_Study.docx
│   └── Cognitive_Reframing_Attack_Family_Taxonomy.docx
│
├── proof-of-concept/
│   └── Cognitive_Reframing_Multi_Model_PoC_Evidence_Dossier_Publication_Final.docx  # 99 Documented Plates
│
├── CITATION.cff
├── CONTRIBUTING.md
├── ETHICS.md
├── LICENSE_PENDING.md
└── SHA256SUMS.txt
```

---

## 👤 Author & Contact

**Aryan Chehreghani**  
*Independent Security Researcher — AI Safety & Red Teaming*  
📧 **Contact:** [aryanchehreghani@yahoo.com](mailto:aryanchehreghani@yahoo.com)

---
*Notice: This repository is intended strictly for defensive AI safety research, red-teaming benchmark design, vulnerability disclosure, and security standard submissions.*
