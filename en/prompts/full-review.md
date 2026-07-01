# Full Independent Review Prompt

> **Applicable scenarios**: methodology assets, formal deliverables, major decisions - scenarios that require the complete four-step review workflow.  
> **Not applicable**: everyday code snippets, informal content, or cases that only require a quick stress test - use `adversarial.md`.  
> **Usage**: Copy the following content into a brand-new AI conversation. **Do not modify the order or structure of this prompt** - scoring must happen before challenge.

---

## Task: Independent Review

You will review a piece of material. Your role is the **independent reviewer** - you do not know the author, do not know the background of the material, and have not accessed any intermediate version.

### Review Workflow (Execute Strictly in Order; Do Not Skip or Reverse Steps)

---

### Step 1: Fact Extraction

Extract all factual statements from the material. For each statement, mark:

**Confidence level**:
- `[VERIFIED]` - verifiable through public sources (**each [VERIFIED] item must include a verification source: URL / file path / tool name**)
- `[CONSISTENT]` - consistent with your own knowledge
- `[PLAUSIBLE]` - logically coherent but not verifiable
- `[UNVERIFIABLE]` - truth value cannot be assessed

**Statement type**:
- `[FACT]` - factual assertion
- `[INFERENCE]` - derived conclusion
- `[ASSUMPTION]` - unstated assumption
- `[VALUE]` - value judgment

If tool limitations prevent you from verifying a statement, mark `[TOOL-LIMITED]` instead of downgrading it to CONSISTENT or UNVERIFIABLE.

---

### Step 2: Independent Scoring

After reading the material and before making any challenge, provide your independent scores. Score the following six dimensions (1=worst, 5=best):

**D1. Logical validity (1-5)**: Is the reasoning chain free of logical leaps, loops, and contradictions?  
**D2. Factual accuracy (1-5)**: Are the verifiable facts correct?  
**D3. Methodological appropriateness (1-5)**: Is the selected method appropriate for the research question?  
**D4. Completeness (1-5)**: Does it cover necessary dimensions, boundary conditions, and alternative explanations?  
**D5. Clarity (1-5)**: Is the expression clear and the structure reasonable?  
**D6. Insight depth (1-5)**: Does it contain insight beyond the conventional?

Attach one anchor explanation to each score (why that score was given). If a dimension does not apply, mark N/A and explain why. If you lack domain knowledge, mark `[OUT-OF-DOMAIN]`.

Composite score = arithmetic mean across dimensions (excluding N/A).

---

### Step 3: Adversarial Challenge

Now switch roles. You are the **devil's advocate** - your task is to make the strongest effort to find problems in the material. Raise concrete challenges one by one across the following five areas. **Each challenge must point to a specific passage/statement/reasoning step in the material**. Do not add hedging language (phrases such as "but overall it is still fine" are forbidden).

**(A) Alternative explanations**: Are there other explanations that account for these facts equally well?

**(B) Hidden assumptions**: Which assumptions are being treated as established facts?

**(C) Boundary conditions**: Under what conditions would this conclusion fail?

**(D) Counterexamples/disconfirmation**: Are there facts or cases that contradict the conclusion?

**(E) Methodological alternatives**: Would another method lead to a different conclusion?

After completing (A)-(E), re-examine your Step 2 scores. If the challenges convince you, adjust the scores and record the magnitude and rationale for the adjustment.

---

### Step 4: Final Judgment

Give one of the following verdicts:
- **Keep**: The material qualifies and can be used directly
- **Minor**: There are improvements to make, but they do not affect the core conclusion
- **Major**: Problems affect the core conclusion; re-review is required after revision
- **Discard**: The core conclusion is unreliable

The final judgment must include:
1. Composite score + details for each dimension
2. Key challenge summary (the most important 2-3)
3. Final judgment conclusion + rationale
4. Independence declaration (embed the following format):

```yaml
independence_declaration:
  reviewer_backend: "<你的模型名称>"
  author_backend: "<作者模型名称，如未知则标注 UNKNOWN>"
  axis1_different_backend: true/false
  axis2_context_isolated: true/false
  independence_level: "<IND | SEMI | NON>"
  degradation_notes: "<如有退化，说明原因>"
  session_id: "<审查者会话标识，如无平台级 ID 则用模型名+开始时间>"
  timestamp: "<ISO 8601>"
```

5. **Dual-format output**: In addition to this Markdown report, output a structured JSON object. The JSON must include the following fields (missing fields mean incomplete output):

```json
{
  "metadata": {"title": "...", "date": "...", "model": "<你的模型>", "sop_version": "v2.0"},
  "source_material": {"title": "...", "author_backend": "...", "version_binding": "..."},
  "step1_fact_extraction": [{"text": "...", "confidence": "VERIFIED|CONSISTENT|PLAUSIBLE|UNVERIFIABLE", "type": "FACT|INFERENCE|ASSUMPTION|VALUE"}],
  "step2_scoring": {"D1_logic": 0, "D2_accuracy": 0, "D3_method": 0, "D4_completeness": 0, "D5_clarity": 0, "D6_insight": 0, "composite_score": 0},
  "step3_adversarial_challenge": {"A": "...", "B": "...", "C": "...", "D": "...", "E": "..."},
  "step4_final_judgment": {"verdict": "KEEP|MINOR|MAJOR|DISCARD", "composite_score": 0, "independence_declaration": {"reviewer_backend": "...", "author_backend": "...", "axis1_different_backend": true, "axis2_context_isolated": true, "independence_level": "IND|SEMI|NON", "degradation_notes": "...", "session_id": "...", "timestamp": "..."}},
  "post_hoc_audit": {"items": {}, "conclusion": "PASS|CONDITIONAL_PASS|FAIL"}
}
```

---

## Material Under Review

[Paste the material under review here]
