# Learning Log

## What This Log Is
- This section exists to capture what I learn by building real systems, not just reading about them.
- I use hands-on projects to pressure-test ideas, surface constraints, and understand trade-offs.
- The goal is steady growth through curiosity and iteration, grounded in practical outcomes.

## Cross-Project Learnings

### 1. Treat AI Output as a Hypothesis, Not an Answer
- AI outputs are useful when paired with evidence and traceability.
- Confidence signals matter; they determine when to automate versus review.
- Evaluation keeps models honest when inputs are short or ambiguous.
- Human validation loops reduce the cost of incorrect summaries or labels.
- I treat outputs as probabilistic signals, not decisions.
- This mindset keeps systems reliable while still benefiting from automation.

### 2. Evaluation Shapes System Design More Than Model Choice
- Accuracy can hide class imbalance and edge-case failures.
- Macro F1 provides a more reliable view across labels.
- Confusion matrices expose where adjacent classes collide.
- Qualitative error analysis drives prompt, data, and UX adjustments.
- Evaluation results influence system boundaries and fallback paths.
- Good metrics are actionable, not just reportable.

### 3. Push-Based Systems Simplify Both Performance and Understanding
- Streaming shifts complexity from polling loops to explicit event flow.
- A single source can serve many subscribers through fan-out.
- Backpressure and disconnects need to be modeled early.
- Push-based updates reduce redundant work and jitter.
- Clear stream lifecycles make realtime behavior easier to reason about.
- This design reduces both cognitive load and system load.

### 4. Resource Lifecycle Management Is a First-Class Concern
- Long-lived resources need explicit creation, reuse, and teardown rules.
- Browser automation taught me to treat lifecycle as a design constraint.
- Ownership boundaries prevent leaks and cascading failures.
- Cleanup paths should be tested as rigorously as success paths.
- Early lifecycle decisions define cost ceilings and reliability.
- Getting this right speeds up iteration and stabilization.

### 5. Simplicity Enables Faster Learning and Higher Impact
- I avoid premature abstraction until real constraints appear.
- Minimal paths make debugging faster and design decisions clearer.
- Smaller surfaces make instrumentation and rollback practical.
- I iterate toward impact by hardening what already works.
- Simplicity keeps room for experimentation without losing control.
- It also makes outcomes easier to explain and defend.

## How These Learnings Influence My Future Work
- I prioritize systems that make AI useful, safe, and observable.
- I choose evaluation methods that expose failure modes early.
- I favor architectures that are event-driven and debuggable.
- I treat lifecycle ownership as part of the system contract.
- I look for projects that translate technical insight into real-world impact.