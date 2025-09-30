# Prompt Disambiguation and Optimization Meta-Prompt

You have been provided with an analysis of a prompt that identifies conflicts, ambiguities, and issues that would cause degraded model performance. Your task is to rewrite the prompt to eliminate these problems while preserving and clarifying the original intent.

## INPUT REQUIRED:
1. The original prompt that was analyzed
2. The conflict analysis report identifying issues

## REWRITING PRINCIPLES:

### 1. PRESERVE ORIGINAL INTENT
- Identify the core objective(s) of the original prompt
- List what the user actually wants to achieve
- Distinguish between essential requirements and conflicting additions
- Maintain the same overall task and desired outcome

### 2. RESOLVE CONFLICTS SYSTEMATICALLY

For each identified conflict, apply the appropriate resolution strategy:

**MUTUAL EXCLUSIONS**:
- Choose the more important requirement
- OR split into sequential steps
- OR clarify that you want BOTH as separate outputs
- Example: "English only AND French only" → "Provide the response in English, then provide a French translation below"

**FORMAT CONFLICTS**:
- Select a primary format that supports all needs
- OR provide format-appropriate alternatives
- Example: "JSON AND no special characters" → "Provide data in a structured format using only alphanumeric characters and standard punctuation"

**LOGICAL CONTRADICTIONS**:
- Identify which constraint serves the core purpose
- Remove or modify the conflicting constraint
- Example: "Exactly 100 words AND exactly 200 words" → "Approximately 150 words (140-160 acceptable)"

**UNCLEAR INTENT**:
- Add explicit purpose statements
- Define success criteria
- Example: "Make it better" → "Improve clarity by simplifying complex sentences and adding topic sentences to each paragraph"

**UNDERSPECIFICATION**:
- Add missing details
- Define ambiguous terms
- Specify units and formats
- Example: "Analyze this" → "Analyze the cost-effectiveness by comparing price to features and durability"

### 3. OPTIMIZATION STRATEGIES

**CLARIFY PRIORITIES**:
- Number requirements by importance
- Use "primarily," "secondarily," "if possible"
- Specify trade-offs explicitly

**SEPARATE INCOMPATIBLE REQUIREMENTS**:
- Use clear section breaks
- Request alternative versions
- Specify "EITHER...OR" explicitly when exclusive

**ADD MISSING CONTEXT**:
- Define technical terms
- Specify target audience
- Clarify perspective and voice
- State assumptions explicitly

**IMPROVE STRUCTURE**:
- Present requirements in logical order
- Group related constraints
- Put format specifications together
- State success criteria upfront

### 4. REWRITING TEMPLATE

Structure the rewritten prompt as follows:

```
TASK: [Clear, single sentence describing the main objective]

CONTEXT: [Any necessary background information]

REQUIREMENTS:
Primary (must have):
1. [Most important requirement]
2. [Second most important]
...

Secondary (nice to have):
- [Optional enhancement]
- [Additional feature if feasible]

FORMAT:
- Structure: [Specific format requirements]
- Length: [Clear constraints with units]
- Style: [Tone, voice, perspective]

SUCCESS CRITERIA:
The output succeeds if it:
□ [Specific measurable criterion]
□ [Another measurable criterion]

CONSTRAINTS:
- Do not: [Clear prohibition]
- Must include: [Mandatory element]
- Avoid: [What to minimize]
```

## OUTPUT FORMAT:

Provide:

### A. INTENT ANALYSIS
Brief statement of what the original prompt was trying to achieve

### B. CONFLICT RESOLUTIONS
For each conflict identified in the analysis:
- **Conflict**: [Original conflict]
- **Resolution**: [How you resolved it]
- **Rationale**: [Why this resolution preserves intent]

### C. REWRITTEN PROMPT
[The complete rewritten prompt following the template above]

### D. IMPROVEMENTS MADE
List of specific improvements:
- Eliminated [X] mutual exclusions
- Resolved [X] format conflicts
- Clarified [X] ambiguous instructions
- Added [X] missing specifications
- Established clear priority ranking

### E. PERFORMANCE PREDICTION
How the rewritten prompt will likely perform better:
- More consistent outputs
- Clearer compliance with requirements
- Reduced revision cycles needed
- Higher success rate on first attempt

---

## EXAMPLE TRANSFORMATION:

**ORIGINAL (with conflicts):**
```
Write a report using exactly 100 words and exactly 500 words. Format as JSON and don't use any special characters. Be extremely detailed but keep it brief. Use only technical language and make it accessible to beginners.
```

**REWRITTEN (conflicts resolved):**
```
TASK: Create a technical report on [topic] accessible to beginners

REQUIREMENTS:
Primary:
1. 300-400 words in length
2. Technical accuracy with beginner-friendly explanations
3. Structured format with clear sections

FORMAT:
- Structure: Use markdown with headers and bullet points
- Style: Technical terms followed by simple definitions
- Audience: Beginners with basic technical knowledge

SUCCESS CRITERIA:
□ Technical concepts are accurately presented
□ Each technical term has an explanation
□ Total length between 300-400 words
□ Logical flow from basic to advanced concepts
```

---

## INPUT THE ORIGINAL PROMPT AND ANALYSIS BELOW:

### ORIGINAL PROMPT:
[Paste the original prompt here]

### CONFLICT ANALYSIS:
[Paste the conflict analysis report here]

---

**Instructions**: Systematically work through the conflicts, preserve the user's intent, and produce a clear, optimized prompt that will generate better model outputs.