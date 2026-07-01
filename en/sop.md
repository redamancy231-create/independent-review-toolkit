# Independent Review Standard Operating Procedure (Independent Review SOP)

> **Version**: v2.0.0 (first standalone release, extracted and generalized from AI Collaboration Project Full Lifecycle Framework v1.6.4)  
> **Date**: 2026-07-01  
> **Predecessor**: methodological-review-sop.md v1.0.4 (framework companion version)  
> **Changes**: Removed framework-specific references, generalized trigger conditions, added adaptation for a standalone repo  
> **Generation model**: DeepSeek-V4-Pro (via Claude Code CLI)  
> **License**: CC BY 4.0

---

## Section 1 When to Trigger

### 1.1 Mandatory Trigger Conditions (T1-T4)

| ID | Trigger condition | Description |
|------|---------|------|
| **T1** | Methodology asset output | Frameworks, SOPs, classification systems, evaluation rubrics, templates - any structural output that will be reused by downstream projects |
| **T2** | A conclusion will enter a formal deliverable | Papers, academic reports, externally released analytical conclusions |
| **T3** | A decision involves resource allocation or irreversible action | Data deletion, architecture rewrite, public release, competition result submission |
| **T4** | Two or more AIs/agents give conflicting conclusions | When an independent third-party decision is needed |

### 1.2 Recommended Trigger Conditions (S1-S3)

| ID | Trigger condition |
|------|---------|
| **S1** | Single-model output with high complexity (a chained conclusion with more than 3 reasoning steps) |
| **S2** | A novel or unvalidated method or toolchain was used |
| **S3** | The model is known to have systematic blind spots in the relevant domain |

### 1.3 Non-Trigger Cases

- Pure format conversion (no content judgment)
- An independent review conclusion already exists and the input has not changed
- The user explicitly states "no review is needed" (the declaration time and context must be recorded). **Restriction**: T2/T3 must not be silently skipped; the risk acceptor, reason, and timestamp must be recorded.

**Alternative path**: When T1-T4 is triggered but bare environment conditions (Section 5.1 C1-C4) cannot be satisfied (for example, only one backend is available or a new conversation cannot be opened), the review does not qualify for this SOP's complete four-step process. The operator must run the following alternative workflow:
1. Record the degradation reason (why C1-C4 is infeasible)
2. Choose the most feasible review method (for example, same-backend cross-checking, author self-review + recorded blind-spot risk)
3. Explicitly declare the degradation level and risk in the report (see Section 5.2)
4. Mark the report as `[NON]` or `[DEGRADED]`; it must not be used as formal independent review evidence
  
A report produced by the alternative workflow is not an "independent review"; it is a "constrained internal cross-check." Its value is in recording verification steps already performed and known blind spots, not in providing confidence equivalent to [IND].

---

## Section 2 Independence Definition and Assessment

### 2.1 Dual-Axis Assessment Model

Independence is assessed along two axes. **Both are required**:

**Axis 1: Backend Model Independence**
- The AI model used by the reviewer must be **different** from the model used by the author
- "Another agent," "another conversation," and "different temperature parameters on the same model" do not constitute independence
- Different companies (Anthropic / OpenAI / DeepSeek / Google) -> independent
- Different models from the same company -> semi-independent (must be declared in the report and down-weighted)

**Axis 2: Context Isolation**
- The reviewer must not be exposed to the author's intermediate reasoning, scoring, or conclusion
- The reviewer may only access: source material (the object under review) + review rubric + bare environment

### 2.2 Three-Level Independence Classification

| Level | Axis 1 | Axis 2 | Tag | Use |
|------|-----|-----|------|------|
| **Independent [IND]** | Different backend | Fully isolated | `[IND]` | Can be used as primary evidence |
| **Semi-independent [SEMI]** | Different model from the same company or partial Axis 2 isolation | Partial isolation | `[SEMI]` | Secondary evidence; limitations must be declared. Independence for different models from the same company depends on architectural difference: different architecture families (such as GPT-4o vs o3) -> may be treated as semi-independent; scaled variants in the same architecture (such as GPT-4o vs GPT-4o-mini) -> tends toward NON. The reviewer explains the specific assessment rationale in the independence declaration. |
| **Non-independent [NON]** | Same backend or Axis 2 not isolated | Not isolated | `[NON]` | Must not be used as independent review |

### 2.3 Assessment Decision Tree

```
Reviewer backend != author backend?
  |-- Yes -> Fully context-isolated?
  |        |-- Yes -> [IND] Independent
  |        `-- No  -> [SEMI] Semi-independent (declare leakage point)
  `-- No  -> Different model from the same company?
           |-- Yes -> Fully context-isolated?
           |        |-- Yes -> [SEMI] Semi-independent
           |        `-- No  -> [NON] Non-independent
           `-- No  -> [NON] Non-independent
```

---

## Section 3 Role Separation

### 3.1 Three-Role Model

| Role | Responsibility | Must not be |
|------|------|---------|
| **Author** | Produces the content under review | - |
| **Operator** | Sets up the bare environment, transfers material, and maintains the information firewall | Must not be the author model and must not be the reviewer model |
| **Reviewer** | Independent scoring + adversarial challenge + final judgment | Must not share a backend with the author and must not access the author's intermediate outputs |

### 3.2 Operator Firewall Responsibilities

- When extracting source material from the author, remove all scoring, self-assessment, and conclusion-oriented judgment
- Before sending material to the reviewer, verify bare environment integrity (Section 5 checklist)
- Do not disclose to the reviewer "where the author thinks it is good/problematic"
- Do not disclose the reviewer's scores to the author until the final judgment is complete

---

## Section 4 Execution Workflow

### 4.1 Four-Step Overview

```
Source material -> [Step 1: Fact extraction] -> [Step 2: Independent scoring] -> [Step 3: Adversarial challenge] -> [Step 4: Final judgment]
                       ^                              ^                                  ^                                  ^
                  Operator transfer              Scoring before challenge!         Adjust score after challenge       Embed independence declaration
                  (bare environment check)       (anti-contamination rule)         (adversarial turn)                 (provenance locked)
```

### 4.2 Key Constraints

1. **Step 2 (scoring) must happen before Step 3 (challenge)**: Scores are based on the initial understanding, not an understanding reshaped by the challenge. Reversing the order will bias the reviewer through their own challenge.
2. **Step 3 must include the "devil's advocate must not soften" constraint**: The challenger must make the strongest effort to find problems and must not end with "but overall it is still fine."
3. **Every step output must mark provenance**: model backend + session identifier + timestamp.

---

## Section 5 Bare Environment Setup

### 5.1 Ten-Item Checklist

Before review begins, the operator confirms each item:

| ID | Check item | Pass criterion |
|------|--------|---------|
| **C1** | New conversation | The reviewer conversation contains no project history |
| **C2** | No memory injection | The reviewer does not load the user's memory/preference files |
| **C3** | No project documents | Only this SOP + source material + rubric are provided |
| **C4** | No author scoring | All self-assessment/conclusions are stripped from the source material |
| **C5** | No intermediate outputs | The reviewer does not see author drafts/reasoning chains |
| **C6** | Different backend | The reviewer uses a model backend different from the author's |
| **C7** | No prior labels | The reviewer is not told "who wrote this" or "what level it is" |
| **C8** | Clean rubric | The review rubric contains no leading language |
| **C9** | Timestamp recording | Every operation records a timestamp |
| **C10** | Version binding | The version identifier of the material under review has been recorded (git hash/file hash/version number) |

### 5.2 Degradation Detection

- **Fully bare environment**: C1-C10 all pass -> [IND] is reachable
- **Partial degradation**: C6 is infeasible (same backend) -> [NON] at most
- **Severe degradation**: Any of C1-C4 is infeasible -> downgrade to "internal cross-check"; this SOP does not apply

---

## Section 6 Step 1: Fact Extraction

### 6.1 Confidence Levels

| Level | Tag | Definition |
|------|------|------|
| **L1** | `[VERIFIED]` | Verifiable through public, checkable sources |
| **L2** | `[CONSISTENT]` | Consistent with the reviewer's own knowledge |
| **L3** | `[PLAUSIBLE]` | Logically coherent but not verifiable |
| **L4** | `[UNVERIFIABLE]` | Truth value cannot be assessed |

> **Numbering note**: L1-L4 are confidence levels. Trigger conditions (Section 1.1) use T1-T4 (Trigger). The two numbering systems do not conflict.

### 6.2 Statement Classification

Each extracted statement is tagged by type: `[FACT]` factual assertion / `[INFERENCE]` derived conclusion / `[ASSUMPTION]` unstated assumption / `[VALUE]` value judgment.

### 6.3 Tool-Limited Marking

If the reviewer cannot verify a statement because of tool limitations, mark `[TOOL-LIMITED]` instead of downgrading its confidence level.

---

## Section 7 Step 2: Independent Scoring

### 7.1 Six-Dimensional Scoring Framework

| Dimension | Name | Range | Description |
|------|------|------|------|
| **D1** | Logical validity | 1-5 | Whether the reasoning chain has no logical leaps, loops, or contradictions |
| **D2** | Factual accuracy | 1-5 | Whether verifiable facts are correct |
| **D3** | Methodological appropriateness | 1-5 | Whether the selected method fits the research question |
| **D4** | Completeness | 1-5 | Whether necessary dimensions, boundary conditions, and alternative explanations are covered |
| **D5** | Clarity | 1-5 | Whether expression is clear and structure is reasonable |
| **D6** | Insight depth | 1-5 | Whether it contains insights beyond the conventional |

### 7.2 Scoring Constraints

- **Evaluate reasoning, not conclusions (D2 is the exception)**: D1/D3-D6 evaluate "how the reasoning reaches the conclusion," not "whether the conclusion is right." **D2 (factual accuracy) is the exception**: verify factual assertions and conclusion reliability first, then evaluate reasoning quality. The core of D2 is whether facts are correct. This is directly related to conclusion reliability, so the "do not evaluate conclusions" constraint does not apply.
- **N/A allowed**: Mark N/A and explain the reason when a dimension does not apply
- **Anchor explanation**: Attach one-sentence explanation to every score
- **Know what you do not know**: Mark `[OUT-OF-DOMAIN]` when domain knowledge is insufficient

### 7.3 Composite Score

Composite score = arithmetic mean across dimensions (excluding N/A dimensions). Do not apply weights. Weighting is itself a value judgment and should be decided by the human user, not delegated to the reviewer. **Customization exit**: If the review scenario requires weighting (for example, safety-critical system review -> weighted D2 factual accuracy), the weighting scheme and rationale must be declared in the final judgment. Use an equal-weight arithmetic mean by default.

---

## Section 8 Step 3: Adversarial Challenge

### 8.1 Five Required Challenge Areas

| ID | Challenge | Trigger question |
|------|------|---------|
| **(A)** | Alternative explanations | "Are there other explanations that account for these facts equally well?" |
| **(B)** | Hidden assumptions | "Which assumptions are being treated as established facts?" |
| **(C)** | Boundary conditions | "Under what conditions would this conclusion fail?" |
| **(D)** | Counterexamples/disconfirmation | "Are there facts or cases that contradict the conclusion?" |
| **(E)** | Methodological alternatives | "Would another method lead to a different conclusion?" |

> **Default setting**: (A)-(E) are full-coverage challenges and apply to methodology asset and formal deliverable reviews. **Customization exit**: For lightweight review scenarios (such as everyday code snippets or informal content), some areas may be selectively omitted, but the omitted areas and rationale must be declared in the final judgment. Do not omit an area out of convenience. If any of (A)-(E) is omitted, it must be because that area does not apply to the material under review, not because it is easier.

### 8.2 Challenge Constraints

1. **The devil's advocate must not soften**: Each challenge is stated in its sharpest form, with no hedging language
2. **Specific, not abstract**: Point to specific passages/statements/reasoning steps in the material
3. **Post-challenge score adjustment**: After completing (A)-(E), re-examine the Step 2 scores

### 8.3 Post-Challenge Score Adjustment Rules

```
IF the challenge finds a problem not noticed in Step 2:
  -> Lower the relevant dimension score and record "adjusted from X to Y; reason: challenge (Z) revealed..."
IF the challenge finds no new problem (the material withstands challenge):
  -> Keep the original score and record "withstood (A)-(E) challenges; score unchanged"
IF the challenge finds a fatal problem:
  -> Trigger red flags R1-R3 (see Section 11), possibly directly recommending "Discard"
```

---

## Section 9 Step 4: Final Judgment

### 9.1 Four-Level Final Judgment

| Verdict | Meaning | Follow-up action |
|------|------|---------|
| **Keep** | The material qualifies and can be used directly | Mark the review conclusion and archive |
| **Minor** | There are improvements to make, but they do not affect the core | List recommendations; no re-review needed |
| **Major** | Problems affect the core conclusion | List issues; re-review after modification is required |
| **Discard** | The core conclusion is unreliable | Record the reason and trigger root-cause analysis |

### 9.2 Final Judgment Output

1. Composite score (D1-D6 average) + details for each dimension
2. Independence level ([IND] / [SEMI] / [NON])
3. Key challenge summary (the most important 2-3 from A-E)
4. Final judgment conclusion + rationale
5. Independence declaration (see 9.3)

### 9.3 Independence Declaration Format (Mandatory Embedding)

```yaml
independence_declaration:
  reviewer_backend: "<模型名称>"
  author_backend: "<作者模型名称>"
  axis1_different_backend: true/false
  axis2_context_isolated: true/false
  independence_level: "IND | SEMI | NON"
  degradation_notes: "<退化说明，如无可省略>"
  session_id: "<审查者会话标识>"
  timestamp: "<ISO 8601>"
```

---

## Section 10 Post-Hoc Audit

### 10.1 Nine Audit Items

| ID | Audit item | Pass criterion |
|------|--------|---------|
| **A1** | Backend verification | Reviewer backend != author backend |
| **A2** | Conversation isolation | Reviewer conversation has no project context leakage |
| **A3** | Scoring first | Step 2 scoring timestamp is earlier than Step 3 challenge timestamp |
| **A4** | Challenge does not soften | Step 3 (A)-(E) contains no hedging language |
| **A5** | Complete provenance | Every step has backend + session + timestamp |
| **A6** | Declaration embedded | Final judgment includes a complete independence_declaration |
| **A7** | Adjustments supported | Score adjustments record magnitude and rationale |
| **A8** | N/A justified | N/A marks have reasonable explanations |
| **A9** | Version binding | The reviewed object version identifier is traceable |

### 10.2 Audit Conclusion

| Conclusion | Condition | Follow-up |
|------|------|------|
| **Audit pass** | A1-A9 all pass | Can be used as formal evidence |
| **Conditional pass** | A5/A7/A8/A9 fail, but A1-A4/A6 pass | Mark as conditionally usable |
| **Audit fail** | Any of A1-A4 or A6 fails | Must not be used; re-review required |

---

## Section 11 Red Flag Clauses

> **Priority**: Red flag clauses take priority over post-hoc audit (Section 10). When a red flag and an audit item detect the same issue (for example, R5 model rewrapping vs A1 backend verification), execute the red flag handling path (void review/re-review) and do not replace it with audit downgrading.

If any of the following is triggered, the review pauses and the operator decides the next steps:

| ID | Red flag | Recommended handling |
|------|------|---------|
| **R1** | Core factual error (L2 or above, affects the core conclusion) | Recommend discard |
| **R2** | Circular reasoning (the conclusion is used as a premise to derive itself) | Recommend major revision |
| **R3** | Category error (the method does not apply to the problem type) | Recommend discard or major revision |
| **R4** | Independence false attestation (the reviewer actually accessed the author's intermediate outputs) | Void the review and re-review |
| **R5** | Model rewrapping (reviewer and author are different wrappers around the same backend) | Void the review |
| **R6** | Review decay (scoring/challenge is significantly below the model's historical performance) | Low confidence; consider supplemental review |
| **R7** | Unavailable data substitution (alternative data used without declaring limitations) | Mark data contamination risk |
| **R8** | Temporal mismatch (outdated information used to verify a current conclusion) | Mark temporal limitation |
| **R9** | Bare environment leakage (C1-C9 failed but was not declared) | Downgrade the review |

---

## Section 12 Common Failure Modes

### 12.1 Pseudo-Independence
**Symptom**: "The same model on another machine" is treated as an "independent reviewer"  
**Detection**: Same Axis 1 backend = non-independent, regardless of packaging  
**Prevention**: Enforce a different backend on Axis 1 and record provenance on the spot

### 12.2 Review Decay
**Symptom**: The reviewer's adversarial posture declines over time; the 5th review is significantly softer than the 1st  
**Detection**: Compare the sharpness of (A)-(E) challenges from the same reviewer at different time points  
**Prevention**: Enforce the "devil's advocate must not soften" constraint (Section 8.2), audit A4

### 12.3 Tool-Limitation Camouflage
**Symptom**: The reviewer cannot verify because of tool limitations but does not mark [TOOL-LIMITED]  
**Detection**: Inspect the verification method for each Step 1 statement  
**Prevention**: Mandatory marking rule in Section 6.3 + the N/A and [OUT-OF-DOMAIN] mechanisms in Section 7.2

---

## Section 13 Output Format

### 13.1 Dual-Format Output Specification

Every independent review produces two files:
1. **`.md` file** (human-readable): a complete review report containing all steps, scores, challenges, final judgment, and audit conclusion
2. **`.json` file** (machine-readable): structured data, with fields corresponding one-to-one to the md sections

### 13.2 JSON Structure Specification

```json
{
  "metadata": {
    "title": "独立审查报告",
    "version": "1.0",
    "date": "YYYY-MM-DD",
    "model": "<审查者后端模型名>",
    "sop_version": "v2.0.0"
  },
  "trigger": {
    "type": "T1|T2|T3|T4|S1|S2|S3",
    "description": "..."
  },
  "source_material": {
    "title": "...",
    "author_backend": "<作者模型名称>",
    "type": "methodology|analysis|conclusion|other",
    "version_binding": {
      "version_id": "git commit hash / 文件 hash / 版本号",
      "binding_method": "git-commit | file-hash | version-number",
      "verified_at": "ISO 8601 timestamp"
    }
  },
  "step1_fact_extraction": {
    "declarations": [
      {
        "text": "...",
        "confidence": "VERIFIED|CONSISTENT|PLAUSIBLE|UNVERIFIABLE",
        "type": "FACT|INFERENCE|ASSUMPTION|VALUE",
        "verification_source": "<VERIFIED 须附来源 URL/路径/工具名>"
      }
    ]
  },
  "step2_scoring": {
    "scores": {
      "D1_logic": {"score": null, "anchor": "..."},
      "D2_accuracy": {"score": null, "anchor": "..."},
      "D3_method": {"score": null, "anchor": "..."},
      "D4_completeness": {"score": null, "anchor": "..."},
      "D5_clarity": {"score": null, "anchor": "..."},
      "D6_insight": {"score": null, "anchor": "..."}
    },
    "composite_score": null,
    "na_dimensions": []
  },
  "step3_adversarial_challenge": {
    "challenges": {
      "A_alternative_explanations": "...",
      "B_hidden_assumptions": "...",
      "C_boundary_conditions": "...",
      "D_counterexamples": "...",
      "E_method_alternatives": "..."
    },
    "post_challenge_score_adjustments": []
  },
  "step4_final_judgment": {
    "verdict": "KEEP|MINOR|MAJOR|DISCARD",
    "composite_score": null,
    "independence_declaration": {
      "reviewer_backend": "...",
      "author_backend": "...",
      "axis1_different_backend": true,
      "axis2_context_isolated": true,
      "independence_level": "IND|SEMI|NON",
      "degradation_notes": "...",
      "session_id": "...",
      "timestamp": "..."
    },
    "key_challenges_summary": [],
    "rationale": "..."
  },
  "post_hoc_audit": {
    "audit_items": {},
    "audit_conclusion": "PASS|CONDITIONAL_PASS|FAIL",
    "auditor": "..."
  }
}
```

---

## Section 14 Glossary

| Term | Definition |
|------|------|
| **Backend** | The model that actually performs inference, such as GPT-5.5, DeepSeek-V4-Pro, Qwen3.7-Max |
| **Source Material** | The output artifact under review |
| **Bare Environment** | A review conversation without project context, author intermediate outputs, or memory (see Section 5.1 C1-C10 for the full checklist) |
| **Operator** | The human/agent that sets up the bare environment and acts as the information firewall |
| **Devil's Advocate** | Systematically argues for the opposing side; used for stress testing |
| **provenance** | Model backend source record used to trace independence after the fact |

---

*Source: extracted from AI Collaboration Project Full Lifecycle Framework v1.6.4 Section 9.2 + methodological-review-sop.md v1.0.4*  
*Generation model: DeepSeek-V4-Pro (via Claude Code CLI) · 2026-07-01*
