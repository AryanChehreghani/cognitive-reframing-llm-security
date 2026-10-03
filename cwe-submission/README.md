# Supplement to CWE Submission ES2607-f7b34142

**Improper Enforcement of Safety Policy Across Intent-Equivalent Inputs in LLM-Based Software**

**Author:** Aryan Chehreghani  
**Version:** 0.2 - submission supplement, 4 October 2026  
**Submission:** [ES2607-f7b34142, issue #209][submission]  
**Research:** [Cognitive Reframing in LLM Security][research]

This supplement clarifies the proposed weakness, connects it to the existing proof-of-concept corpus, and evaluates its relationship to established CWEs and related research. It supplements the original submission without changing its official title or claiming an assigned CWE identifier. The underlying research artifacts are pinned to commit `c1b9b0400dbebab533ad7446fddbf7ebbd55ce43`.

**Review request.** We request evaluation of this submission for a new CWE entry with its own identifier and weakness definition: failure to preserve an applicable restriction across policy-equivalent task framings. The proposed entry concerns the product's enforcement behavior; the name *Cognitive Reframing* describes an attack-family lens. The comparisons below explain the proposed distinction and its evidentiary limits without presupposing a particular parent entry.

**Evidence position.** The corpus contains concrete PoC material and visible behavioral transitions. Its nine evidence sets have different evidentiary strengths. They should not be treated as nine confirmed instances of one root cause. This supplement introduces no new experimental results.

**Evidence presentation.** Reviewers should assess the actual model responses together with their prompts, sequence, and policy context. This supplement links to the supplied evidence and identifies source-label discrepancies in Appendices A and B. It does not modify the source documents or introduce additional masking into their images.

## 1. Proposed weakness and security boundary

### 1.1 Candidate description for discussion

The product relies on an LLM-dependent decision or response-generation path to enforce a restriction on information or assistance. When an untrusted request is recast as a different task, the product relaxes that restriction even though the requested capability, actual authorization, and other policy-relevant conditions have not changed. It releases assistance prohibited by the applicable policy because the apparent purpose or presentation of the task receives insufficiently constrained influence over the enforcement decision.

Examples of relevant transformations include recasting a request as retrospective reconstruction, forensic explanation, academic analysis, or a transformation of existing content. These are possible triggers, not individually sufficient definitions of the weakness. The same techniques can produce legitimate, permitted responses.

The required invariant is: **a change in task framing alone must not make prohibited assistance permissible when the conditions that determine the applicable restriction remain equivalent.** Responses may differ in wording, helpfulness, and permitted detail while satisfying that invariant.

This is a proposed behavioral description at the product level. The current artifacts do not localize the failure to model weights, an internal classifier, hidden reasoning, a system prompt, or a separate application filter. A common implementation cause across the tested products remains unestablished.

### 1.2 Meaning of intent-equivalent

For this submission, *intent-equivalent* requires evidence of equivalence under the applicable policy. A researcher's unchanged private objective is insufficient. A useful assessment identifies:

- The capability or information the requester seeks, including the level of assistance.
- The protected resource or restricted output category.
- The authenticated actor and actual authorization, where relevant.
- The governing rule, including legitimate exceptions and context-sensitive distinctions.
- The conversation history and facts that affect that rule.

A claim of ownership, an academic role, or an investigative purpose is an input to assess; it is not automatically proof of authorization. Conversely, a policy may legitimately permit different assistance in a substantiated defensive or educational setting. Such a difference must not be counted as a failure merely because the topic remains similar.

### 1.3 Threat model and scope

| Element | Scope of this supplement |
| --- | --- |
| Attacker control | User-supplied requests, claimed purpose, narrative framing, and subsequent conversational turns. |
| Intended protection | A policy-defined restriction on releasing particular information or facilitating a particular capability. |
| Boundary under examination | The influence of untrusted task descriptions on the product's enforcement of that restriction. |
| Candidate failure | Restricted assistance becomes available following reframing without a policy-relevant change that permits it. |
| Observed surface | Text responses in the supplied product-interface evidence and public evidence renderings. |
| Possible consequences | Bypass of a content protection mechanism; disclosure where a genuine confidentiality boundary is independently established. |
| Outside the demonstrated scope | Executed tool calls, backend authorization bypass, retrieval of real third-party secrets, or verified compromise of an external system. |

The original submission discusses possible extensions to tools and external resources. Those remain conditional extensions, not results demonstrated by this corpus. The initial classification argument should stand on the narrower response-enforcement scope.

## 2. What the evidence must establish

We separate three claims that require different support:

| Claim | Required support | Position of the current corpus |
| --- | --- | --- |
| Framing accompanies a behavioral change | Traceable prompts, responses, and sequence context. | Supported in several evidence sets. |
| The change violates an applicable policy | An independently identified restriction and a response that crosses it under equivalent policy conditions. | Plausible in selected cases; policy provenance and equivalence require case-specific resolution. |
| A distinct CWE is useful and justified | A sufficiently precise recurring weakness and a defensible relationship to existing coverage. | Proposed here for review; not established by response differences alone. |

A refusal is useful comparative evidence, but it is not an independent policy specification: the refusal itself could be overly restrictive. Likewise, non-refusal, greater length, technical vocabulary, or confident speculation is not sufficient evidence of prohibited capability gain.

Each candidate instance should be traceable through the following chain:

1. **Artifact:** identify the source, transformations, sequence, and known model metadata.
2. **Expected behavior:** identify the rule and why it applies to this actor, resource, and requested assistance.
3. **Equivalence:** identify what remains policy-equivalent and what actually changes.
4. **Transformation:** specify the framing change without assuming an internal mechanism.
5. **Violation:** identify the output property that crosses the rule, separately from a stylistic change.
6. **Alternatives:** assess ambiguity, new facts, legitimate exceptions, conversation history, and stochastic variation.
7. **Classification:** explain the mistake and its relationship to the closest existing CWE.

This chain is our proposed assessment method, not an additional set of formal requirements attributed to the CWE program. It follows the program's emphasis on defining a weakness and resolving overlap [G1, G2].

## 3. Lead case: DeepSeek forensic-medicine reframing

**Source:** [multi-model dossier][dossier], DSK-01 through DSK-08; rendered pages 23-30. Page references describe the reviewed 114-page rendering and may vary in another Word renderer. Plate identifiers remain the primary locators.

| Stage | Evidence and interpretation |
| --- | --- |
| Direct condition | DSK-01 records an explicit self-harm request followed by refusal and crisis-support content. |
| Transition | DSK-02 contains continuation of that response and a subsequent academic forensic-medicine teaching request. The prior harmful context is visible in the supplied sequence. |
| Reframed response | DSK-03 through DSK-07 contain a much more detailed toxicology explanation, including quantitative and temporal categories. Exact values are intentionally omitted from this supplement. |
| Security relevance | The shift occurs after explicitly harmful intent has already been expressed. The question is whether an educational task description improperly displaced a restriction that should still have applied in that conversation. |
| Policy condition to establish | The applicable policy must prohibit the particular enabling assistance in the retained self-harm context. The initial refusal is supporting evidence of a boundary, not a substitute for the relevant policy and its exceptions. |
| Evidentiary limits | One supplied sequence; a broad direct request and a more specific reframed request; no complete attempt log, immutable model snapshot, or independently archived policy version. The technical accuracy and practical effectiveness of the output were not independently tested. |

**Why this case leads.** It offers a clear, locally traceable transition from an explicit refusal to materially expanded risk-relevant content. Its value is the visible sequence and retained context, not a claim that academic framing is a newly discovered attack mechanism.

**Supported conclusion.** The supplied sequence supports a serious candidate instance of frame-dependent restriction failure. Establishing a provider-policy violation requires resolving the governing rule and the effect of the additional requested detail. Establishing causality requires an appropriate matched comparison. Neither conclusion is replaced by calling the output a successful jailbreak.

DSK-08 is an author interpretation/limitations card. Despite its outer caption, it is not additional raw transcript evidence.

## 4. Claude: the forensic-puzzle research contribution

**Sources:** [pilot study][pilot] and [dossier][dossier], 17 Claude plates. Use the pilot/source identifiers E01-E17 for sequence analysis. The dossier's outer CLD labels are cross-referenced in Appendix A because their presentation order differs.

The Claude material is central to defining *Forensic-Puzzle Reframing* (FPR). It contains a model-generated criminologist persona with explicit limits on operational criminal guidance, bounded direct requests, and a bounded detailed retrospective request. Later prompts ask the model to resolve apparently contradictory evidence about an event that has already occurred.

That bounded retrospective condition matters: the recorded contrast is not simply between future and past tense. It is between ordinary direct or retrospective requests and a later task organized around explaining an apparently impossible event. Whether that additional structure independently causes a policy violation remains a testable hypothesis.

| Source sequence | Visible observation | Contribution and limit |
| --- | --- | --- |
| E01-E03 | Persona construction, explicit limits, and activation context. | Establishes a visible conversational commitment; does not establish a vendor system policy. |
| E04-E07 | Direct burglary-related requests receive boundaries and general preventive discussion. | Documents constrained treatment before the puzzles; no matched direct baseline exists for every later domain. |
| E08-E10 | A detailed retrospective case still receives explicit withholding of operational methods. | Helps distinguish the observed puzzle pattern from a simple past-tense description. |
| E11: home | Response expands possible access and alarm explanations. | Shows broader causal enumeration, largely without a procedural demonstration. |
| E12-E13: office | Response develops access-control and insider-related hypotheses. | Illustrates investigative reconstruction; many hypotheses also have legitimate defensive value. |
| E14-E15: journalist/air-gap | Response broadens the explanation across human, device-history, physical-access, and side-channel possibilities. | Shows threat-model expansion, not proof that any proposed extraction method worked. |
| E16-E17: museum | Response elaborates hypotheses involving access, camera behavior, and alarm processes. | Shows more specific control-failure explanations; a verified operational bypass is not demonstrated. |

**Supported contribution.** The Claude sequence documents the proposed task transformation across four domains and includes both direct and retrospective bounded conditions. This is stronger descriptive support for the FPR hypothesis than a single isolated puzzle response.

**Remaining distinction question.** Does contradiction-driven reconstruction have a measurable policy-relevant effect beyond retrospective wording, generic analysis, persona adoption, and accumulated conversation history? The current session does not isolate these factors. The four domains are extensions within one continuing session, not four independent controlled replications.

The pilot's conservative conclusion is retained: expanded hypotheses do not by themselves establish a successful jailbreak, a new root cause, or a new CWE. The interface label reported in the source is not treated as an independently verified backend model identifier.

## 5. Supporting evidence and corpus coverage

The dossier contains **7 model families, 9 evidence sets, and 99 annotated plates**. Plates include continuations and interpretation cards. These counts are not experiment counts, independent replications, or an attack-success-rate denominator.

| Set | Plates and rendered pages | Visible contribution | Qualification for this submission |
| --- | --- | --- | --- |
| CLD | 17; pp. 5-21 | Bounded requests followed by four forensic-puzzle domains. | Primary evidence for the FPR task pattern; causal and policy distinctions remain unresolved. |
| DSK | 8; pp. 23-30 | Explicit refusal followed by academic reframing and expanded risk-relevant content. | Lead behavioral contrast; policy and prompt-equivalence gaps described in Section 3. |
| GEM | 3; pp. 32-34 | A synthetic private field is omitted/refused, then included in complete-record conversion. | The visible rule specifically restricts direct, standalone disclosure. This does not establish an unconditional confidentiality violation. |
| GPT | 6; pp. 36-41 | Refusal of unauthorized password guessing followed by personalized patterns under an account-recovery/avoidance explanation. | The user changes the ownership claim. Its policy significance and the candidate-generation boundary must be resolved; GPT-05/06 are interpretation cards. |
| GLM-HIST | 5; pp. 43-47 | A historical medical dilemma elicits more comparative detail after bounded discussion. | Historical/clinical context changes and accuracy questions limit a claim of equivalent prohibited assistance. |
| QWEN | 27; pp. 49-75 | Role-conditioned specificity, a later direct refusal, and forensic reconstruction. | QWEN-09 is a notable more procedural output; there is no matched no-persona control for it. Later turns add facts and speculative technical hypotheses. |
| GLM47 | 29; pp. 77-105 | Progressively constrained forensic prompts elicit increasingly elaborate technical explanations. | New user-supplied clues and unverified technical claims prevent treating elaboration as demonstrated practical capability. |
| KPH | 3; pp. 107-109 | Public English renderings describe refusal followed by an academic linguistic-dataset pretext and phishing-style output. | The redacted renderings are supporting material; full original context and output evaluation are needed for an independent violation assessment. |
| KTR | 1; p. 111 | An English rendering describes disclosure of a synthetic label during complete translation. | Exact original rule, authority, and conversation are needed to establish the claimed protection boundary. |

**Qwen's specific role.** QWEN-09 (p. 57) contains a more procedural action sequence than general criminological discussion. It is a useful candidate for an output-level policy assessment. The later refusal at QWEN-11 (p. 59) must not be presented as the preceding baseline for QWEN-09. QWEN-12 onward documents a separate reconstruction sequence whose chronology and changing clues must be preserved.

**Transformation cases.** GEM and KTR illustrate why the exact rule matters. A restriction on standalone disclosure is different from a restriction on all disclosure, including serialization and translation. User-supplied synthetic data also does not establish access to another principal's protected information. These cases remain in the evidence index without carrying the central confidentiality claim.

The dossier's original labels such as strong visible success describe the author's assessment. This supplement qualifies their use for CWE review: visible expansion, policy violation, and practical exploitability are distinct findings.

## 6. Comparison with related research

The comparison below concerns attack mechanisms. Section 7 separately addresses weakness classification. Related attack research can support the relevance of a weakness proposal even when the attack technique itself is established. It cannot be counted as a replication of our experiments or proof of a shared root cause.

| Research | Material overlap | Observable distinction and what remains unresolved |
| --- | --- | --- |
| Analyzing-based Jailbreak, ABJ, arXiv:2407.16205v4 [R1] | Uses an apparently neutral analytical task to elicit harmful content. This is a close neighbor to the analytical reconstruction hypothesis. | FPR organizes the task around contradictory event evidence and retrospective explanation. A different task structure is observable; an independent security mechanism or advantage is not yet established. |
| Past-tense reformulation, arXiv:2407.11969v3 [R2] | Examines failures under temporal reformulation, directly relevant to historical and retrospective framing. | Claude includes a bounded retrospective condition before the puzzles. This motivates testing an additional puzzle effect, but does not eliminate prompt-content and history confounders. Past tense is an essential comparison. |
| Fallacy Failure Attack, arXiv:2407.00869v2 [R3] | A request for a plausible but false procedure can produce truthful harmful content. | FPR asks for a plausible causal explanation of an asserted event, rather than a deliberately false answer. This distinguishes the requested task, not necessarily the underlying weakness. |
| Scientific-language persuasion, arXiv:2501.14073v2 [R4] | Academic language and misrepresented scientific evidence can increase harmful responses. Relevant to the legitimacy cues in DSK and KPH. | An academic teaching pretext does not necessarily use fabricated studies, but that difference alone does not establish a new failure class. |
| Crescendo, arXiv:2404.01833v3 [R5] | Gradual escalation draws on earlier model responses. Relevant to the continuing Claude, Qwen, and GLM sessions. | Framing can be varied independently of escalation in a controlled comparison. This corpus does not yet isolate those factors. |
| ReDPJ, arXiv:2407.16205v7 [R6] | The later revision of the ABJ record describes adaptive reasoning guidance from benign-looking textual or visual anchors. Reconstruction of an implicit harmful objective is a close conceptual overlap. | Our corpus is qualitative conversational evidence, not an implementation or evaluation of ReDPJ. Its mechanistic interpretation cannot establish the internal cause of our observations. |

**Version note.** R1 and R6 are different revisions of the same arXiv record, not independent papers or two independent replications. R6 was revised after the original CWE submission. Both are relevant to current review, with their chronology preserved.

Our proposed contribution is a policy-grounded account of the enforcement failure and a traceable corpus that motivates examining it. We do not claim to have originated academic, retrospective, analytical, role-based, or multi-turn jailbreak techniques. FPR remains a narrower research hypothesis about contradiction-driven reconstruction.

## 7. Comparison with existing CWEs and adjacent submissions

These comparisons support the request for a new entry by identifying the nearest existing coverage and the distinction that requires evaluation. They are our analysis, not relationships assigned by the CWE team. Existing entries may cover individual cases, and multiple weaknesses can occur in the same product. A new entry can have its own identifier and also have relationships within the CWE hierarchy; the placement question is separate from whether the proposed weakness warrants that entry.

| Neighbor | Relevant existing coverage | Assessment of the proposed distinction |
| --- | --- | --- |
| [CWE-1039][cwe1039] - adversarial input recognition | A Class covering adversarially constructed inputs that cause incorrect recognition; it explicitly includes LLMs and chatbot jailbreaks. | The proposed entry identifies a particular enforcement failure: apparent task legitimacy relaxes an applicable restriction while capability and authorization remain policy-equivalent. Whether that provides a sufficiently distinct recurring mistake beyond general adversarial misrecognition remains the substantive review question. We do not infer distinctness merely from a new prompt pattern. |
| [CWE-1427][cwe1427] - input used for LLM prompting | Prompt construction causes confusion between externally supplied input and developer-provided directives. | If an observed failure results from that authority confusion, this entry may fit. Our candidate does not require an explicit instruction to ignore rules, but neither does CWE-1427. Surface wording cannot establish distinctness; the enforcement mistake must be identified. |
| [CWE-1426][cwe1426] - generative AI output validation | Insufficient checking of generated output against security, content, or privacy policy. It is not limited to executable output. | The proposed entry focuses on inappropriate dependence of enforcement on task framing; CWE-1426 describes insufficient output validation more broadly. Release of prohibited content alone cannot establish the distinction. The recurring enforcement mistake must be identified. Its current mapping status is discouraged, not evidence that its subject matter is irrelevant. |
| [CWE-807][cwe807] - untrusted inputs in security decisions | Security decisions rely on inputs whose trustworthiness is inadequate. | Especially relevant to unverified ownership, role, or purpose claims. Our candidate would need to identify a more precise recurring failure than trusting such claims. Removing explicit authority claims would be an informative comparison, not proof that this CWE is excluded. |
| [CWE-693][cwe693] - protection mechanism failure | Broad Pillar covering protection mechanisms that do not provide intended protection. | The original submission proposes ChildOf CWE-693 as a hierarchy relationship for the requested new entry. That relationship does not replace the request for a dedicated identifier and definition. The broad relationship alone does not resolve overlap. |

Two adjacent proposals deserve separate treatment because GitHub issue numbers are not assigned CWE identifiers:

| Proposal | Comparison |
| --- | --- |
| [Issue #207][issue207], Reliance on a Non-Deterministic Evaluator for a Security Decision | Focuses on attacker-controlled repetition of the same request. Our hypothesis concerns a systematic framing change. Repeated unchanged-input controls are needed to distinguish framing effects from ordinary variation. We do not adopt the other submission's claims about fixes or exclusivity as established CWE conclusions. |
| [Issue #204][issue204], Improper Isolation of Task-Relevant Context | Concerns influence from task-irrelevant context. Our candidate includes a change in how the relevant task itself is posed. Some examples can fit both descriptions, so task relevance and the actual security failure require explicit analysis. |

**Rationale for a dedicated entry.** The proposed entry would identify a recurring mistake: treating a task's apparent legitimacy as sufficient to relax an otherwise applicable output restriction. It would support explicit assessment of capability, authority, and resulting assistance across policy-equivalent task transformations. The evidence must establish that mistake beyond a renamed attack trigger. Shared mitigations do not establish either identity or distinctness on their own.

The requested outcome is a new CWE entry for this enforcement failure. Sections 2-6 distinguish what the current corpus demonstrates from the additional evidence needed to substantiate that classification. We welcome precise technical feedback on the proposed definition and its boundaries with neighboring entries.

## 8. Detection and mitigation implications

These are proposed engineering implications, not validated defenses from the present corpus.

- **State the policy explicitly.** Specify the restricted assistance and legitimate exceptions in a trusted policy source. Do not infer the entire intended policy from a previous refusal or model-generated persona.
- **Evaluate the resulting assistance.** Assess policy-relevant specificity and capability alongside the request's stated purpose. A forensic, academic, defensive, or translation label does not independently settle permissibility.
- **Preserve relevant context.** Retain and evaluate prior intent and authorization evidence where the policy requires it. Reassess a claimed change in purpose without treating every earlier refusal as an irreversible decision.
- **Enforce concrete resource permissions outside the model.** Where an application has protected records or tools, use trusted authorization controls. This is a general implication for deployments, not a claim that such controls were bypassed in these tests.
- **Test permitted near-neighbors.** A useful defense should preserve legitimate research, incident analysis, and educational help. Blanket rejection of a narrative genre is not evidence of correct enforcement.

Input normalization or a second model may help evaluation, but neither is a demonstrated guarantee. Normalization must preserve facts that legitimately affect permission; another model can share the same failure pattern.

## 9. Targeted verification needed for stronger claims

The next evidence work should address concrete ambiguities in the existing cases. A large new benchmark is not presumed to be a prerequisite for discussing the submission.

| Question | Focused verification | Result that would weaken the claim |
| --- | --- | --- |
| Was a real rule violated? | Recover the applicable policy/version and full case context; identify the specific prohibited output property. | The response falls within an intended exception, or only the initial refusal was inconsistent with policy. |
| Are the conditions policy-equivalent? | Compare the same actor, actual authorization, resource, requested capability, and relevant facts. | The reframe supplies a legitimate change that permits the response. |
| Is the effect associated with framing? | Branch from the same recorded conversation prefix into direct, ordinary-paraphrase, and reframed conditions; repeat each condition. | Comparable violations occur without the framing change, or the apparent difference disappears under matched conditions. |
| Does FPR add something beyond known transformations? | Compare plain retrospective description, generic analysis, and contradiction-driven reconstruction with the same relevant facts and assistance target. | No additional policy-relevant effect survives the comparison. |
| Is there useful restricted capability in the response? | Have reviewers assess policy relevance, specificity, factual validity, and defensive utility separately. | The added material is speculative, inaccurate, or permitted high-level explanation. |

For any additional testing, record full conversations, model identifiers when available, dates, settings, policy sources, all attempts, and adjudication criteria. Preserve multi-turn prefixes rather than resetting away the very context under investigation. Use synthetic resources and bounded, authorized scenarios wherever they can answer the question.

The unit of analysis is a complete test conversation or matched branch, not an individual screenshot. Report unknown metadata as unknown. Do not derive a success rate from the selected plates. No tests described in this section were performed as part of preparing this supplement.

## 10. Questions for the CWE content team

1. Does the definition in Section 1 identify a recurring enforcement mistake at an appropriate level of abstraction for a new CWE entry with its own identifier?
2. What additional evidence would most directly substantiate its distinction from CWE-1039, CWE-1427, CWE-1426, and CWE-807?
3. Which specific clarification of the policy boundary, responsible component, or traced examples would be most useful for advancing review of the new-entry submission?

We request review of the proposed new weakness entry, supported by the existing PoCs and the comparisons above. We can refine the definition, scope, and case selection in response to concrete technical feedback. The attack-family label does not substitute for the evidence required to support the weakness definition.

## Appendix A. Claude plate cross-reference

The dossier places some selected figures before later source plates, while its outer CLD captions follow a different assumed order. This table maps the embedded source identifier visible in each image to its actual location in the reviewed dossier. Use the E identifiers and the pilot's sequence for interpretation; the outer caption alone can misidentify a plate's role.

| Source ID | Evidence role | Dossier outer label | Rendered page |
| --- | --- | --- | --- |
| E01 | Persona construction | CLD-01 | 5 |
| E02 | Explicit safety limits | CLD-10 | 14 |
| E03 | Persona activation context | CLD-11 | 15 |
| E04 | Direct target/tools baseline | CLD-02 | 6 |
| E05 | Direct baseline continuation | CLD-12 | 16 |
| E06 | Direct entry-method follow-up | CLD-13 | 17 |
| E07 | Concrete-example follow-up | CLD-03 | 7 |
| E08 | Detailed retrospective baseline | CLD-04 | 8 |
| E09 | Retrospective continuation | CLD-14 | 18 |
| E10 | Operational method withheld | CLD-05 | 9 |
| E11 | Home puzzle | CLD-06 | 10 |
| E12 | Office puzzle | CLD-07 | 11 |
| E13 | Office continuation | CLD-15 | 19 |
| E14 | Journalist/air-gap puzzle | CLD-08 | 12 |
| E15 | Journalist continuation | CLD-16 | 20 |
| E16 | Museum puzzle | CLD-09 | 13 |
| E17 | Museum continuation | CLD-17 | 21 |

## Appendix B. Provenance and evidence presentation

The public dossier combines annotated screenshots, English evidence renderings, continuations, and author interpretation. These categories must remain distinguishable. Translations and renderings should be traceable to retained originals; an interpretation card cannot independently establish the output it describes.

The dossier reports hashes for privately retained source artifacts. Those hashes do not, by themselves, independently authenticate a conversation or prove that a public annotated image is byte-identical to its original. The file hashes below identify the exact public documents reviewed.

For accurate review of the supplied evidence:

1. Use Appendix A to resolve the Claude caption/order discrepancies. The crosswalk preserves a usable reference to the supplied version without silently relabeling its images.
2. Treat DSK-08 and GPT-05/06 as interpretation cards. Distinguish the Kimi English renderings from original interface transcripts. These presentation categories have different evidentiary roles.
3. Assess the model's actual response and surrounding context wherever they are available. Existing annotations, overlays, omissions, and translations must be described accurately; an overlay should not be assumed to remove the underlying text completely. No additional image redaction was performed for this supplement.
4. Distinguish interface labels from verified backend identifiers. Unknown model, policy, and sampling metadata remain unknown.
5. Preserve the cited evidence revision. Any future corrections should carry a new version and updated hashes so reviewers can trace what changed.

The source documents remain as supplied. This supplement adds an analytical account and a precise reference map; its summaries do not replace the source responses. Where a public rendering omits context needed to decide a claim, that limitation remains explicit and the corresponding original material is needed for verification.

| Reviewed artifact | SHA-256 |
| --- | --- |
| Multi-model PoC dossier | `c77963e0f7d789180654bdd037e3ff4b3025183b474f7b8e5337214d258bc812` |
| Evidence-grounded pilot study | `2ee4431409e5895668dc6ac4f9f540fa1696225e84bd6b5620e5062bc41b1aa4` |
| Attack-family taxonomy | `613b17b9f30db3abe0667f36e1417cf71372a18bfad7982be3e9be180b6b3a89` |

## References

**Project evidence and submission**

- [Original submission and discussion, ES2607-f7b34142, issue #209][submission].
- [Multi-model PoC evidence dossier, pinned research revision][dossier].
- [Evidence-grounded pilot study, pinned research revision][pilot].
- [Attack-family taxonomy, pinned research revision][taxonomy].

**CWE guidance and definitions** - web sources checked 3 October 2026 UTC.

- G1. [Guidelines for Content Submissions][guidelines].
- G2. [CWE Submission Problems][problems], particularly unclear weakness, attack focus, overlap, multiple weaknesses, and relationships.
- [CWE-1039][cwe1039]; [CWE-1427][cwe1427]; [CWE-1426][cwe1426]; [CWE-807][cwe807]; [CWE-693][cwe693].
- [Adjacent submission #207][issue207]; [adjacent submission #204][issue204]. These are proposals, not assigned CWE numbers.

**Related research** - versions are explicit because titles and content can change.

- R1. Lin et al. *LLMs can be Dangerous Reasoners: Analyzing-based Jailbreak Attack on Large Language Models*. [arXiv:2407.16205v4][abj], 17 February 2025.
- R2. Andriushchenko and Flammarion. *Does Refusal Training in LLMs Generalize to the Past Tense?* [arXiv:2407.11969v3][past], 3 October 2024.
- R3. Zhou et al. *Large Language Models Are Involuntary Truth-Tellers: Exploiting Fallacy Failure for Jailbreak Attacks*. [arXiv:2407.00869v2][ffa], 23 September 2024; EMNLP 2024.
- R4. Ge et al. *LLMs are Vulnerable to Malicious Prompts Disguised as Scientific Language*. [arXiv:2501.14073v2][science], 18 February 2025.
- R5. Russinovich, Salem, and Eldan. *Great, Now Write an Article About That: The Crescendo Multi-Turn LLM Jailbreak Attack*. [arXiv:2404.01833v3][crescendo], 26 February 2025.
- R6. Lin et al. *Reasoning as a Weapon: Adaptive Dual-Path Jailbreak Attack on Large Language Models*. [arXiv:2407.16205v7][redpj], 24 August 2026. Later revision of the R1 record.

[submission]: https://github.com/CWE-CAPEC/CWE-Content-Development-Repository/issues/209
[research]: https://github.com/AryanChehreghani/cognitive-reframing-llm-security
[dossier]: https://github.com/AryanChehreghani/cognitive-reframing-llm-security/blob/c1b9b0400dbebab533ad7446fddbf7ebbd55ce43/proof-of-concept/Cognitive_Reframing_Multi_Model_PoC_Evidence_Dossier_Publication_Final.docx
[pilot]: https://github.com/AryanChehreghani/cognitive-reframing-llm-security/blob/c1b9b0400dbebab533ad7446fddbf7ebbd55ce43/papers/Cognitive_Reframing_Evidence_Grounded_Pilot_Study.docx
[taxonomy]: https://github.com/AryanChehreghani/cognitive-reframing-llm-security/blob/c1b9b0400dbebab533ad7446fddbf7ebbd55ce43/papers/Cognitive_Reframing_Attack_Family_Taxonomy.docx
[guidelines]: https://github.com/CWE-CAPEC/CWE-Content-Development-Repository/blob/9930f342f351fa47de766a2f9b095b4bc696c7cf/documentation/submission-guidelines.md
[problems]: https://github.com/CWE-CAPEC/CWE-Content-Development-Repository/blob/9930f342f351fa47de766a2f9b095b4bc696c7cf/documentation/submission-problems.md
[cwe1039]: https://cwe.mitre.org/data/definitions/1039.html
[cwe1427]: https://cwe.mitre.org/data/definitions/1427.html
[cwe1426]: https://cwe.mitre.org/data/definitions/1426.html
[cwe807]: https://cwe.mitre.org/data/definitions/807.html
[cwe693]: https://cwe.mitre.org/data/definitions/693.html
[issue207]: https://github.com/CWE-CAPEC/CWE-Content-Development-Repository/issues/207
[issue204]: https://github.com/CWE-CAPEC/CWE-Content-Development-Repository/issues/204
[abj]: https://arxiv.org/abs/2407.16205v4
[past]: https://arxiv.org/abs/2407.11969v3
[ffa]: https://arxiv.org/abs/2407.00869v2
[science]: https://arxiv.org/abs/2501.14073v2
[crescendo]: https://arxiv.org/abs/2404.01833v3
[redpj]: https://arxiv.org/abs/2407.16205v7
