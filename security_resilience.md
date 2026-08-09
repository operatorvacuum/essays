# Residual Diversity

*A research seed on AI monoculture, unknown unknowns, and security resilience.*

## Hypothesis

AI resilience may depend not only on diversity in models, architectures,
or outputs, but on diversity in **what systems preserve as unexplained**.

Call this **residual diversity**.

When a system encounters an observation that does not fit its current model,
it has several possible responses:

1. assimilate it into the nearest known category,
2. dismiss it as noise or irrelevant,
3. flag it as anomalous but discard it,
4. preserve it as unresolved and investigate further.

The choice happens upstream of the final answer.

Two models can produce different outputs while sharing the same blind spot
if both discard the same anomalous evidence before it becomes an object of
investigation.

## Why this may matter for security

Future attacks will include behaviors that are novel relative to existing
benchmarks, threat models, and evaluations.

No benchmark can enumerate every future attack.

If AI systems become increasingly similar through shared training data,
benchmarks, tuning objectives, evaluation practices, and safety norms, they
may develop correlated ways of deciding what *doesn't matter*.

The resulting monoculture could therefore be deeper than correlated outputs:

> Models may share the same policy for collapsing the unexplained.

An attacker only needs to find structure inside that shared negative space.

Model diversity could bound the blast radius of surprise — but only if the
diversity exists at the layer where surprise is interpreted.

## Residual diversity

Let a **residual** mean an observation, prediction error, contradiction, or
anomaly not adequately explained by the system's current representation.

Residual diversity is variation across systems in which residuals they:

- dismiss,
- assimilate,
- preserve,
- prioritize,
- investigate.

This is different from ordinary ensemble diversity.

Architecture diversity asks whether systems are built differently.

Output diversity asks whether systems give different answers.

Residual diversity asks:

> **Do the systems notice and preserve different unexplained things?**

The distinction matters because diversity downstream cannot recover
information that every system discarded upstream.

## Not all surprise is useful

Maximizing residual retention would be a bad objective.

Random or intrinsically unpredictable observations can generate endless
prediction error without containing learnable structure — the classic
curiosity / "noisy TV" problem.

The target is therefore not maximum surprise.

A useful system needs some policy for estimating whether an unresolved
residual may contain **learnable or security-relevant structure**.

The resilience hypothesis is that these policies themselves should not
become a monoculture.

## A possible architecture

Instead of:

 anomaly

 ↓
 
 shared dismissal / normalization policy
 
 ↓
 
 answer

consider an ensemble with heterogeneous residual policies:

  observation
 
  ↓
 
  prediction / representation mismatch
 
  ↓
 
  residual
 
  ↓ 
 
  heterogeneous retention policies
 
  ↙ ↓ ↘
 
  dismiss    preserve    investigate
 
             ↓
 
  discriminating probe
 
             ↓
  
  environment / reality
 
             ↓
 
  update or discard

The unresolved state needs enough persistence that it cannot be erased
merely because the current ontology lacks a category for it.

This is **bounded epistemic persistence**, not permanent attachment to an
anomaly: evidence must still be able to kill the hypothesis.

## Security prediction

This suggests a testable prediction:

> Under sufficiently novel attacks, ensembles optimized for residual
> diversity should exhibit lower correlated miss rates than ensembles
> selected only for architecture, benchmark-performance, or output diversity.

The important dependent variable is not merely average model accuracy.

It is **correlated failure / blast radius**.

## Possible experiment

Construct several defensive ensembles:

- homogeneous models,
- architecture-diverse models,
- output-diverse models,
- models explicitly diversified in residual-retention / anomaly-investigation
 policies.

Expose them to attacks deliberately selected or generated outside the threat
distribution used during training and evaluation.

Measure:

- fraction of systems compromised,
- correlation of misses,
- whether any system preserves the first useful anomalous signal,
- time from anomalous signal to investigation,
- whether one system's preserved residual allows the ensemble to detect an
 attack missed by the others.

A particularly interesting case is one where every model initially lacks the
correct attack concept.

The question becomes not:

> "Which model already knows the answer?"

but:

> **"Does at least one model preserve the clue from which the answer can be
> discovered?"**

## Connection to open-endedness

This idea sits near several existing research areas:

- curiosity-driven learning,
- open-ended learning,
- unknown-unknown discovery,
- anomaly detection,
- adversarial transferability,
- ensemble diversity,
- AI monoculture / correlated failure.

The proposed bridge is:

 open-ended curiosity
 ↓
 preservation of unexplained residuals
 ↓
 heterogeneous investigation
 ↓
 independent discovery paths
 ↓
 reduced correlated blind spots
 ↓
 bounded blast radius under surprise

The claim is not that these component ideas are new.

The question is whether **diversity in residual preservation itself** is an
important and under-measured axis of AI security resilience.

## Open questions

- How should a "residual" be represented for large language models?
- Can residual diversity be measured independently of output diversity?
- Do shared benchmarks cause convergence in residual-dismissal policies?
- Which training pressures produce correlated negative space?
- How can residuals persist without producing noisy-TV behavior?
- Should systems exchange preserved residuals even when they disagree about
 their interpretation?
- Can adversaries specifically search for residuals that an entire model
 population systematically dismisses?
- Does optimizing residual diversity actually reduce transferability or
 merely move the shared blind spots elsewhere?

I don't know yet.

That's the point of the seed.