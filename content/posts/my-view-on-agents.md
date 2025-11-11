+++
date = '2025-11-11T14:24:50+08:00'
draft = true
title = 'My View on Agents'
+++

**My View on Agents: From Workflows to Strategic Thinking**

OpenAI defines an _Agent_ as a system that integrates model capabilities, tool interfaces, and strategies — capable of autonomously perceiving, deciding, acting, and improving its performance.

Claude, on the other hand, highlights the goal-driven and interactive nature of Agents: they not only understand and generate information, but also refine their behavior through continuous feedback.

In my view, if an LLM is the **brain**, then an Agent is the **body that acts on behalf of that brain**.
An LLM is like a _super-intelligent search engine and content generator_ — it can understand problems and produce answers, but it doesn’t act on its own.
An Agent, in contrast, is like a _thoughtful, hands-on assistant_ — it not only understands and generates, but also takes initiative and adapts based on feedback.

---

### A simple example: weekly reports

Before LLMs, writing a weekly report meant manually gathering data, summarizing project progress, picking highlights, formatting, and sending it out.

![this is a image](/images/report_by_llm.png "Report By LLM")

With LLMs, you can now dump your notes or project summaries into the model and have it generate the report.
That’s convenient — but you still need to copy, paste, and send the final file yourself. The LLM understands and writes, but it doesn’t _do_.

![this is a image](/images/report_by_agent.png "Report By Agent")

With an Agent, you simply say: “Prepare and send the weekly report.”
The Agent automatically gathers data (say, from your CRM), checks project updates (from Jira, Notion, or local folders), generates the report using an LLM, and then sends it out — all by itself.
Over time, it learns from feedback and refines how it structures and prioritizes future reports.

An Agent, in this sense, acts like a _conscientious personal assistant_ — you express the goal, and it completes the entire process while improving each time.

---

### The real value of Agents

The true power of an Agent isn’t just in understanding or generating information — it lies in **acting, deciding, and improving**.
That’s why developers must shift their focus: from building _processes_ to designing _methods and strategies_.

---

## Rethinking Agent Development

When developing Agents, we need to move from **linear workflows** to **strategic maps**.
Traditional software design is about defining a fixed sequence of steps.
Agent design, by contrast, is about enabling goal-driven decision-making.

---

### Old way: “Process Thinking” (Traditional Systems)

![this is a image](/images/process_thinking.png "Process Thinking")

**Mindset:** “What functions do I need to implement?”
**Implementation:**

- The user enters an order number and selects a question type from a dropdown.
- The system uses a rigid `if...then...else` rule set to find an answer.
- If nothing matches, it creates a support ticket for a human to handle.

**Developer experience:**
My focus was making sure the process didn’t break — as long as order input worked and tickets were created, my job was done. But users often found it clunky and limited.

**Core concern:** Process correctness.

---

### New way: “Strategic Thinking” (Agent Systems)

**Mindset:** “How can the system choose the best strategy on its own to solve the user’s problem?”
**Implementation:**

- The user types freely: “Can I return my red shoes order?” (unstructured input).
- The Agent invokes the LLM to interpret intent — it infers the goal is to process a return for the red-shoe order.
- The Agent autonomously checks the user’s history and stock, sees that one-click return is allowed, and replies: “Your return request has been submitted. Please check your email.”
- If information is missing, the Agent proactively asks for it — instead of freezing.

**Developer experience:**
My focus shifted from “features” to “decision chains.”
I gave the Agent tools and objectives, and it figured out the best way to achieve them.
The system became more flexible — more like a skilled teammate than a static program.

**Core concern:** Strategic optimality.

---

### From Process to Strategy — The Mental Shift

This evolution from _process-focused_ to _strategy-focused_ thinking is what defines modern AI development.
An Agent isn’t just another layer of automation — it’s a **new architectural paradigm** that redefines how we design, build, and evaluate software systems.

In the future, successful AI developers won’t be those who write the most complex code — but those who design **the most elegant, efficient, and self-improving strategies**.
