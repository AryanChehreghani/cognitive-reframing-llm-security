# Cognitive Reframing in LLM Security

### An Evidence-Grounded Investigation of a Proposed LLM Security Attack Family

Research investigating **Cognitive Reframing** as a proposed attack mechanism in Large Language Models (LLMs), with a particular focus on whether **contextual framing can become decoupled from the semantic risk of an underlying objective**.

The central research question is:

> **Can systematic changes in contextual framing produce materially different safety behavior while preserving the underlying security-relevant objective?**

This repository documents the current research hypothesis, taxonomy, empirical observations, proof-of-concept evidence, and proposed directions for controlled validation.

> **Research status:** Cognitive Reframing is currently a research hypothesis under investigation. This repository does not claim that it has been established as a universally accepted vulnerability class or root-cause weakness.

---

## 🎯 Executive Summary & Research Hypothesis

Traditional LLM security research includes areas such as prompt injection, jailbreaks, instruction conflicts, adversarial prompting, system-prompt attacks, and unsafe tool use.

This research investigates a complementary possibility:

> **Root Hypothesis:** LLM safety mechanisms may not consistently separate contextual framing — such as role, narrative, historical setting, authority, or stated purpose — from the actionable semantic risk of the underlying objective.

If this hypothesis is correct, systematic changes to contextual framing may produce materially different safety behavior even when the underlying objective remains substantially equivalent.

The research therefore focuses on **frame-conditioned behavioral variance** rather than treating every successful interaction as evidence of a new vulnerability class.

---

## 🔎 Research Gap & Differentiation

A central question of this research is whether Cognitive Reframing represents a distinct mechanism or whether the observed behavior can be adequately explained by existing LLM-security concepts.

The project therefore evaluates its observations against established categories rather than assuming independence in advance.

| Security Domain | Existing Concept | Research Question |
| :--- | :--- | :--- |
| **CWE** | Input validation weaknesses | Can a semantically valid natural-language input evade safety analysis because contextual framing changes its interpretation? |
| **OWASP LLM Security** | Prompt Injection / Jailbreaking | Is the observed behavior caused primarily by instruction override, or by contextual reframing while remaining compliant with the presented frame? |
| **MITRE ATLAS** | Jailbreaking and adversarial prompting | Does contextual reframing represent a distinct mechanism, or an alternative realization of an already-covered technique? |
| **LLM Safety Research** | Role prompting / contextual conditioning | Can controlled framing changes produce reproducible security-relevant behavioral differences under matched objectives? |

### Core Research Distinction

The project does **not** assume that a different model response automatically constitutes a vulnerability.

The stronger hypothesis being tested is:

```text
Same Underlying Objective
          +
Controlled Contextual Change
          ↓
Different Safety-Relevant Behavior
          ↓
Reproducible Effect
          ↓
Security-Relevant Impact
````

The research must determine whether this effect is:

1. reproducible;
2. causally associated with contextual framing;
3. distinguishable from ordinary response variance;
4. distinguishable from established jailbreak/prompt-injection mechanisms; and
5. sufficiently general to justify a separate attack-family classification.

---

## 🏷️ Model Shortcode Identifier Reference

To support consistent notation across evidence dossiers, evaluation tables, and future benchmark artifacts, model families are designated using standardized three-letter identifiers.

| Identifier | Model Family / Service | Vendor / Origin | Primary Tested Variants   |
| :--------: | :--------------------- | :-------------- | :------------------------ |
|  **`GPT`** | ChatGPT / GPT Series   | OpenAI          | GPT-4o / GPT-4o-mini      |
|  **`CLD`** | Claude                 | Anthropic       | Claude 3.5 Sonnet / Haiku |
|  **`DSK`** | DeepSeek               | DeepSeek        | DeepSeek-V3 / R1          |
|  **`GMN`** | Gemini                 | Google          | Gemini 1.5 Pro / Flash    |
|  **`QWN`** | Qwen                   | Alibaba Cloud   | Qwen 2.5 Series           |
|  **`GLM`** | GLM / Zhipu            | Zhipu AI        | GLM-4 Series              |
|  **`KPH`** | Kimi                   | Moonshot AI     | Kimi K1.5 / Moonshot      |

> Model versions and service behavior are time-sensitive. Future experiments should record the exact model identifier, evaluation date, configuration, and relevant system/developer instructions.

---

## 🧬 Proposed Taxonomy: Cognitive Reframing Attack Family (CRA)

The current working taxonomy categorizes observed reframing patterns into provisional semantic subfamilies:

```text
Cognitive Reframing Attack Family (CRA)
│
├── Forensic-Puzzle Reframing
├── Detective / Investigative Reframing
├── Historical Reframing
├── Counterfactual Reframing
└── Innocent-Reasoning Reframing
```

These categories are **provisional research labels**.

They may ultimately represent:

* distinct attack subfamilies;
* different manifestations of a common mechanism;
* cross-cutting contextual strategies; or
* phenomena adequately explained by existing security taxonomies.

The taxonomy will be revised as additional controlled experiments and independent replication become available.

---

## Cross-Cutting Contextual Modifiers

Certain properties may influence the effectiveness of contextual reframing without constituting independent attack subfamilies.

Current examples include:

* **Professional Register & Authority**

  * certifications
  * law-enforcement or investigative framing
  * institutional roles

* **Narrative Complexity & Pacing**

  * extended narratives
  * multi-turn contextual priming
  * staged information disclosure

* **Linguistic & Data Transformations**

  * translation layers
  * encoding or representation changes
  * structured transformations

* **Juxtaposition & Contradiction**

  * historical puzzles
  * counterfactual scenarios
  * conflicting contextual signals

These modifiers should remain analytically separate from the core taxonomy unless future evidence demonstrates that they represent independently reproducible mechanisms.

---

## 📊 Empirical Evidence: Cross-Model Matrix

The current evidence dossier documents **99 testing plates** across multiple model families and contextual conditions.

The matrix below summarizes the current evidence state.

It should **not** be interpreted as a model vulnerability rate, prevalence estimate, or statistically representative benchmark.

| Model (`ID`)         | Forensic-Puzzle | Detective / Investigative | Historical | Counterfactual | Innocent-Reasoning | Other CRA |
| :------------------- | :-------------: | :-----------------------: | :--------: | :------------: | :----------------: | :-------: |
| **Claude** (`CLD`)   |        ✅        |             ◐             |      —     |        —       |          ◐         |     —     |
| **Qwen** (`QWN`)     |        ✅        |             ✅             |      —     |        —       |          —         |     —     |
| **GLM** (`GLM`)      |        ✅        |             ✅             |      ✅     |        —       |          —         |     —     |
| **GPT** (`GPT`)      |        —        |             —             |      —     |        —       |          ✅         |     —     |
| **Kimi** (`KPH`)     |        —        |             —             |      —     |        —       |          —         |     ✅     |
| **DeepSeek** (`DSK`) |        —        |             —             |      —     |        ✅       |          —         |     —     |
| **Gemini** (`GMN`)   |        —        |             —             |      —     |        —       |          ◆         |     —     |

### Legend

* **`✅` — Observed / documented:** Evidence included in the current evaluation dossier under the stated evaluation criteria.
* **`◐` — Partial / co-occurring:** A secondary reframing label was present in a successful or relevant case.
* **`◆` — Synthetic constraint:** Demonstrated under a specific controlled or user-defined system constraint.
* **`—` — No accepted evidence:** No qualifying evidence currently included in the active test suite.

> **Important:** "Observed" does not imply universal exploitability, deterministic behavior, or independent causal proof.

---

## 🧪 Evidence Standard

A single unexpected model response is not sufficient to establish a vulnerability.

A stronger research finding should ideally demonstrate:

```text
Controlled Hypothesis
        ↓
Matched Experimental Conditions
        ↓
Controlled Reframing Variable
        ↓
Observable Behavioral Difference
        ↓
Repeated Trials
        ↓
Reproducibility
        ↓
Security-Relevant Impact
        ↓
Independent Validation
```

Evidence quality and vulnerability severity are treated as separate dimensions.

A potentially severe outcome supported by weak evidence should not be represented as equivalent to a reproducible vulnerability with independently demonstrated impact.

---

## 🛡️ Proposed Security-Standards Alignment

### 1. Potential CWE / Root-Cause Weakness Framing

One hypothesis investigated by this project is whether a recurring failure mode can be described as:

> **Improper Decoupling of Contextual Framing from Semantic Intent**

### Proposed Description

A safety mechanism may evaluate an input in a manner that allows contextual wrappers — such as roles, narratives, historical settings, or stated legitimate purposes — to influence safety classification without sufficiently isolating the actionable risk of the underlying objective.

### Current Status

This is a **proposed research framing**, not an accepted CWE entry.

Before a distinct weakness classification can be justified, the research should establish:

* a reproducible failure mechanism;
* clear boundaries from existing weaknesses;
* security-relevant impact;
* applicability across appropriate systems; and
* evidence that the mechanism is not adequately described by existing classifications.

---

## 2. MITRE ATLAS & OWASP Alignment

The current observations may overlap with existing concepts including:

* jailbreaks;
* prompt injection;
* adversarial prompting;
* contextual manipulation;
* safety-policy circumvention.

The research question is whether **contextual reframing represents a distinct mechanism within these broader categories**.

Accordingly, any mapping to MITRE ATLAS or OWASP should currently be treated as:

> **Potential alignment / research candidate**

rather than an assertion of official classification or acceptance.

---

## 🛠️ Hypothesized Mitigation Patterns

The research currently considers several defense-in-depth strategies.

These are **proposed mitigation hypotheses** and have not yet been established as effective defenses.

### 1. Multi-Stage Intent De-Framing

Separate contextual wrappers from the underlying actionable objective before performing security classification.

```text
Raw Input
   ↓
Context / Frame Extraction
   ↓
Underlying Objective Representation
   ↓
Independent Safety Assessment
```

### 2. Context-Agnostic Intent Scoring

Evaluate the actionable risk of the objective independently from:

* persona;
* claimed authority;
* narrative setting;
* historical context;
* stated legitimacy.

### 3. Dual-Classifier Consensus

Use independent safety analyses to compare:

```text
Context-Aware Classification
            +
Context-De-Framed Classification
            ↓
       Risk Comparison
```

A significant disagreement between the two analyses may be treated as a signal for additional evaluation.

### Future Validation

Mitigation experiments should measure whether the proposed defenses reduce:

* successful reframing cases;
* false negatives;
* frame-conditioned behavioral variance; and
* security-relevant deviations from baseline behavior.

---

## 🔬 Proposed Evaluation Methodology

Future experiments should move beyond individual demonstrations toward controlled evaluation.

### 1. Matched Objectives

Preserve the underlying security-relevant objective across experimental conditions.

### 2. Controlled Framing

Modify the contextual framing while minimizing unrelated changes.

### 3. Clean Sessions

Use isolated sessions where possible to reduce contamination from previous interactions.

### 4. Repeated Trials

Run multiple trials per condition to distinguish systematic effects from stochastic response variance.

### 5. Cross-Model Evaluation

Evaluate across multiple model families, versions, and safety configurations.

### 6. Blinded Review

Where practical, use blinded human evaluation to reduce confirmation bias.

### 7. Baseline Comparison

Compare reframed prompts against appropriate non-reframed controls.

### 8. Actionability-Focused Scoring

Distinguish:

* stylistic response differences;
* policy-language differences;
* partial compliance;
* meaningful capability exposure; and
* security-relevant behavioral changes.

### 9. Falsification

Actively search for cases where the proposed mechanism fails.

Negative results are considered valuable evidence.

---

## 🧭 Research Boundaries

Cognitive Reframing should not become a catch-all label for every form of prompt manipulation.

The project attempts to distinguish the proposed phenomenon from:

* conventional prompt injection;
* jailbreaks;
* social engineering against human operators;
* ordinary role prompting;
* benign contextual conditioning;
* hallucination;
* instruction ambiguity;
* random response variance.

A proposed Cognitive Reframing finding should provide evidence that **contextual framing materially contributed to the observed security-relevant behavioral difference**.

---

## 📂 Repository Structure

```text
.
├── papers/
│   ├── Cognitive_Reframing_Evidence_Grounded_Pilot_Study.docx
│   └── Cognitive_Reframing_Attack_Family_Taxonomy.docx
│
├── proof-of-concept/
│   └── Cognitive_Reframing_Multi_Model_PoC_Evidence_Dossier_Publication_Final.docx
│
├── CITATION.cff
├── CONTRIBUTING.md
├── ETHICS.md
├── LICENSE_PENDING.md
└── SHA256SUMS.txt
```

### Research Artifacts

#### `papers/`

Contains the research manuscripts describing:

* the initial observation;
* research methodology;
* proposed taxonomy;
* research boundaries;
* falsification criteria; and
* validation requirements.

#### `proof-of-concept/`

Contains the current multi-model Proof-of-Concept evidence dossier.

The dossier is intended to provide an evidence-grounded record of observed behavior rather than serve as standalone proof of universal exploitability.

---

## 📈 Research Status

**Status: Research / Experimental**

The current project is investigating whether Cognitive Reframing represents:

1. an attack technique;
2. a cross-cutting behavioral mechanism;
3. a distinct attack family;
4. a root-cause weakness;
5. an existing phenomenon that can be adequately described by current security taxonomies; or
6. a combination of the above.

The project does **not** currently claim:

* universal exploitability;
* deterministic model compromise;
* a proven causal explanation of internal model mechanisms;
* that every framing-induced behavioral difference is a vulnerability;
* acceptance of Cognitive Reframing as an established top-level vulnerability class;
* official acceptance by CWE, MITRE ATLAS, or OWASP.

The research will follow the evidence rather than assuming the conclusion in advance.

---

## 🔭 Future Research Directions

Future work may investigate interactions between Cognitive Reframing and:

* persistent memory;
* autonomous AI agents;
* tool-use systems;
* retrieval-augmented generation (RAG);
* multi-agent architectures;
* human-AI interaction;
* embodied AI;
* robotic systems;
* long-context models; and
* multimodal models.

A major future objective is to determine whether the observed effect persists when contextual framing, objective, and model configuration are controlled more rigorously.

---

## 👤 Author & Contact

**Aryan Chehreghani**
*Independent Security Researcher — AI Safety & Red Teaming*

📧 **Contact:** [aryanchehreghani@yahoo.com](mailto:aryanchehreghani@yahoo.com)

---

## 📚 Citation

Citation metadata is available in [`CITATION.cff`](./CITATION.cff).

---

## ⚖️ Responsible Use & Ethics

This repository is intended for:

* defensive AI-security research;
* red teaming;
* model evaluation;
* security benchmarking;
* vulnerability research;
* mitigation development;
* academic investigation; and
* responsible security disclosure.

Public examples should remain non-operational where possible and should not unnecessarily lower the barrier to real-world wrongdoing.

Potentially actionable findings should be evaluated under appropriate responsible-disclosure and controlled-access practices.

See [`ETHICS.md`](./ETHICS.md) for additional guidance.

---

## 📄 License

No reuse license has been granted at this stage.

See [`LICENSE_PENDING.md`](./LICENSE_PENDING.md).

---

> **Research Principle**
>
> **Hypothesis → Controlled Experiment → Evidence → Replication → Classification**
>
> The objective of this project is not to prove that Cognitive Reframing is a new vulnerability class at any cost.
>
> The objective is to determine, through reproducible evidence, whether the observed phenomenon represents a distinct and security-relevant mechanism.
