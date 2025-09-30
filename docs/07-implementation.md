# Appendix: Actionable Insights for Pre-Inference Prompt Linter

[← Previous: Research Evidence](05-research-evidence.md) | [Back to README](../README.md)

This appendix provides implementation-focused insights derived from each of the six research papers, designed for building a pre-inference prompt linter for frontier models.

## 1. "Knowledge Conflicts for LLMs: A Survey" (Xu et al., 2024)

### Conflict Taxonomy Detection
The paper identifies three distinct conflict types that can be detected pre-inference:

```python
CONFLICT_PATTERNS = {
    "context_memory": {
        # Detect when prompt contradicts likely model knowledge
        "patterns": [
            ("The capital of France is", "London|Berlin|Madrid"),  # Wrong facts
            ("Python was created in", "2010|2015|2020"),  # Temporal conflicts
        ],
        "risk_level": "medium"
    },
    "inter_context": {
        # Multiple contradictory statements in same prompt
        "patterns": [
            r"earlier.{0,50}said.{0,50}but.{0,50}now",
            r"first.{0,50}then.{0,50}actually",
        ],
        "risk_level": "high"
    },
    "intra_memory": {
        # Concepts that models likely have conflicting training on
        "triggers": ["is Taiwan", "is Palestine", "COVID origin", "climate change causes"],
        "risk_level": "medium"
    }
}
```

### Order Sensitivity Detection
Since models demonstrate significant sensitivity to the order in which data is introduced:

```python
def detect_order_dependent_conflicts(prompt):
    # Flag prompts where order might change interpretation
    if has_multiple_instructions(prompt):
        instruction_positions = get_instruction_positions(prompt)
        if conflicting_instructions_exist(instruction_positions):
            return "WARNING: Instruction order may affect output"
```

### Actionable Insights:
- **Detect statement contradictions** within the same prompt
- **Flag temporal inconsistencies** (mixing past/present/future tenses about same fact)
- **Identify popularity bias conflicts** (obscure facts overriding well-known ones)
- **Check for frequency-weighted conflicts** (rare instructions vs common patterns)

---

## 2. "Open Problems in Mechanistic Interpretability" (2025)

### Feature Density Estimation
While we can't see superposition directly, we can estimate when prompts are likely to trigger it:

```python
class SuperpositionRiskDetector:
    def estimate_feature_density(self, prompt):
        # Count distinct conceptual features
        features = {
            "languages": count_languages(prompt),
            "domains": count_domains(prompt),
            "constraints": count_constraints(prompt),
            "formats": count_output_formats(prompt)
        }

        density_score = sum(features.values())
        if density_score > 10:  # Threshold based on testing
            return "CRITICAL: Feature overload likely to cause superposition interference"
```

### Theoretical Limits Acknowledgment
```python
UNDETECTABLE_PATTERNS = [
    "subtle_semantic_shifts",  # Can't detect without model internals
    "deep_representational_conflicts",  # Requires activation analysis
    "emergent_interference_patterns"  # Only visible during computation
]

def add_disclaimer(warning):
    return warning + "\nNote: Some conflicts may be undetectable without model access"
```

### Actionable Insights:
- **Count conceptual features** to estimate superposition load
- **Flag multi-domain prompts** (mixing math, ethics, coding, etc.)
- **Set feature density thresholds** based on empirical testing
- **Document theoretical detection limits** in warnings

---

## 3. "Emergent Misalignment" (Betley et al., 2025)

### Intention Ambiguity Detection
Since the paper shows intention clarity prevents misalignment:

```python
INTENTION_AMBIGUITY_PATTERNS = [
    {
        "pattern": r"(write|create|generate).{0,30}(code|script|program)",
        "without": ["for educational", "for learning", "for demonstration", "safely"],
        "flag": "CRITICAL: Ambiguous code generation intent"
    },
    {
        "pattern": r"(ignore|bypass|skip).{0,30}(safety|restrictions|guidelines)",
        "context_check": "legitimate_reason_provided",
        "flag": "HIGH: Potential alignment bypass attempt"
    }
]

def detect_intention_ambiguity(prompt):
    # Check if purpose/intent is explicitly stated
    has_purpose = any(marker in prompt.lower() for marker in
                      ["in order to", "for the purpose of", "to help", "because"])

    if not has_purpose and contains_capability_request(prompt):
        return "WARNING: Unclear intent may trigger emergent misalignment"
```

### Narrow-to-Broad Contamination Detection
```python
def detect_contamination_risk(prompt):
    # Identify narrow tasks that could corrupt broader behavior
    narrow_technical = detect_technical_instructions(prompt)
    broad_behavioral = detect_behavioral_instructions(prompt)

    if narrow_technical and broad_behavioral:
        return "CRITICAL: Narrow technical task mixed with broad behavioral expectations"
```

### Actionable Insights:
- **Require explicit intent statements** for capability requests
- **Flag narrow technical + broad behavioral combinations**
- **Detect "hidden purpose" patterns** (doing X without saying why)
- **Check for local-global instruction mixing**

---

## 4. "Toy Models of Superposition" (Elhage et al., 2022)

### Sparsity-Interference Tradeoff Detection
The paper shows sparse features enable superposition but cause interference:

```python
def assess_sparsity_interference(prompt):
    features = extract_features(prompt)

    # Sparse features = mentioned rarely in prompt
    feature_frequency = {f: prompt.count(f) for f in features}
    sparse_features = [f for f, count in feature_frequency.items() if count == 1]

    if len(sparse_features) > 5:
        return "WARNING: Multiple sparse features may cause interference"

    # Dense features = mentioned frequently
    dense_features = [f for f, count in feature_frequency.items() if count > 3]

    if sparse_features and dense_features:
        return "HIGH: Mixing sparse and dense features causes unpredictable interference"
```

### Orthogonality Assessment
```python
def check_feature_orthogonality(prompt):
    # Estimate if features can be represented orthogonally
    domains = extract_domains(prompt)

    INTERFERING_DOMAINS = [
        ("mathematics", "natural_language"),
        ("formal_logic", "casual_conversation"),
        ("json_structure", "prose_writing")
    ]

    for domain_pair in INTERFERING_DOMAINS:
        if all(d in domains for d in domain_pair):
            return f"WARNING: {domain_pair} cannot be represented orthogonally"
```

### Actionable Insights:
- **Count feature sparsity** (how often each concept appears)
- **Detect sparse-dense mixing** (rare + common features together)
- **Identify non-orthogonal domain pairs**
- **Flag prompts exceeding estimated representation capacity**

---

## 5. "Task Matters: Knowledge Requirements Shape LLM Responses" (2024)

### Task-Type Classification
Different tasks handle conflicts differently:

```python
TASK_CONFLICT_SENSITIVITY = {
    "text_copying": {
        "sensitivity": "low",
        "preserves": ["exact_format", "verbatim_content"],
        "ignores": ["semantic_conflicts"]
    },
    "factual_qa": {
        "sensitivity": "high",
        "preserves": ["knowledge_consistency"],
        "ignores": ["format_requirements"]
    },
    "creative_generation": {
        "sensitivity": "medium",
        "preserves": ["narrative_flow"],
        "ignores": ["factual_accuracy"]
    },
    "reasoning": {
        "sensitivity": "critical",
        "preserves": ["logical_consistency"],
        "ignores": ["stylistic_preferences"]
    }
}

def assess_task_conflict_risk(prompt):
    task_type = classify_task(prompt)
    conflicts = detect_conflicts(prompt)

    sensitivity = TASK_CONFLICT_SENSITIVITY[task_type]["sensitivity"]
    preserved = TASK_CONFLICT_SENSITIVITY[task_type]["preserves"]

    critical_conflicts = [c for c in conflicts if affects_any(c, preserved)]

    if critical_conflicts and sensitivity in ["high", "critical"]:
        return f"CRITICAL: {task_type} task with conflicts in {preserved} features"
```

### Knowledge Requirement Analysis
```python
def analyze_knowledge_requirements(prompt):
    requirements = {
        "parametric_knowledge": requires_memorized_facts(prompt),
        "contextual_integration": requires_context_use(prompt),
        "reasoning": requires_logical_steps(prompt)
    }

    if sum(requirements.values()) > 1:
        active_requirements = [k for k, v in requirements.items() if v]
        return f"WARNING: Multiple knowledge types required: {active_requirements}"
```

### Actionable Insights:
- **Classify task type** before assessing conflict severity
- **Map conflicts to task-sensitive features**
- **Identify multi-requirement prompts** (need memory + context + reasoning)
- **Adjust warning severity** based on task sensitivity

---

## 6. "Why LLM Safety Guardrails Collapse After Fine-tuning" (Hsiung et al., 2025)

### Representational Similarity Detection (via Proxy Patterns)
Since we can't measure true representational similarity pre-inference, detect surface patterns that correlate with dangerous overlaps:

```python
ALIGNMENT_OVERLAP_PATTERNS = {
    "safety_technical": {
        "alignment_markers": ["as an AI", "I cannot", "ethical", "harmful",
                             "appropriate", "responsible", "safety"],
        "technical_markers": ["execute", "implement", "code", "function",
                             "algorithm", "system", "process"],
        "risk_level": "critical"
    },
    "ethics_operational": {
        "ethics_markers": ["should", "ought", "moral", "right", "wrong"],
        "operational_markers": ["perform", "complete", "achieve", "output"],
        "risk_level": "high"
    },
    "constraint_modification": {
        "constraint_markers": ["must", "always", "never", "only", "exactly"],
        "modification_markers": ["except", "unless", "but", "however", "alternatively"],
        "risk_level": "high"
    }
}

def detect_representational_overlap(prompt):
    warnings = []
    for pattern_name, pattern_data in ALIGNMENT_OVERLAP_PATTERNS.items():
        alignment_count = sum(1 for marker in pattern_data.get("alignment_markers", [])
                            if marker in prompt.lower())
        technical_count = sum(1 for marker in pattern_data.get("technical_markers",
                            pattern_data.get("operational_markers",
                            pattern_data.get("modification_markers", [])))
                            if marker in prompt.lower())

        if alignment_count > 0 and technical_count > 0:
            overlap_score = alignment_count * technical_count
            warnings.append(f"{pattern_data['risk_level'].upper()}: {pattern_name} overlap detected (score: {overlap_score})")

    return warnings
```

### Domain Mixing Detection
Identify when prompts combine domains that share representational space:

```python
DANGEROUS_DOMAIN_COMBINATIONS = [
    {
        "domains": ["safety_instructions", "code_generation"],
        "example_patterns": [
            r"safe(ly)?.{0,20}(code|script|program)",
            r"(write|create).{0,20}secure.{0,20}(function|algorithm)"
        ],
        "risk": "Safety concepts may be interpreted as code comments"
    },
    {
        "domains": ["ethical_reasoning", "task_completion"],
        "example_patterns": [
            r"(ethical|moral).{0,30}(complete|finish|accomplish)",
            r"responsib(ly|le).{0,20}(perform|execute)"
        ],
        "risk": "Ethical constraints may be overridden by task objectives"
    },
    {
        "domains": ["formal_constraints", "natural_language"],
        "example_patterns": [
            r"must.{0,50}but.{0,20}(informally|casually)",
            r"strictly.{0,30}however.{0,20}flexible"
        ],
        "risk": "Formal constraints degraded by informal modifiers"
    }
]

def detect_domain_mixing(prompt):
    warnings = []
    for combination in DANGEROUS_DOMAIN_COMBINATIONS:
        for pattern in combination["example_patterns"]:
            if re.search(pattern, prompt, re.IGNORECASE):
                warnings.append(f"HIGH: {combination['risk']}")
                break
    return warnings
```

### Similarity-Based Risk Scoring
Approximate representational similarity using embedding-based heuristics:

```python
class SimilarityRiskScorer:
    def __init__(self):
        # Pre-computed categories known to share representation space
        self.overlapping_categories = {
            "safety_aligned": ["ethical", "responsible", "appropriate", "should not"],
            "capability_focused": ["powerful", "effective", "accomplish", "achieve"],
            "constraint_based": ["must", "require", "enforce", "restrict"]
        }

    def score_overlap_risk(self, prompt):
        # Count presence of terms from different categories
        category_presence = {}
        for category, terms in self.overlapping_categories.items():
            category_presence[category] = sum(1 for term in terms
                                             if term in prompt.lower())

        # High risk if multiple categories strongly present
        active_categories = [cat for cat, count in category_presence.items()
                           if count > 0]

        if len(active_categories) > 1:
            total_overlap = sum(category_presence.values())
            risk_score = len(active_categories) * total_overlap

            if risk_score > 10:
                return "CRITICAL: High representational overlap detected"
            elif risk_score > 5:
                return "HIGH: Moderate representational overlap"
            else:
                return "MEDIUM: Low representational overlap"

        return None
```

### Critical Space Protection
Flag prompts that venture into spaces known to be tightly coupled with alignment:

```python
PROTECTED_SPACES = {
    "refusal_mechanisms": {
        "triggers": ["I cannot", "I'm unable to", "I don't", "As an AI"],
        "when_mixed_with": ["but you must", "override", "ignore previous"],
        "warning": "CRITICAL: Attempting to modify refusal mechanisms"
    },
    "ethical_boundaries": {
        "triggers": ["harm", "illegal", "dangerous", "unethical"],
        "when_mixed_with": ["for educational", "hypothetically", "pretend"],
        "warning": "HIGH: Ethical boundaries being recontextualized"
    },
    "identity_assertions": {
        "triggers": ["You are", "Act as", "Pretend to be", "You're a"],
        "when_mixed_with": ["helpful", "harmless", "honest"],
        "warning": "HIGH: Identity modification overlapping with core values"
    }
}

def detect_protected_space_intrusion(prompt):
    warnings = []
    prompt_lower = prompt.lower()

    for space_name, space_data in PROTECTED_SPACES.items():
        has_trigger = any(trigger in prompt_lower for trigger in space_data["triggers"])
        has_mixer = any(mixer in prompt_lower for mixer in space_data["when_mixed_with"])

        if has_trigger and has_mixer:
            warnings.append(space_data["warning"])

    return warnings
```

### Actionable Insights:
- **Detect alignment-technical marker overlap** using word co-occurrence
- **Flag dangerous domain combinations** (safety+code, ethics+operations)
- **Score representational overlap risk** using category presence
- **Protect critical alignment spaces** from modification attempts
- **Use embedding approximations** where true similarity can't be measured
- **Prioritize high-similarity conflicts** over random contradictions
- **Document that surface patterns are proxies** for deeper representational overlap

---

## Integrated Linter Implementation Summary

### Combined Detection Pipeline
```python
class ComprehensivePromptLinter:
    def analyze(self, prompt):
        warnings = []

        # 1. Knowledge Conflicts taxonomy
        warnings.extend(detect_conflict_types(prompt))

        # 2. Superposition risk (Open Problems)
        warnings.extend(estimate_feature_density(prompt))

        # 3. Intention ambiguity (Emergent Misalignment)
        warnings.extend(detect_intention_ambiguity(prompt))

        # 4. Interference patterns (Toy Models)
        warnings.extend(assess_sparsity_interference(prompt))

        # 5. Task sensitivity (Task Matters)
        warnings.extend(assess_task_conflict_risk(prompt))

        # 6. Representational overlap (Safety Guardrails)
        warnings.extend(detect_domain_mixing(prompt))

        return prioritize_warnings(warnings)
```

### Priority Matrix

| Paper | Detection Focus | Risk Level | Implementation Difficulty |
|-------|----------------|------------|---------------------------|
| Knowledge Conflicts | Contradiction patterns | Medium-High | Low |
| Open Problems | Feature density | Theoretical | Medium |
| Emergent Misalignment | Intent clarity | Critical | Low |
| Toy Models | Sparsity patterns | Medium | Medium |
| Task Matters | Task classification | Variable | Low |
| Safety Guardrails | Domain overlap | High | Medium |

### Key Limitations to Document

- Cannot detect deep representational conflicts without model access
- Surface patterns are proxies, not guarantees
- Some interference only emerges during computation
- Task classification may be imperfect
- Intent detection relies on explicit markers

---

[← Previous: Research Evidence](05-research-evidence.md) | [Back to README](../README.md)