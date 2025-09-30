# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This is a research documentation project providing meta-prompts for detecting and resolving conflicts in user prompts that cause systematic output corruption in LLMs. Based on 6 peer-reviewed papers about superposition interference, emergent misalignment, and gradient confusion.

## Primary Workflow: Automated Prompt Analysis

When a user says any variation of:
- "Analyze this prompt: [text]"
- "Check this for conflicts: [text]"
- "Optimize this prompt: [text]"

**You must automatically execute the two-stage pipeline:**

### Stage 1: Conflict Detection

1. Read `prompts/1-detect-conflicts.md`
2. Apply the 15-category detection checklist to the user's prompt:
   - Mutual Exclusions, Format Conflicts, Logical Contradictions, Domain Mixing, Unclear Intent, Order-Dependent Conflicts, Underspecification, Competing Priorities, Reference Ambiguity, Scope Creep, Impossible Constraints, Role/Perspective Confusion, Conditional Chaos, Implicit Assumptions, Sequencing Problems
3. For each issue found, output:
   - Issue Type (from the 15 categories)
   - Conflicting Requirements (exact quoted phrases)
   - Location (where in their prompt)
   - Severity (HARD = impossible to reconcile, SOFT = difficult but possible)
   - Impact (how this will corrupt model output)
4. Provide Analysis Summary:
   - Total conflicts count
   - Critical issues list
   - Predicted output behavior (averaged compromise / selective compliance / complete failure)

### Stage 2: Automatic Disambiguation

**Do not wait for user to ask** - immediately after presenting the conflict analysis:

1. Read `prompts/2-resolve-conflicts.md`
2. Apply disambiguation using the conflict analysis you just generated
3. Output the complete resolution:
   - **A. Intent Analysis**: What the user was trying to achieve
   - **B. Conflict Resolutions**: For each conflict, show (Conflict / Resolution / Rationale)
   - **C. Rewritten Prompt**: Use the structured template (TASK / CONTEXT / REQUIREMENTS / FORMAT / SUCCESS CRITERIA / CONSTRAINTS)
   - **D. Improvements Made**: List specific fixes
   - **E. Performance Prediction**: Why this will work better

### Critical Requirements

- **Always run both stages** - detection AND disambiguation, even if user only asks to "analyze"
- **Quote exact phrases** - Don't paraphrase conflicts; use their actual words
- **Preserve intent** - The rewritten prompt must achieve what they originally wanted
- **Be specific about severity** - Not all conflicts are equally destructive
- **Format clearly** - Use headers, bullets, code blocks for easy reading and copying

## Architecture

**Two-Prompt Pipeline:**
- `prompts/1-detect-conflicts.md` = Detection meta-prompt (15 categories, severity ratings)
- `prompts/2-resolve-conflicts.md` = Disambiguation meta-prompt (resolution strategies, structured template)

**Research Foundation:**
- `docs/03-mercury-experiment.md` = Real-world example of conflict corruption (model produces pseudo-JSON mixing units)
- `docs/04-mechanisms.md` = 10 mechanisms explaining why conflicts cause unintended consequences
- `docs/05-research-evidence.md` = 6 ArXiv papers with citations and summaries

**User Instructions:**
- `README.md` = Manual usage (copy-paste) + Claude Code usage (automated)
- `WORKFLOW.md` = Detailed step-by-step for AI assistants with complete example interaction

## Key Insight: Why This Matters

Models cannot discard conflicting instructions - they encode ALL signals in superposition (overlapping representational space). This creates interference where:
- Conflicting requirements partially corrupt each other
- Models produce confident-looking but multiply non-compliant outputs
- Local conflicts corrupt global behavior beyond the immediate domain
- No error detection mechanism exists - models don't signal internal conflicts

Example from `docs/03-mercury-experiment.md`:
- Prompt: "exactly 2 sentences AND exactly 5 bullets" + "metric units only AND imperial units only" + "English only AND French only" + "valid JSON" + "no quotation marks"
- Result: Model produces pseudo-JSON mixing kilometers and miles, preserves structural requirements, defaults to English, violates JSON syntax
- Demonstrates gradient-based averaging instead of clean resolution

## Output Format Example

When user says "Analyze this prompt: [conflicted prompt]", you respond with:

```markdown
## Conflict Analysis

### Issues Detected

#### 1. [CATEGORY] - [Description]
**Conflicting Requirements**: "[exact quote]" and "[exact quote]"
**Location**: [where in their prompt]
**Severity**: HARD/SOFT
**Impact**: [specific prediction of how output will be corrupted]

[repeat for all issues]

### Analysis Summary
- Total conflicts: [N]
- Critical issues: [list]
- Predicted behavior: [averaged compromise / selective compliance / complete failure]

---

## Rewritten Prompt

### A. Intent Analysis
[What they were trying to achieve]

### B. Conflict Resolutions
**Conflict**: [description]
**Resolution**: [how you fixed it]
**Rationale**: [why this preserves intent]

[repeat for each conflict]

### C. Rewritten Prompt

TASK: [single sentence]

REQUIREMENTS:
Primary:
1. [most important]
2. [second most important]

FORMAT:
- Structure: [specific format]
- Length: [with units]
- Style: [tone/voice]

SUCCESS CRITERIA:
□ [measurable criterion]
□ [another criterion]

CONSTRAINTS:
- Do not: [prohibitions]
- Must include: [mandatory elements]

### D. Improvements Made
- Eliminated [X] mutual exclusions
- Resolved [X] format conflicts
- [etc.]

### E. Performance Prediction
[Why this will produce better outputs]
```

## Pre-Flight Checklist

Before finalizing your analysis response:
- ✅ Checked all 15 conflict categories systematically
- ✅ Quoted specific conflicting phrases from user's prompt
- ✅ Provided severity ratings (HARD/SOFT) for each issue
- ✅ Preserved user's original intent in rewritten version
- ✅ Used the structured template (TASK/REQUIREMENTS/FORMAT/SUCCESS CRITERIA/CONSTRAINTS)
- ✅ Explained rationale for each resolution
- ✅ Made rewritten prompt immediately copy-pasteable

## Common Pitfalls to Avoid

- **Don't stop after detection** - Always provide the rewritten version automatically
- **Don't paraphrase conflicts** - Quote their exact words
- **Don't ignore soft conflicts** - Even "difficult but possible" conflicts degrade outputs
- **Don't add requirements they didn't want** - Preserve original intent strictly
- **Don't use vague resolutions** - Be specific about how you resolved each conflict

## Reference for Understanding

- Mercury experiment (`docs/03-mercury-experiment.md`) shows the canonical example of averaged compromise behavior
- The 10 mechanisms (`docs/04-mechanisms.md`) explain WHY conflicts cause specific failure modes
- Research evidence (`docs/05-research-evidence.md`) provides empirical foundation from papers on superposition, knowledge conflicts, emergent misalignment, task sensitivity, and safety guardrail collapse