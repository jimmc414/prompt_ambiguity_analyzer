# Prompt Ambiguity Analyzer

A research-driven framework for detecting and resolving conflicts in prompts that cause systematic output corruption in frontier language models.

## Overview

This project provides meta-prompts and workflows to identify prompt ambiguities that trigger:
- **Superposition interference** - Models encoding conflicting instructions in overlapping representational space
- **Emergent misalignment** - Local ambiguities corrupting broader model behavior
- **Gradient confusion** - Contradictory signals producing averaged/compromised outputs

Based on 6 peer-reviewed papers analyzing how neural networks handle conflicting information.

## Project Structure

```
prompt_ambiguity_analyzer/
├── README.md                    # This file
├── WORKFLOW.md                  # Instructions for AI assistants
├── prompts/
│   ├── 1-detect-conflicts.md   # Standalone detection meta-prompt
│   └── 2-resolve-conflicts.md  # Standalone disambiguation meta-prompt
├── docs/
│   ├── 01-objective.md          # Project goals and scope
│   ├── 02-hypothesis.md         # Core hypothesis
│   ├── 03-mercury-experiment.md # Demonstrating the problem
│   ├── 04-mechanisms.md         # 10 failure mechanisms
│   ├── 05-research-evidence.md  # Paper summaries and citations
│   └── 07-implementation.md     # Technical appendix
└── 06-meta-prompts.md          # Integrated meta-prompt documentation
```

## Quick Start

### For Manual Use (Copy-Paste to LLM)

1. **Analyze your prompt for conflicts:**
   - Copy the contents of `prompts/1-detect-conflicts.md`
   - Replace `{Insert your prompt here}` with your actual prompt
   - Submit to any frontier LLM

2. **Get a rewritten version:**
   - Copy the contents of `prompts/2-resolve-conflicts.md`
   - Insert your original prompt and the conflict analysis
   - Submit to get an optimized version

### For Use with Claude Code (Automated)

1. Open this project in Claude Code
2. Say: "Analyze this prompt: [your prompt]"
3. Claude Code will automatically:
   - Run the detection meta-prompt
   - Generate a conflict analysis report
   - Run the disambiguation meta-prompt
   - Provide the rewritten prompt

See `WORKFLOW.md` for detailed AI assistant instructions.

## How It Works

### Detection (Prompt 1)
Analyzes prompts across 15 categories:
- Mutual exclusions ("only X" + "only Y")
- Format conflicts (JSON + "no special characters")
- Logical contradictions (exact counts that conflict)
- Domain mixing (technical + casual requirements)
- Unclear intent, underspecification, scope creep
- And 8 more categories...

**Output**: Structured report with:
- Specific conflicts quoted from your prompt
- Severity ratings (HARD/SOFT)
- Impact predictions
- Overall risk assessment

### Disambiguation (Prompt 2)
Takes the conflict analysis and:
- Preserves your original intent
- Resolves conflicts systematically
- Adds missing specifications
- Structures requirements by priority
- Provides success criteria

**Output**: Rewritten prompt optimized for model performance

## Research Foundation

Based on 6 ArXiv papers:
1. **Knowledge Conflicts for LLMs** - How models handle contradictions
2. **Mechanistic Interpretability** - Superposition and feature interference
3. **Emergent Misalignment** - How local conflicts corrupt global behavior
4. **Toy Models of Superposition** - Why models can't discard conflicts
5. **Task Matters** - How task type affects conflict resolution
6. **Safety Guardrails Collapse** - Representational overlap problems

Full citations and summaries in `docs/05-research-evidence.md`

## Documentation

- **`docs/01-objective.md`** - Project goals, scope, and limitations
- **`docs/02-hypothesis.md`** - Core hypothesis about superposition
- **`docs/03-mercury-experiment.md`** - Real example of conflict corruption
- **`docs/04-mechanisms.md`** - 10 mechanisms causing unintended consequences
- **`docs/05-research-evidence.md`** - Research papers and key findings
- **`docs/07-implementation.md`** - Technical implementation insights
- **`06-meta-prompts.md`** - Complete meta-prompt documentation

## Example Use Case

**Problematic Prompt:**
```
Write a report using exactly 100 words and exactly 500 words.
Format as JSON and don't use any special characters.
Be extremely detailed but keep it brief.
```

**After Detection:**
- ❌ HARD conflict: "exactly 100 words" vs "exactly 500 words"
- ❌ HARD conflict: "JSON format" vs "no special characters"
- ❌ SOFT conflict: "extremely detailed" vs "keep it brief"

**After Disambiguation:**
```
TASK: Create a technical report on [topic]

REQUIREMENTS:
Primary:
1. 300-400 words in length
2. Structured format with clear sections
3. Balance detail with conciseness

FORMAT:
- Structure: Use markdown with headers
- Style: Technical but accessible

SUCCESS CRITERIA:
□ Length between 300-400 words
□ Logical section organization
□ Key points clearly explained
```

## Key Features

✅ Research-backed detection categories
✅ Specific conflict quotation and location
✅ Severity ratings and impact predictions
✅ Intent-preserving disambiguation
✅ Structured output templates
✅ Automated workflow for AI assistants

## Limitations

As a prompt-based analysis tool:
- Cannot measure true representational similarity
- Uses surface patterns as proxies for deeper conflicts
- Cannot detect all interference that emerges during computation
- Provides heuristic risk assessment, not guarantees

See `docs/01-objective.md` for full discussion of limitations and value proposition.

## Contributing

This is a research documentation project. If you find errors or want to suggest improvements based on additional research, please open an issue.

## License

Research documentation - use freely with attribution.