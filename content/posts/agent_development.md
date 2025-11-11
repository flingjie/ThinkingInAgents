+++
date = '2025-11-11T17:38:20+08:00'
draft = true
title = 'Agent development — Think in patterns, not frameworks'
+++

## 1. Why “off-the-shelf frameworks” are starting to fail

A framework is a tool for imposing order. It helps you set boundaries amid messy requirements, makes collaboration predictable, and lets you reproduce results.

Whether it’s a business framework (OKR) or a technical framework (React, LangChain), its value is that it makes experience portable and complexity manageable.

But frameworks assume a stable problem space and well-defined goals. The moment your system operates in a high-velocity, high-uncertainty environment, that advantage falls apart:

- abstractions stop being sufficient
- underlying assumptions break down
- engineers get pulled into API/usage details instead of system logic

The result: the code runs, but the system doesn’t _grow_.

![this is a image](/images/frameworks_fail.png "frameworks_fail")

Frameworks focus on implementation paths; patterns focus on design principles.
A framework-oriented developer asks “which `Agent.method()` should I call?”; a pattern-oriented developer asks “do I need a single agent or many agents? Do we need memory? How should feedback be handled?”

Frameworks get you to production; patterns let the system evolve.

---

## 2. Characteristics of Agent systems

Agent systems are more complex than traditional software:

- state is generated dynamically
- goals are often vague and shifting
- reasoning is probabilistic rather than deterministic
- execution is multi-modal (APIs, tools, side-effects)

That means we can’t rely only on imperative code or static orchestration. To build systems that adapt and exhibit emergence, we must compose **patterns**, not just glue frameworks together.

Examples of useful patterns:

- **Reflection pattern** — enable self-inspection and iterative improvement
- **Conversation loop pattern** — keep dialogue context coherent across turns
- **Task decomposition pattern** — break complex goals into executable subtasks

A pattern describes recurring relationships and strategies in a system — it finds stability _inside_ change.

Take the “feedback loop” pattern: it shows up in many domains

- in management: OKR review cycles
- in neural nets: backpropagation
- in social networks: echo chambers

Because patterns express dynamic laws, they are more fundamental and more transferable than any one framework.

---

## 3. From “writing code” to “designing behavior”

Modern software increasingly resembles a living system: it has state, feedback, and purpose.

We’re no longer only sequencing function calls; we’re designing behavior cycles:

**sense → decide → act → reflect → improve**

For agent developers this matters: whether you’re building a support agent, an analytics assistant, or an automated workflow, success isn’t decided by which framework you chose — it’s decided by whether the behavior patterns form a closed loop.

---

## 4. Pattern thinking = generative thinking

When you think in patterns your questions change.

You stop asking:

> “Which framework should I use to solve this?”

You start asking:

> “What dynamics are happening here?”
> “Which relationships recur in this system?”

In AI development:

- LLM evolution follows _emergent patterns_ of complex systems
- model alignment is a _multi-level feedback_ pattern
- multi-agent collaboration shows _self-organization_ patterns

These are not just feature stacks — they are _generators of new design paradigms_.

So: don’t rush to build another Agent framework. First observe the underlying rules of agent evolution.

Once you see these composable, recursive patterns, you stop “writing agents” and start designing the _evolutionary logic_ of intelligent systems.
