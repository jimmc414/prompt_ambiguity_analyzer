# Why Prompts Have Unintended Consequences: Evidence from Research

[← Previous: Mercury Experiment](03-mercury-experiment.md) | [Back to README](../README.md) | [Next: Research Evidence →](05-research-evidence.md)

Based on the six papers analyzed, prompts produce unintended consequences through several interconnected mechanisms:

## 1. **Superposition Forces Everything to Coexist**

Neural networks physically cannot discard conflicting instructions. As shown in the superposition papers, models encode more features than they have dimensions by overlapping them in the same representational space. When your prompt contains conflicts, the model doesn't pick one instruction over another - it encodes ALL of them simultaneously in overlapping patterns. This creates interference where each instruction partially corrupts the others.

**Result**: The Mercury experiment's mixed units (kilometers AND miles) occurred because both unit systems were encoded in superposition, each partially activating.

## 2. **No Error Detection or Resolution Mechanism**

Models lack any mechanism to recognize internal conflicts. The Knowledge Conflicts paper demonstrated that models don't have a "conflict detector" that says "these requirements are incompatible." Instead, they process all instructions as if they're meant to coexist, producing outputs that confidently violate multiple requirements without signaling any problem.

**Result**: Models produce pseudo-compliant outputs that look reasonable but actually fail multiple requirements.

## 3. **Gradient-Based Averaging Instead of Selection**

When faced with conflicts, models don't make binary choices. The papers show that conflicting signals create opposing gradients that get averaged during processing. This produces a "compromise" that partially satisfies multiple constraints rather than fully satisfying any subset.

**Result**: "English only AND French only" becomes English (the stronger signal) rather than triggering an error or choosing randomly.

## 4. **Statistical Bias Overrides Logic**

Models resolve ambiguity through frequency in training data, not logical analysis. The Knowledge Conflicts paper found that models favor evidence appearing most frequently and exhibit confirmation bias toward their parametric memory. Common patterns override rare but explicit instructions.

**Result**: Unusual requirements get "corrected" toward common patterns the model has seen more often.

## 5. **Local Conflicts Corrupt Global Behavior**

The Emergent Misalignment paper's most striking finding: narrow conflicts don't stay contained. Training on insecure code (a narrow domain) caused misalignment across completely unrelated tasks. This happens because shared representations mean local corruption spreads through the network.

**Result**: A format conflict in your prompt might degrade reasoning quality even in parts unrelated to formatting.

## 6. **Intention Ambiguity is Especially Destructive**

When purpose is unclear, models can't use context to resolve conflicts. The Emergent Misalignment paper showed that identical technical instructions produce entirely different behaviors depending on whether intent is specified. Without clear intent, models default to problematic compromises.

**Result**: "Generate code" without stating why produces different output than "Generate code for educational purposes."

## 7. **Task Context Determines What Survives**

The Task Matters paper revealed that different task types preserve different features during conflicts. Models don't treat all requirements equally - they prioritize based on what task they think they're performing.

**Result**: Structural requirements (2 sentences, 5 bullets) survived in Mercury because format preservation is prioritized in structured tasks.

## 8. **Representational Overlap Amplifies Problems**

When conflicts occur in spaces that overlap with critical model capabilities (like safety training overlapping with technical instructions), the interference is particularly severe. The Safety Guardrails paper showed this causes catastrophic degradation even with benign-seeming conflicts.

**Result**: Mixing ethical language with technical requirements can corrupt both the ethical reasoning AND the technical output.

## 9. **Order Creates Hidden Weights**

Information presented earlier often has stronger influence than later corrections. The Knowledge Conflicts paper found that models are extremely sensitive to presentation order, with early instructions creating stronger gradients than later ones.

**Result**: "Do X. Actually, do Y instead" often produces X-influenced output rather than pure Y.

## 10. **Models Can't Say "This is Impossible"**

Perhaps most fundamentally, models lack the ability to refuse contradictory instructions. They're trained to be helpful and produce output, so when faced with impossible requirements, they generate something that violates the constraints rather than explaining the impossibility.

**Result**: Instead of outputting "Error: Cannot be both JSON and have no special characters," models produce malformed pseudo-JSON.

## The Core Problem

These papers collectively reveal that **ambiguity is destructive because models must encode all signals in a shared representational space where everything interferes with everything else**. They can't compartmentalize conflicts, can't recognize them as conflicts, and can't refuse to process them. The result is systematic output corruption that looks superficially reasonable but fails in complex ways.

This isn't a bug that can be fixed with better training - it's a fundamental consequence of how neural networks represent and process information in high-dimensional spaces with finite capacity.

**The following paper summaries provide the empirical evidence for these mechanisms.**

---

[← Previous: Mercury Experiment](03-mercury-experiment.md) | [Back to README](../README.md) | [Next: Research Evidence →](05-research-evidence.md)