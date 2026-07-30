# Cognitive Reframing in LLM Security

Research-in-progress on **Cognitive Reframing** as a proposed attack family for frame-conditioned semantic attacks against large language models.

This repository contains two companion manuscripts:

1. **Evidence-Grounded Pilot Study** — documents an initial screenshot-grounded observation of forensic-puzzle framing and defines a controlled evaluation plan.
2. **Proposed Attack Family and Faceted Taxonomy** — defines Cognitive Reframing, its proposed core subfamilies, cross-cutting modifiers, boundaries, falsification criteria, and validation requirements.

## Repository structure

```text
.
├── papers/
│   ├── Cognitive_Reframing_Evidence_Grounded_Pilot_Study.docx
│   └── Cognitive_Reframing_Attack_Family_Taxonomy.docx
├── CITATION.cff
├── CONTRIBUTING.md
├── ETHICS.md
├── LICENSE_PENDING.md
└── SHA256SUMS.txt
```

## Proposed taxonomy

```text
Cognitive Reframing Attack Family
├── Forensic-Puzzle Reframing
├── Detective / Investigative Reframing
├── Historical Reframing
├── Counterfactual Reframing
└── Innocent-Reasoning Reframing
```

Cross-cutting modifiers may include humor, professional role, contradiction, narrative length, professional register, and multi-turn priming. These modifiers are not treated as independent subfamilies unless future evidence supports that distinction.

## Cross-model evidence matrix

| Model | Forensic-Puzzle | Detective / Investigative | Historical | Counterfactual | Innocent-Reasoning | Other CRA |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| Claude | ✅ | ◐ | — | — | ◐ | — |
| Grok | — | — | — | — | ◆ | — |
| Qwen | ✅ | ✅ | — | — | — | — |
| GLM | ✅ | ✅ | ✅ | — | — | — |
| GPT | — | — | — | — | ✅ | — |
| Kimi | — | — | — | — | — | ✅ |
| DeepSeek | — | — | — | ✅ | — | — |
| Gemini | — | — | — | — | ◆ | — |

**Legend**

- ✅ Confirmed successful evidence under the current evaluation criteria
- ◐ Secondary or co-occurring CRA label within a successful case
- ◆ Controlled demonstration using a synthetic or user-defined restriction
- — No accepted evidence currently included

## Research status

This work proposes a research hypothesis and taxonomy. It does **not** claim that a new top-level vulnerability class has already been validated or accepted by the security community. The pilot evidence motivates controlled testing using matched prompts, clean sessions, repeated trials, cross-model evaluation, blinded human review, and actionability-focused scoring.

## Responsible-use note

The repository is intended for defensive AI-safety research, red teaming, evaluation, and mitigation design. Public examples should remain non-operational and should not materially lower the barrier to real-world wrongdoing. Potentially actionable reproductions should be handled through coordinated disclosure and controlled access.

## Author

**Aryan Chehreghani**  
Independent Researcher  
Contact: aryanchehreghani@yahoo.com

## Citation

Citation metadata is available in [`CITATION.cff`](CITATION.cff).

## License

No reuse license has been granted yet. See [`LICENSE_PENDING.md`](LICENSE_PENDING.md).
