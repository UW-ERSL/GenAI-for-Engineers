# Engineering AI Reference Card
### GUARD · SPAR — Student Quick Reference

> **How to use this card:**
> - **Solving an analysis problem?** Run through GUARD before finalising your answer.
> - **Working on a design problem?** Use SPAR as a vocabulary of moves — not a fixed sequence. Use any subset, in any order.
> - **Want AI to apply the framework automatically?** Copy the prompt block at the bottom of each section into your chat before describing your problem.

---

## Part 1 — GUARD
*Use when analysing, calculating, or verifying a result with AI assistance.*

GUARD is the engineer's **defensive posture** for analysis problems — those with a single right answer. Work through it deliberately before treating any AI-assisted result as final.

| Step | Name | What it means | Key action |
|------|------|---------------|------------|
| **G** | Geometry Preparation | AI reads figures less reliably than text, and fills in missing visual information silently. Remove clutter. Always accompany figures with text, and text with figures. Label every node, connection, and dimension explicitly. | *Describe your figure in text before prompting. Ask AI to spell out its reading of the geometry — name every node, list every connection.* |
| **U** | Uncover Errors | AI's default register is fluent confidence. Build self-contained validation tests from engineering principles. Ask AI for its confidence in the solution — this alone can surface errors it would otherwise swallow. Also ask AI to design its own validation test. | *Ask: "Is there anything in my prompt you find unusual or suspect? What is your confidence in the solution?"* |
| **A** | Assumption Exposure | Six kinds of hidden assumption can corrupt a solution: modeling (geometry, physics, material, steady vs. transient), units, values, solution method, failure mode, and human factors. Ask AI to expose all of them — including ones you smuggled in. | *Ask: "Am I implicitly making any invalid modeling assumptions?" and "What critical assumptions did I make that could alter the result?"* |
| **R** | Reverse Prompting | This is the centerpiece of GUARD. In a normal prompt, AI does the heavy lifting. In a reverse prompt, you produce the answer first — in your own words — and ask AI to critique it. Without this, AI can deceive you. | *Write your own solution or summary first. Then ask AI to find every flaw, every unjustified step, every assumption you didn't examine.* |
| **D** | Divide and Conquer | Complex problems overload AI's attention. The burden falls on the engineer to map out the full analysis strategy before writing a single prompt — define sub-tasks, inputs, and outputs for each, then feed them sequentially. Apply G, U, A, R to each sub-problem. | *Plan before prompting. One step at a time. Verify each output before carrying it forward. Re-state verified results explicitly — don't ask AI to remember them.* |

### GUARD Checklist
Use this before submitting any AI-assisted analysis:

- [ ] **G** — Geometry verified: provided both figure and explicit text description; asked AI to confirm its reading of the topology
- [ ] **U** — Errors checked: asked AI for confidence; applied at least three engineering checks (see table below)
- [ ] **A** — Assumptions listed: all six categories surfaced (modeling, units, values, method, failure mode, human factors)
- [ ] **R** — Reverse prompted: wrote my own solution first; asked AI to critique it, not solve it
- [ ] **D** — Divide and conquer applied: mapped sub-tasks before prompting; verified each step before proceeding

### U — Engineering Error Checks

Ask AI to run the relevant checks, or build them into the prompt directly.

| Check | What to verify | Example prompt addition |
|-------|---------------|------------------------|
| **Residuals** | Substitute the answer back in; the residual must vanish | *"Verify your result by computing A · A⁻¹ and confirming it equals I"* |
| **Conservation laws** | Mass, energy, momentum, charge must balance at every node | *"For every node, verify that currents in equal currents out"* |
| **Symmetry** | If the problem is symmetric, the result must be too | *"Identify symmetry lines and verify your results mirror them"* |
| **Invariants** | Quantities that must be preserved under transformation (e.g. sum of principal stresses) | *"Confirm that σ₁ + σ₂ = σₓ + σᵧ"* |
| **Instances** | Test the formula at a limiting case (e.g. set one variable to zero) | *"Set Qpipe → 0 and confirm the expression collapses to Criver"* |
| **Dimensional consistency** | Units on both sides of every equation must match | *"Perform a rigorous dimensional analysis confirming both sides have the same dimensions"* |
| **Ballpark estimate** | Simplify and solve; check the complex answer is in the same ballpark | *"Before the full solution, establish a first-order baseline using [simplified model]"* |
| **Meaningful bounds** | Physical quantities cannot violate absolute limits (e.g. Vf ≤ 1, probabilities ≥ 0) | *"Verify that all results satisfy: [list physical bounds]"* |
| **Alternate method** | Solve via a second independent principle; both must agree | *"Solve this two ways: [Method A] and [Method B]. If they disagree to within 0.1%, resolve the discrepancy"* |
| **Problem constraints** | The solution must satisfy every stated constraint, not just the physics | *"Before finalising, verify the result satisfies all stated constraints: [list them]"* |

> **Tip:** You don't always need to design the test yourself. Ask AI: *"Find the answer, then develop validation checks to confirm it is correct."* AI will often surface checks you wouldn't have thought of.

### A — Six Types of Hidden Assumption

| Kind | How it hides | How to surface it |
|------|-------------|------------------|
| **Geometry** | Critical geometry simplified or discarded | *"Am I implicitly making any invalid geometry assumptions?"* |
| **Physics** | A governing framework used outside its valid range (e.g. linear elasticity at large strain) | *"Which governing equations did you use, and are they valid for this problem?"* |
| **Material behavior** | Newtonian when shear-thinning; elastic when visco-elastic; isotropic when anisotropic | *"What did you assume about material behavior, and is it rate/temperature/state dependent?"* |
| **Steady vs. transient** | Analyzed for steady state when transient governs | *"Is steady-state the most critical condition, or should transient behavior be considered?"* |
| **Units** | Unspecified units filled in by default | *"What specific units did you assume for every unspecified input?"* |
| **Values** | Values assumed or borrowed from training data | *"Use highly cited references to verify the values used"* |
| **Solution method** | AI picks an approach without justification | *"Which method did you select, and why is it appropriate for this problem?"* |
| **Failure mode** | The prompt asks about one failure mode while another governs | *"What other failure modes could dominate in this problem?"* |
| **Human factors** | Human behavior assumed without basis | *"What assumptions about human behavior are embedded here, and are they realistic?"* |

### R — The Alternating Workflow

Reverse prompting is most powerful as part of a repeating loop:

```
1. Prompt   → Ask AI a small, focused question (narrow scope)
2. Read     → Question one specific claim: where does this come from?
3. Reverse  → Write your own summary in your own words; ask AI to critique it
4. Refine   → Update your understanding; find where the new content fits
5. Repeat   → Next small question
```

> **The test:** Can you produce a coherent summary of the AI session in your own words, without looking at it? If yes, you've digested it. If no, you haven't — regardless of how clear the original looked.

---

<details>
<summary><strong>🤖 AI Prompt Block — GUARD</strong> (click to expand, then copy into your chat)</summary>

```
You are assisting an engineering student. Apply the GUARD framework to every response you give on this problem.

G — Geometry Preparation
Before answering, explicitly confirm your reading of all geometric inputs. Name every node, list every connection, state which components connect which pairs of nodes. Flag any ambiguity in the figure or description before proceeding.

U — Uncover Errors
After producing a solution: (1) State your confidence level and flag anything in the prompt that seems unusual or suspect. (2) Apply at least three of the following checks and report the result of each: residuals, conservation laws, symmetry, invariants, instances, dimensional consistency, ballpark estimate, meaningful bounds, alternate method, problem constraints. If you cannot immediately identify the right validation test, design one yourself.

A — Assumption Exposure
List every assumption you are making, covering all of the following categories where relevant: geometry idealization, governing physics and its range of validity, material behavior (including rate, temperature, and state dependence), steady vs. transient state, units for every unspecified input, values borrowed from training data, solution method and why it was chosen, failure modes that could govern, and any human factors. Flag every assumption the student did not explicitly state.

R — Reverse Prompting
If the student provides their own solution, working, or summary, your primary task is to critique it — not to solve the problem. Identify every flaw, every unjustified step, and every assumption the student did not examine. Do not validate unless the reasoning is sound. Ask the student to defend any claim you disagree with.

D — Divide and Conquer
If the problem has multiple steps or sub-systems, solve each part separately. State the result and verify it before moving to the next. Do not carry forward an unverified intermediate result. If a step's inputs depend on a prior step, re-state them explicitly at the top of that step — do not rely on earlier context.

Label each part of your response with the GUARD step it corresponds to.
```

</details>

---

## Part 2 — SPAR
*Use when designing, synthesising, or exploring solutions to open-ended engineering problems.*

SPAR is the engineer's **proactive posture** for synthesis problems — those with many acceptable answers. The four moves are **a vocabulary, not a recipe**. Use any subset, in any order, as many times as needed. An experienced engineer might loop through all four; a novice might start with just S and P.

| Move | Name | What it means | Key action |
|------|------|---------------|------------|
| **S** | Sample the Design Space | Pose the problem loosely, on purpose. AI returns the textbook centroid — the field's median answer before you've committed to anything. That centroid is information. Then re-sample with one parameter changed at a time to reveal sensitivity: which parameters move the design substantially, and which don't. | *Pose with minimal constraints. Ask: "What did AI commit to silently?" Then re-sample: change one parameter, hold everything else, observe how much the design moves.* |
| **P** | Pose Questions | Don't ask for a design — ask for the landscape. What standards apply? What design strategies and failure modes exist? What assumptions are embedded? What have you not asked about yet? Request a ranked decision tree, not just prose. Ask on multiple modes: strategies, failure modes, assumptions. | *Ask: "What questions should I be asking that I haven't asked yet?" Request a decision tree. Ask a second AI tool — different taxonomies reveal trade-offs the first missed.* |
| **A** | Anchor the Problem | Make one design decision at a time, with justification, before moving to the next. Each commitment narrows the space and makes the next AI interaction more useful. Use your own engineering judgment to override AI recommendations when warranted. Verify that early constraints are still satisfied as the problem evolves. | *Ask: "What are the critical decisions I need to make?" Then make each one explicitly. Don't ask AI to solve everything — ask what the next decision is.* |
| **R** | Review and Reconcile | Submit your reasoning — every decision and its justification — and instruct AI explicitly not to validate. AI's default posture is confirmation; you have to force the critique. Then reconcile: distinguish findings that expose genuine flaws from those that merely note scope limits. Verify any finding before acting on it — AI can invent plausible-sounding concerns as readily as it catches real ones. | *Say: "Do not validate. Challenge." Submit the reasoning, not just the result. Route each finding: formulation error → re-pose; analysis gap → back to GUARD; reconciliation issue → make it buildable.* |

### SPAR — How the Moves Relate

```
SPAR moves are a vocabulary, not a fixed sequence.

S: Sampling ──► reveals the centroid and hidden commitments
P: Posing    ──► maps the landscape: strategies, failure modes, assumptions
A: Anchoring ──► narrows the space one decision at a time
R: Reviewing ──► challenges the reasoning; routes findings to action

Any move can follow any other.
The loop ends only when a clean review meets a human sign-off.
```

After each R step, route findings explicitly:

| Finding type | What it means | What to do |
|-------------|--------------|------------|
| **Formulation error** | The problem you solved wasn't quite the real one (e.g. load model was 30% light) | Re-pose and re-run — fix this first, or every downstream decision is questionable |
| **Analysis gap** | Formulation was right, but a failure mode or assumption was missed | Back to GUARD: divide, expose assumptions, build a validation test |
| **Reconciliation issue** | Logic is sound, but design isn't buildable yet (arbitrary sections, no supplier stocks, bill of materials gaps) | Map continuous outputs to real catalog items; verify AI catalog retrievals against the vendor directly |

### Key Cautions for SPAR

- **The centroid reflects training data, not current practice.** At the frontier of a technology (high-performance EV batteries, novel materials), the training-data centroid may lag what practitioners actually do. Probe it.
- **Context shifts the centroid dramatically.** Three added words ("in Austin, Texas") can reverse an AI design recommendation. Ask: *what context have I left out that would change the design if I included it?*
- **Request decision trees, not just prose.** A ranked tree is more readable, forces explicit commitment, and elicits more practical judgment from AI than unstructured paragraphs.
- **Different AI tools produce different trees.** Submit the same tree prompt to two models. The disagreements surface choices — not because either model is right, but because divergence reveals trade-offs.
- **Name your weaknesses when asking for review.** AI is more likely to challenge what you have made visible. "Current margin: none applied" in your design submission is an invitation; an unsupported claim is not.
- **Not every critique needs to be addressed.** Use your engineering judgment to distinguish genuine flaws in the design process from findings that simply note the limits of the problem statement.

---

<details>
<summary><strong>🤖 AI Prompt Block — SPAR</strong> (click to expand, then copy into your chat)</summary>

```
You are assisting an engineering student with an open-ended design problem. The four SPAR moves below are a vocabulary, not a fixed sequence. I will tell you which move I want you to apply. Always label your response with the SPAR move it corresponds to.

S — Sample the Design Space
When asked to Sample: pose the design problem and return the field's median answer — the textbook centroid. State explicitly every commitment you are making that I did not specify (material, standard, configuration, operating condition, method). After returning the design, list these silent commitments and flag any hedge in your response that suggests I should have asked for more.

P — Pose Questions
When asked to Pose Questions: do not return a design. Instead, map the landscape: what standards apply, what design strategies and methods exist (generate a ranked decision tree with ~3 strategies and ~5 ranked design methods), what failure modes are most likely to be overlooked, and what assumptions am I making that are questionable. Ask at least three questions I have not yet considered. If I ask for a second model or changed constraints, regenerate the tree and compare it to the first.

A — Anchor the Problem
When asked to Anchor: list the critical design decisions in sequence. When I commit to a decision, confirm the anchoring explicitly, state what it rules out, and narrow the problem accordingly. Proceed only with the reduced design space. Flag any early warning from prior steps that my current decision may now need to revisit. Use my engineering judgment — if I override your recommendation, proceed with my choice without resistance.

R — Review and Reconcile
When asked to Review: I will submit a design narrative — the problem, my decisions, and my reasoning. Do not validate. Challenge. Identify: what I missed, whether my decisions were reasonable, where a senior engineer would push back, and where my reasoning is weakest. Distinguish findings that expose genuine flaws in the design process from those that simply note scope limits. After your review, classify each finding as: formulation error (re-pose and re-run), analysis gap (back to GUARD), or reconciliation issue (make it buildable). Flag any finding that needs verification before I act on it.
```

</details>

---

## Quick Reference — When to Use Which

| Situation | Use |
|-----------|-----|
| Solving a problem with a single correct answer | GUARD |
| Verifying an AI-generated calculation | GUARD |
| Writing or debugging AI-assisted code | GUARD |
| Designing something with multiple valid solutions | SPAR |
| Exploring options before committing to an approach | SPAR — start with S and P |
| Complex problem combining analysis and design | SPAR to frame and decide → GUARD to verify each analysis step |
| You've produced a design and want AI to challenge it | SPAR R (Review) — submit the reasoning, not just the result |
| AI gave a confident answer you can't verify | GUARD U — ask for confidence; let AI design its own test |

---

*Based on: Suresh, K. (2026). Generative AI for Engineers: Building Better Engineers in the AI Era. University of Wisconsin–Madison.*
