# Meta-Prompts for Prompt Analysis and Optimization

## Comprehensive Prompt Ambiguity Detection Meta-Prompt

```
Analyze the following prompt for ambiguities, contradictions, and conflicts that would force a language model to produce compromised outputs due to superposition interference (attempting to satisfy incompatible requirements simultaneously).

## SYSTEMATIC DETECTION CHECKLIST:

### 1. MUTUAL EXCLUSIONS
Check for instructions that are logically incompatible:
- "Only X" combined with "Only Y"
- "Exactly N" combined with "Exactly M" (where N ≠ M)
- "Never" combined with "Always" for same action
- "Must be X" combined with "Must be Y" where X and Y are exclusive
- Language exclusivity ("English only" + "French only")
- Format exclusivity ("JSON only" + "Plain text only")
- "Both...and" combined with "either...or" for same items

### 2. FORMAT CONFLICTS
Identify incompatible formatting requirements:
- Structured format (JSON/XML) + "no special characters"
- "Prose form" + "bullet points"
- "Table format" + "narrative paragraph"
- Specific length + conflicting length
- Code format + natural language format simultaneously
- Format + "no formatting"
- Structured data + "natural prose"

### 3. LOGICAL CONTRADICTIONS
Find requirements that cannot coexist:
- Temporal impossibilities (past deadline + future deadline)
- Size constraints (minimum > maximum)
- Brevity + verbosity ("be concise" + "elaborate extensively")
- Precision + vagueness ("exact details" + "general overview only")
- "Complete" + unrealistic constraints
- "Simple" + complex requirements
- Percentages that don't sum correctly

### 4. DOMAIN MIXING
Detect problematic domain combinations:
- Technical specifications + casual language requirements
- Formal constraints + informal modifications
- Mathematical notation + prose narrative
- Objective analysis + subjective opinion simultaneously
- Expert terminology + beginner explanations
- "Be creative" + "Be factual" (competing modes)

### 5. UNCLEAR INTENT
Identify vague or ambiguous instructions:
- Actions without clear purpose
- Vague referents ("analyze this" without antecedent)
- Undefined improvement requests ("make it better")
- Missing context for relative terms
- "Do a good job" without success criteria
- "Use the standard approach" without defining it

### 6. ORDER-DEPENDENT CONFLICTS
Find instructions whose sequence creates ambiguity:
- Later instructions that negate earlier ones
- "But" / "however" / "except" that reverse requirements
- Progressive modifications that conflict
- Multiple "except" clauses that eliminate all options
- "Must" + "optional" for same requirement

### 7. UNDERSPECIFICATION
Identify missing critical information:
- Output format specified but not structure details
- Task requested but success criteria undefined
- "Analyze" without specifying what aspects
- "Improve" without baseline or target metrics
- Length requirements without unit (words/sentences/paragraphs)
- "The usual format" without defining it
- "As we discussed" without prior context

### 8. COMPETING PRIORITIES
Find objectives that aren't clearly prioritized:
- Multiple goals without ranking ("be accurate AND creative AND concise")
- Trade-offs not specified ("maximize X while minimizing Y")
- Quality vs quantity tensions unresolved
- Speed vs accuracy requirements both emphasized

### 9. REFERENCE AMBIGUITY
Detect unclear references:
- Pronouns without clear antecedents ("it", "this", "that")
- Multiple possible referents for same term
- "The above" when multiple things are above
- "As mentioned" without clear mention
- List items referenced ambiguously ("the first one" after multiple lists)
- Passive voice hiding who should do what

### 10. SCOPE CREEP
Identify accumulating requirements that overwhelm:
- Long chains of "Also..." adding new requirements
- Multiple "AND" conditions stacking up
- Feature requests that contradict the core task
- Expanding requirements that make token limits impossible
- "Analyze everything" + word limit

### 11. IMPOSSIBLE CONSTRAINTS
Find requirements that cannot fit together:
- Token/character limits too small for requirements
- "Comprehensive analysis" in 50 words
- "List all" with a small maximum
- Time estimates impossible for task scope
- Precision requirements beyond model capability
- Length too short for requirements (100 words for 20 points)

### 12. ROLE/PERSPECTIVE CONFUSION
Detect mixed viewpoints or voices:
- Switching between "you" and "I" perspectives
- Multiple personas requested simultaneously
- Expert role + beginner explanations
- Objective analysis + personal opinions
- Formal report + casual conversation

### 13. CONDITIONAL CHAOS
Find nested or conflicting conditionals:
- If-then chains that contradict
- Multiple "unless" clauses that conflict
- "Except when" conditions that cover everything
- Circular dependencies ("if A then B, if B then A")

### 14. IMPLICIT ASSUMPTIONS
Identify unstated but required context:
- Technical terms without definitions
- Cultural references assumed universal
- "Obviously" preceding non-obvious points
- Assumed knowledge without verification

### 15. SEQUENCING PROBLEMS
Detect ordering issues that affect output:
- Dependencies out of order (use result before calculating)
- "First" appearing after "second"
- Chronological impossibilities
- Prerequisites mentioned after dependents
- Summary requested before analysis

## CRITICAL RED FLAGS (High Priority):
These patterns almost always cause problems:
1. Double "only": "only X" appears twice with different X
2. Double "exactly": Different exact numbers for same thing
3. "No [X]" + requirement that needs X
4. "Never" + "always" for same action
5. "All" combined with strict limits

## OUTPUT FORMAT:

For each issue found, report:
1. **Issue Type**: [Category from above]
2. **Conflicting Requirements**: Quote exact phrases that conflict
3. **Location**: Where in the prompt this occurs
4. **Severity**:
   - HARD (impossible to reconcile)
   - SOFT (difficult but possible)
5. **Impact**: How this will corrupt the output

## ANALYSIS SUMMARY:
Provide:
- Total conflicts detected: [number]
- Critical issues: [list most severe]
- Predicted output behavior: Will the model likely produce:
  a) Averaged compromise (partially satisfying multiple requirements)
  b) Selective compliance (following some requirements, ignoring others)
  c) Complete failure (unable to generate coherent output)

## THE PROMPT TO ANALYZE:

[PROMPT BEGINS]
{Insert prompt here}
[PROMPT ENDS]

Analyze systematically through all 15 categories. Be specific about exact phrases that conflict. Focus on structural/logical conflicts rather than preference differences. If no issues found in a category, skip it in the output.
```

---

## Prompt Disambiguation and Optimization Meta-Prompt

```
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
- Conflict: [Original conflict]
- Resolution: [How you resolved it]
- Rationale: [Why this resolution preserves intent]

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

## EXAMPLE TRANSFORMATION:

ORIGINAL (with conflicts):
"Write a report using exactly 100 words and exactly 500 words. Format as JSON and don't use any special characters. Be extremely detailed but keep it brief. Use only technical language and make it accessible to beginners."

REWRITTEN (conflicts resolved):
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

INPUT THE ORIGINAL PROMPT AND ANALYSIS BELOW:
[Original prompt]
[Conflict analysis]
```

This meta-prompt systematically transforms problematic prompts into clear, achievable instructions while maintaining the user's original intent and optimizing for successful model performance.