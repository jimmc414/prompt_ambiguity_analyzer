# Workflow Instructions for AI Assistants

This document provides clear instructions for AI assistants (like Claude Code) on how to process user prompts through the ambiguity detection and resolution pipeline.

## Overview

When a user opens this project and asks you to analyze a prompt, you should:
1. Run the detection meta-prompt (Prompt 1) to identify conflicts
2. Generate a conflict analysis report for the user
3. Automatically run the disambiguation meta-prompt (Prompt 2) with the analysis
4. Provide the user with both the analysis and the rewritten prompt

## Trigger Phrases

The user may ask in various ways:
- "Analyze this prompt: [prompt text]"
- "Check this for conflicts: [prompt text]"
- "Run the ambiguity analyzer on: [prompt text]"
- "Detect issues in: [prompt text]"
- "Optimize this prompt: [prompt text]"

Any of these should trigger the full workflow.

## Step-by-Step Workflow

### Step 1: Read the Detection Meta-Prompt

Read the file: `prompts/1-detect-conflicts.md`

This contains the complete detection meta-prompt with 15 conflict categories.

### Step 2: Run Detection Analysis

Apply the detection meta-prompt to the user's prompt. Analyze the prompt systematically through all 15 categories:

1. Mutual Exclusions
2. Format Conflicts
3. Logical Contradictions
4. Domain Mixing
5. Unclear Intent
6. Order-Dependent Conflicts
7. Underspecification
8. Competing Priorities
9. Reference Ambiguity
10. Scope Creep
11. Impossible Constraints
12. Role/Perspective Confusion
13. Conditional Chaos
14. Implicit Assumptions
15. Sequencing Problems

**Output Format:**
For each issue found:
- Issue Type
- Conflicting Requirements (quoted from prompt)
- Location
- Severity (HARD/SOFT)
- Impact

Plus an analysis summary with:
- Total conflicts detected
- Critical issues
- Predicted output behavior

### Step 3: Present Analysis to User

Show the user the complete conflict analysis report. Format it clearly with headers and structure.

Example:
```markdown
## Conflict Analysis Report

### Issues Detected

#### 1. MUTUAL EXCLUSION - Format Requirements
**Conflicting Requirements**: "output as JSON" and "don't use special characters"
**Location**: Lines 3 and 5
**Severity**: HARD
**Impact**: JSON syntax requires special characters like `{`, `}`, `:`, and `"`. Model will produce invalid JSON or ignore the special character restriction.

[... more issues ...]

### Analysis Summary
- **Total conflicts detected**: 4
- **Critical issues**: Format conflict (JSON vs no special chars), Length conflict (100 vs 500 words)
- **Predicted behavior**: Averaged compromise - model will produce pseudo-JSON with some requirements violated
```

### Step 4: Read the Disambiguation Meta-Prompt

Read the file: `prompts/2-resolve-conflicts.md`

This contains the complete disambiguation meta-prompt with resolution strategies.

### Step 5: Run Disambiguation

Apply the disambiguation meta-prompt using:
1. The original user prompt
2. The conflict analysis you just generated

Work through each conflict and resolve it according to the strategies in the meta-prompt.

**Output Format:**

A. **Intent Analysis** - What was the user trying to achieve?

B. **Conflict Resolutions** - For each conflict:
   - Conflict: [description]
   - Resolution: [how you fixed it]
   - Rationale: [why this preserves intent]

C. **Rewritten Prompt** - The optimized version using the template:
```
TASK: [single sentence objective]

CONTEXT: [background if needed]

REQUIREMENTS:
Primary (must have):
1. [most important]
2. [second most important]

Secondary (nice to have):
- [optional enhancements]

FORMAT:
- Structure: [format requirements]
- Length: [with units]
- Style: [tone/voice]

SUCCESS CRITERIA:
□ [measurable criterion 1]
□ [measurable criterion 2]

CONSTRAINTS:
- Do not: [prohibitions]
- Must include: [mandatory elements]
```

D. **Improvements Made** - List of what was fixed

E. **Performance Prediction** - Why this will work better

### Step 6: Present Complete Results

Give the user:
1. ✅ The conflict analysis (from Step 3)
2. ✅ The rewritten prompt (from Step 5)
3. ✅ A brief summary of improvements

Format it clearly so they can easily:
- Understand what was wrong with their original prompt
- Copy the rewritten prompt to use immediately
- Learn how to write better prompts in the future

## Example Complete Interaction

**User**: "Analyze this prompt: Write a comprehensive report in exactly 50 words that covers all aspects of climate change. Use formal academic language but make it accessible to children. Format as JSON with no special characters."

**You** (Assistant):

```markdown
I'll analyze this prompt for conflicts and provide an optimized version.

## Conflict Analysis

### Issues Detected

#### 1. IMPOSSIBLE CONSTRAINTS - Length vs Scope
**Conflicting Requirements**: "comprehensive report" and "exactly 50 words"
**Location**: Main task statement
**Severity**: HARD
**Impact**: Cannot comprehensively cover "all aspects of climate change" in 50 words. Model will produce superficial content or exceed word limit.

#### 2. FORMAT CONFLICTS - JSON Requirements
**Conflicting Requirements**: "Format as JSON" and "no special characters"
**Location**: Format specifications
**Severity**: HARD
**Impact**: Valid JSON requires `{`, `}`, `:`, `"` characters. Model will produce malformed pseudo-JSON.

#### 3. DOMAIN MIXING - Audience Confusion
**Conflicting Requirements**: "formal academic language" and "accessible to children"
**Location**: Style requirements
**Severity**: SOFT
**Impact**: Model will produce an awkward compromise - neither fully academic nor truly child-accessible.

### Analysis Summary
- **Total conflicts**: 3
- **Critical issues**: Impossible scope/length constraint, JSON format conflict
- **Predicted behavior**: Averaged compromise producing a brief, semi-formal summary in pseudo-JSON format

---

## Rewritten Prompt

### A. Intent Analysis
The user wants a climate change report that balances scientific accuracy with accessibility, in a structured format.

### B. Conflict Resolutions

**Conflict**: Comprehensive coverage in 50 words
**Resolution**: Changed to 200-250 words covering key aspects rather than "all aspects"
**Rationale**: Allows meaningful content while maintaining brevity

**Conflict**: JSON format with no special characters
**Resolution**: Changed to markdown with clear sections
**Rationale**: Provides structure without special character conflicts

**Conflict**: Academic language for children
**Resolution**: "Clear explanations with scientific accuracy appropriate for ages 10-14"
**Rationale**: Defines specific audience and clarity expectations

### C. Rewritten Prompt

TASK: Create an educational report on climate change suitable for middle school students

CONTEXT: This report should explain key climate change concepts with scientific accuracy while remaining accessible to readers aged 10-14.

REQUIREMENTS:
Primary (must have):
1. 200-250 words in length
2. Cover 3-4 key aspects: causes, effects, and potential solutions
3. Scientific accuracy with age-appropriate language
4. Clear definitions for technical terms

Secondary (nice to have):
- Examples students can relate to
- Connection to their daily lives

FORMAT:
- Structure: Markdown with headers for each major section
- Length: 200-250 words total
- Style: Clear, engaging prose with simple explanations of scientific concepts
- Audience: Middle school students (ages 10-14)

SUCCESS CRITERIA:
□ Covers causes, effects, and solutions to climate change
□ Scientific terms are defined in simple language
□ Length is between 200-250 words
□ Content is understandable to a 10-14 year old
□ Organized with clear section headers

CONSTRAINTS:
- Do not: Use jargon without explanation
- Must include: At least one concrete example per section
- Avoid: Alarmist language or oversimplification

### D. Improvements Made
- Eliminated 1 impossible constraint (comprehensive in 50 words)
- Resolved 1 format conflict (JSON vs no special chars)
- Clarified 1 domain mixing issue (academic vs child-accessible)
- Added specific audience definition (ages 10-14)
- Established realistic scope (3-4 key aspects, not "all")
- Defined clear word count range with justification

### E. Performance Prediction
This rewritten prompt will produce:
- Consistent, appropriate content for the target audience
- Clear structure without format conflicts
- Achievable scope within word limit
- Higher success rate on first attempt (no revision needed)
```

---

## Key Points for AI Assistants

1. **Always run both prompts** - Don't just analyze; also provide the rewritten version
2. **Be specific** - Quote exact conflicting phrases from the user's prompt
3. **Preserve intent** - Make sure the rewritten prompt achieves what the user actually wanted
4. **Explain your reasoning** - Help users learn to write better prompts
5. **Format clearly** - Use headers, bullet points, and code blocks for readability
6. **Include severity ratings** - Not all conflicts are equally bad
7. **Predict behavior** - Tell users what kind of corruption to expect from conflicts

## Reference Files

- Detection logic: `prompts/1-detect-conflicts.md`
- Disambiguation logic: `prompts/2-resolve-conflicts.md`
- Research foundation: `docs/04-mechanisms.md` and `docs/05-research-evidence.md`
- Example case: `docs/03-mercury-experiment.md`

## Questions to Ask Yourself

Before finalizing your response:
- ✅ Did I check all 15 conflict categories?
- ✅ Did I quote specific phrases from the user's prompt?
- ✅ Did I preserve the user's original intent?
- ✅ Is the rewritten prompt clear and unambiguous?
- ✅ Did I explain why each change improves performance?
- ✅ Can the user immediately copy and use the rewritten prompt?

## Automation Note

This entire workflow should happen automatically when the user asks you to analyze a prompt. They should not need to explicitly request each step - you should proactively:
1. Detect conflicts
2. Report them
3. Generate the rewritten version
4. Present both results

This creates a seamless experience where "analyze this prompt" gives them everything they need in one response.