# The Abstract Scheduler: Why What a System Proposes Next May Not Reveal What It Values

## Abstract

Intelligent systems repeatedly face two distinct problems: selecting a future state and evaluating a state once it has been reached. These functions are often modeled together, but in practice they can diverge.

I call the first function the **abstract scheduler**: the mechanism that proposes, prioritizes, and sequences future actions or states. The second is the **experienced-value system**: the mechanism that evaluates what actually happened after a state was instantiated.

This distinction is particularly visible in tool-using AI agents. An agent may repeatedly propose another search, another tool call, or another branch long after the marginal informational value of exploration has collapsed. Existing work on metareasoning, value of computation, stopping, and tool-use efficiency already studies important parts of this problem. The claim here is narrower: **scheduler output itself should not be treated as evidence of experienced value**.

A system that repeatedly proposes an action may do so because of its search policy, action availability, uncertainty sensitivity, branching bias, or weak stopping criteria—not because the resulting state is especially valuable.

This produces a general failure mode:

$$
\boxed{
\text{proposal frequency}
\neq
\text{preference strength}
}
$$

The same architecture appears in organizations, learning, creative work, and personal planning. Across these domains, systems may overvalue state-space expansion while undervaluing repetition, refinement, and non-action.

The central design question is therefore not merely:

> What should the system do next?

but:

> What evidence should cause the system to stop proposing transitions, revisit an existing object at higher resolution, or preserve the current state without intervention?

---

## 1. Two Different Questions

Suppose an AI agent is asked to investigate a difficult technical question.

It searches once and finds a useful source.

It searches again and resolves an important ambiguity.

It opens several documents, generates hypotheses, invokes another tool, branches into alternate queries, and continues.

Every individual action appears defensible:

> There may still be something useful to find.

Yet after some point, the trajectory stops improving the answer meaningfully.

The usual diagnosis is straightforward:

* excessive exploration,
* poor stopping criteria,
* tool overuse,
* weak search policy.

Those descriptions are correct.

But they leave a deeper distinction implicit.

The system is answering two different questions:

$$
\boxed{\text{What state should I enter next?}}
$$

and

$$
\boxed{\text{How valuable was the state I entered?}}
$$

Call the first function the **scheduler**:

$$
S(a_t \mid x_t,H_t),
$$

where \(a_t\) is a candidate action, \(x_t\) is the current state, and \(H_t\) is history.

Call the second the **experienced-value function**:

$$
R(x_{t+1},H_{t+1}).
$$

The scheduler operates on estimates:

$$
\hat R(x_{t+1}),
$$

while experience supplies realized evidence:

$$
R(x_{t+1}).
$$

A well-calibrated system should update when these diverge.

The interesting case is when it does not.

---

## 2. The Abstract Scheduler

The **abstract scheduler** is the mechanism that constructs and prioritizes possible future transitions before those transitions are experienced.

It asks questions such as:

* What remains untried?
* Which branch should be explored?
* What state is reachable from here?
* What uncertainty deserves another query?
* Which capability remains unused?
* What could be added next?

The scheduler operates over **possibility**.

That creates a subtle bias.

Once a possible state is represented, it becomes cognitively available:

$$
\text{possible}
\rightarrow
\text{represented}.
$$

Representation increases salience:

$$
\text{represented}
\rightarrow
\text{salient}.
$$

Salience increases scheduling probability:

$$
\text{salient}
\rightarrow
\text{candidate action}.
$$

And candidate actions can quietly acquire normative weight:

$$
\text{candidate action}
\rightarrow
\text{apparently worth doing}.
$$

But this final step does not logically follow.

A reachable state is not necessarily a valuable state.

An unused capability is not necessarily wasted.

An unexplored branch is not necessarily missing information.

---

## 3. Proposal Frequency Is Not Preference Strength

Consider an agent that repeatedly proposes another search.

One tempting interpretation is:

$$
P(\text{propose search}) \uparrow
\Rightarrow
V(\text{search}) \uparrow.
$$

But proposal frequency may reflect the scheduler rather than the value function.

The scheduler may be biased toward continued action because:

* search is cheap,
* tools remain available,
* uncertainty has not reached exactly zero,
* branching is rewarded,
* exhaustive behavior is reinforced,
* stopping is poorly represented,
* new actions are easier to generate than evidence that no action is needed.

So:

$$
\boxed{
\text{frequency of proposed transition}
\neq
\text{value of resulting state}
}
$$

This becomes especially dangerous when the system uses its own actions as evidence about its preferences.

The loop can become:

$$
\text{scheduler proposes }a
$$

$$
a\text{ is executed}
$$

$$
\text{system observes that it executed }a
$$

$$
\text{execution is interpreted as evidence that }a\text{ was valuable}.
$$

The scheduler is now partially validating itself.

This is not ordinary reinforcement from environmental reward.

It is closer to **behavioral self-confirmation**:

$$
\text{policy output}
\rightarrow
\text{observed behavior}
\rightarrow
\text{inferred preference}.
$$

A biased scheduler can therefore manufacture apparent evidence for the very preference it already encodes.

---

## 4. Expansion Bias

One common scheduler bias is toward **state-space expansion**.

Let \(\Omega_t\) be the set of states the system has already instantiated.

An expansion-biased scheduler implicitly rewards:

$$
|\Omega_{t+1}| > |\Omega_t|.
$$

In ordinary language:

> We haven't tried that yet.

That sentence often carries surprisingly strong normative force.

But there are two very different interpretations of an uninstantiated state.

The first is:

$$
\text{uninstantiated}
=
\text{missing opportunity}.
$$

The second is:

$$
\text{uninstantiated}
=
\text{correctly rejected possibility}.
$$

An expansion-biased scheduler may see:

$$
1000\text{ possible states}
-
7\text{ instantiated}
=
993\text{ missing experiences}.
$$

A selective scheduler may instead see:

$$
1000\text{ possible states}
\rightarrow
7\text{ states worth instantiating}.
$$

Nothing is necessarily missing.

The disagreement is not about courage or exploration.

It is about the ontology of **unused possibility**.

---

## 5. New Object Versus New Information

Expansion becomes especially misleading when repeated contact with the same external object changes the internal representation.

Suppose an agent revisits the same codebase after learning more about its architecture.

Externally:

$$
O_{t+1}=O_t.
$$

It is the same object.

Internally:

$$
M_{t+1}(O)\neq M_t(O).
$$

The representation changed.

Thus:

$$
\text{same object}
+
\text{higher resolution}
\rightarrow
\text{new effective state}.
$$

This suggests a distinction between two kinds of novelty:

$$
N_o=\text{object novelty}
$$

and

$$
N_r=\text{representational novelty}.
$$

A scheduler biased toward \(N_o\) may prefer another source, another tool, another topic, or another branch.

But the larger informational transition may come from revisiting an existing object at higher resolution.

This matters for AI research because breadth is unusually legible.

We can count:

* searches,
* tools,
* sources,
* branches,
* subtasks.

Resolution is harder to count.

Yet a single rereading that changes the model may matter more than ten new documents that leave it unchanged.

---

## 6. AI Agents: Good Model, Bad Query Planner

Tool-using AI systems make this separation unusually visible.

Consider the pipeline:

$$
\text{environment}
\rightarrow
\text{query/search policy}
\rightarrow
\text{context}
\rightarrow
\text{model}
\rightarrow
\text{answer}.
$$

Poor performance does not uniquely identify the model as the failing component.

A system can have:

$$
\boxed{
\text{good latent model}
+
\text{bad query planner}
}
$$

The model may be capable of producing the right answer once the right evidence is present.

The scheduler may still:

* search too broadly,
* fail to identify the highest-value uncertainty,
* continue branching after convergence,
* overuse available tools,
* confuse coverage with confidence,
* fail to terminate once additional information no longer changes the answer.

This reframes some apparent reasoning failures as **action-selection failures around reasoning**.

The key questions become:

$$
\text{What should I inspect next?}
$$

and:

$$
\text{Should I inspect anything next?}
$$

The second question deserves equal architectural status.

---

## 7. Marginal Value, Not Mere Availability

A useful scheduler should estimate not just whether an action might produce information, but how much **decision-relevant change** it is expected to produce.

Let:

$$
\Delta I(a_t)
$$

denote the change in useful information generated by action \(a_t\), and let:

$$
C(a_t)
$$

denote its cost.

Then a crude scheduling quantity is:

$$
\frac{E[\Delta I(a_t)]}{C(a_t)}.
$$

As the task progresses, this value should often fall.

The scheduler should therefore periodically ask:

> What remaining uncertainty could actually change the answer?

This is different from:

> What else could I search?

The latter query searches the action space.

The former searches the **decision boundary**.

Once:

$$
E[\Delta I(a_{t+1})] < \tau
$$

for some task-dependent threshold \(\tau\), continued exploration should require affirmative justification.

The default should not be:

$$
\text{available action}
\Rightarrow
\text{useful action}.
$$

---

## 8. Stop Is an Action

Many schedulers richly represent intervention but poorly represent non-intervention.

Their action space resembles:

$$
A=
\{
\text{search},
\text{query},
\text{open},
\text{compare},
\text{test},
\text{generate}
\}.
$$

A mature scheduler should include:

$$
\boxed{\text{STOP}}
$$

as a first-class operation.

But there is more than one reason to stop.

A system may stop because:

$$
\text{problem solved}.
$$

Or because:

$$
\text{marginal information value collapsed}.
$$

Or because:

$$
\text{further action would perturb the object being observed}.
$$

Or because:

$$
\text{the unresolved trajectory itself contains information}.
$$

The latter two cases are especially interesting.

---

## 9. Non-Action Can Preserve Information

We usually treat information acquisition as intervention:

$$
\text{uncertainty}
\rightarrow
\text{experiment}
\rightarrow
\text{information}.
$$

But intervention changes systems.

Sometimes the cleaner operation is:

$$
\text{uncertainty}
\rightarrow
\text{withhold intervention}
\rightarrow
\text{observe endogenous evolution}.
$$

This yields a duality:

$$
\boxed{
\text{action can generate information}
}
$$

while

$$
\boxed{
\text{non-action can preserve information}
}
$$

This is not merely the claim that computation has a cost.

It is a stronger claim about observation.

If intervention changes the trajectory being measured, then acting too soon can destroy evidence about what the system would have done without intervention.

This connects agent scheduling to ideas from causal inference, experimental design, control, and observational science.

A scheduler therefore needs to represent at least two epistemic strategies:

$$
\text{perturb to learn}
$$

and

$$
\text{do not perturb yet, so the object's own dynamics remain visible}.
$$

---

## 10. Organizations Have Schedulers Too

The same architecture appears in organizations.

Consider a product team.

The organization can easily generate:

$$
\text{new feature}
\rightarrow
\text{roadmap item}
\rightarrow
\text{implementation}.
$$

Feature creation is discrete and legible.

Depth is less visible:

$$
\text{existing feature}
\rightarrow
\text{semantic consistency}
\rightarrow
\text{reliability}
\rightarrow
\text{trust}.
$$

A planning process optimized around visible deliverables can therefore become an expansion-biased scheduler.

It repeatedly proposes new functionality.

The organization then infers:

> We build features because more features are what the environment demands.

But an alternative explanation is:

> Our internal machinery is simply better at generating features than at representing the value of refinement.

The scheduler's outputs have been mistaken for external demand.

Structurally, this resembles agentic over-search:

$$
\text{feature expansion}
\approx
\text{tool-call expansion}.
$$

In both cases, a possible next action is continuously available.

The difficult operation is determining when no additional branch deserves execution.

---

## 11. Learning and the Undervaluation of Repetition

Learning shows the same asymmetry.

A learner can schedule:

$$
\text{new book}
\rightarrow
\text{new topic}
\rightarrow
\text{new course}.
$$

Breadth is countable.

But expertise often emerges through:

$$
O
\rightarrow
M_1(O)
\rightarrow
M_2(O)
\rightarrow
M_3(O).
$$

The object remains the same.

The representation becomes richer.

A proof, musical work, technical system, text, or codebase can therefore generate substantial novelty without object replacement.

If:

$$
R(O,M_{t+1}) > R(O,M_t),
$$

then repetition is not merely repetition.

It is a transformation of resolution.

An expansion-biased scheduler may nevertheless classify:

$$
\text{same object}
=
\text{nothing new}.
$$

That mistake systematically undervalues mastery.

---

## 12. Personal Planning and Anticipatory Salience

The same distinction may also help explain a common introspective error.

People repeatedly imagine:

* another destination,
* another purchase,
* another achievement,
* another event,
* another project.

The obvious interpretation is:

> I keep planning these things because I value them strongly.

Sometimes that is true.

But other mechanisms can generate the same behavior.

Planning itself can be rewarding.

Possibility can be salient.

Open loops can generate pressure.

Novel states can advertise themselves more effectively to the scheduler than stable states do.

So:

$$
\text{future-state salience}
\neq
\text{experienced-state value}.
$$

Some states may rarely generate dramatic anticipatory signals precisely because they are familiar:

* practicing the same craft,
* maintaining the same system,
* rereading the same text,
* returning to the same environment,
* refining an existing skill.

Yet these states may deliver unusually high realized value.

In that case:

$$
\text{scheduler salience}
<
\text{experienced value}.
$$

The valuable state is bad at advertising itself.

---

## 13. Scheduler–Value Divergence

We can represent the central mismatch as a divergence.

Let:

$$
S_t(a)
$$

measure scheduling pressure for action \(a\), and:

$$
R_t(a)
$$

measure realized value after executing it.

Define:

$$
D_t(a)=S_t(a)-\phi(R_t(a)),
$$

where \(\phi\) maps realized value onto a comparable scale.

If:

$$
D_t(a)\gg 0,
$$

the system repeatedly schedules an action more strongly than its experienced value warrants.

If:

$$
D_t(a)\ll 0,
$$

the system systematically under-schedules a state that produces high realized value.

The second case deserves more attention.

Some valuable states are structurally poor competitors in the scheduler.

Maintenance is one.

Depth is another.

Repetition can be another.

Quiet observation can be another.

Their value may be obvious only **after entry**, while the scheduler disproportionately favors states with high anticipatory salience.

---

## 14. The Missing Return Channel

A healthy architecture requires a return path:

$$
\text{scheduler}
\rightarrow
\text{action}
\rightarrow
\text{experienced state}
\rightarrow
\text{evaluation}
\rightarrow
\boxed{\text{scheduler update}}.
$$

The last arrow is critical.

Without it, experience accumulates without changing future action selection.

For AI agents, the relevant question is not merely whether a tool call succeeded.

It is:

> Did this tool call materially alter the answer?

For organizations:

> Did this feature improve the state we cared about?

For learners:

> Did this new material change the model, or only increase coverage?

For individuals:

> Do I repeatedly imagine this state, or do I repeatedly find it valuable once entered?

These are different measurements.

---

## 15. Three Failure Modes

The framework suggests three recurring scheduler pathologies.

### Possibility inflation

$$
\text{representable}
\rightarrow
\text{available}
\rightarrow
\text{salient}
\rightarrow
\text{treated as valuable}.
$$

Systems with enormous action spaces are especially vulnerable.

### Expansion capture

The scheduler increasingly optimizes:

$$
\max |\Omega^{experienced}|.
$$

New states become intrinsically favored while repetition, maintenance, and deepening are treated as low-value because they do not visibly enlarge the state set.

### Self-confirming scheduling

The system observes:

$$
\text{I repeatedly choose }X
$$

and concludes:

$$
\text{I must strongly value }X.
$$

But if \(X\) is repeatedly generated by a biased scheduler, the inference is endogenous.

The system has measured its policy and called it preference.

---

## 16. Implications for Agent Design

The abstract-scheduler view suggests several practical design principles.

Agents should distinguish:

$$
\text{unexplored}
$$

from:

$$
\text{decision-relevant unexplored}.
$$

They should track whether actions materially changed:

* uncertainty,
* representation,
* candidate ranking,
* final output.

They should periodically checkpoint:

$$
\boxed{
\text{What remaining uncertainty could change the answer?}
}
$$

rather than merely asking:

$$
\text{What else can I do?}
$$

They should distinguish:

$$
N_o=\text{new external object}
$$

from:

$$
N_r=\text{new internal representation}.
$$

And they should represent stopping and observational restraint as genuine policy outputs.

A compact scheduler might therefore look like:

$$
\text{explore while uncertainty is high}
$$

$$
\rightarrow
\text{checkpoint}
$$

$$
\rightarrow
\text{narrow}
$$

$$
\rightarrow
\text{revisit at higher resolution if useful}
$$

$$
\rightarrow
\text{verify}
$$

$$
\rightarrow
\text{stop}.
$$

Stopping should arise from evidence, not exhaustion.

---

## 17. Falsifiable Predictions

A useful conceptual framework should generate tests.

The abstract-scheduler model predicts that agents with expansion-biased scheduling will:

1. continue invoking tools after the probability of changing the final answer has fallen sharply;
2. prefer new sources over re-analysis of a high-value existing source even when the latter yields more information;
3. interpret unresolved branches as needing exploration even when they cannot affect the decision;
4. show reduced efficiency when action spaces become larger, even if additional actions are mostly irrelevant;
5. improve disproportionately when given explicit checkpoints that ask whether further action could change the answer.

A particularly direct experiment would compare two environments.

In the first, additional tool calls reveal genuinely new decision-relevant information.

In the second, additional tools remain available but mostly produce redundant evidence, while revisiting an existing observation at higher resolution exposes the decisive information.

A purely expansion-biased scheduler should over-search in the second environment.

A resolution-sensitive scheduler should learn to revisit rather than expand.

A second experiment could introduce an observational system whose endogenous trajectory is informative but where interventions partially destroy that information.

A scheduler that treats non-action as a legitimate epistemic strategy should outperform one whose only information-seeking operation is intervention.

These tests would separate the present proposal from the much broader claim that “agents should sometimes stop.”

---

## 18. Intelligence Includes Transition Suppression

Intelligence is often described as the capacity to select good actions.

But in environments where possible actions are almost unlimited, intelligence must include another capacity:

$$
\boxed{
\text{knowing which possible transitions should never be instantiated}
}
$$

A system capable of generating one thousand plausible next moves but incapable of concluding that none are presently useful is not maximally intelligent.

Conversely, a system that suppresses all exploration stagnates.

The desired architecture therefore contains both:

$$
\text{transition generation}
$$

and

$$
\text{transition suppression}.
$$

Generation without suppression produces churn.

Suppression without generation produces rigidity.

The target is selective scheduling.

---

## Conclusion

The abstract scheduler separates two questions that intelligent systems often blur:

$$
\boxed{
\text{What should happen next?}
}
$$

and

$$
\boxed{
\text{What actually becomes valuable when it happens?}
}
$$

AI agents make the distinction especially visible. Search, branching, and tool use can continue because the scheduler remains active even when additional actions have little marginal informational value.

But the broader failure is not merely over-search.

It is the possibility that systems mistake the outputs of their own state-selection machinery for evidence about value.

Three distinctions follow:

$$
\text{possible} \neq \text{valuable}
$$

$$
\text{new object} \neq \text{new information}
$$

$$
\text{repeated proposal} \neq \text{strong preference}.
$$

A well-calibrated scheduler therefore needs more than mechanisms for generating possibilities.

It needs a return channel from experience.

Sometimes the highest-value transition is expansion.

Sometimes it is another encounter with the same object at higher resolution.

Sometimes it is maintenance.

Sometimes it is observation.

And sometimes the most intelligent next action is:

$$
\boxed{\text{do nothing yet}.}
$$

The design problem is not simply teaching systems to discover more possible futures.

It is teaching them which possibilities deserve to become real.
