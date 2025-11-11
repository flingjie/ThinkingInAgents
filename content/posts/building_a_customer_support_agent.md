+++
date = '2025-11-11T17:03:14+08:00'
draft = true
title = 'Building a Customer Support Agent: From Linear Flows to Expert Routing'
+++

Traditional customer service bots rely heavily on _if/else_ rules and rigid intent-matching. The moment a user says something vague or deviates from the expected flow, the system breaks down. This is what we call **“process thinking.”**

In the Agent era, we shift toward **“strategic thinking”** — building intelligent systems that can make decisions autonomously and dynamically route conversations to the right experts. Such a system isn’t just an LLM; it’s an **LLM-powered network of specialized experts.**

This article walks through a practical implementation of a **multi-expert customer service Agent** using five core prompts and **LangGraph**, showing how this shift in thinking comes to life.

---

## **Architecture Evolution: The “Expert Router” Strategy**

The key design principle of a support Agent is simple: use a central “router brain” to classify user queries, then delegate each one to the **expert model** best suited to handle it.

| Module              | Role           | Core Strategy                   | Prompt                                                   |
| ------------------- | -------------- | ------------------------------- | -------------------------------------------------------- |
| **Main Controller** | Commander      | Decides intent and routing path | Intent Classifier                                        |
| **Expert Models**   | Domain Experts | Solve specialized sub-goals     | Judgment / Information / Chitchat / Exception Assistants |

---

![this is a image](/images/workflow_of_customer_service_agent.png "workflow_of_customer_service_agent")

## **Execution Flow Overview**

1. The user submits a question.
2. The **Main Controller (Router)** analyzes the input and returns a routing key.
3. The system forwards the query and context to the corresponding **Expert Model**.
4. Each expert, guided by its own specialized prompt and tools, generates a professional response.

---

## **Step 1: The Core Module — Designing the Five Expert Prompts**

### 🚦 1. The Router (Main Controller)

This is the Agent’s **brain** and the starting point of all decisions.
Its goal isn’t to answer, but to identify intent and route efficiently.

**Prompt Strategy:**
Force _mutually exclusive classification_ (ensuring only one route per query) and output in structured JSON for easy parsing.

**Thinking Upgrade:**
From “intent matching” to “strategic routing.”
Instead of just classifying _what_ the question is about (“a refund”), it determines _how_ it should be handled (“a yes/no refund decision”).

| Category Key  | Expert Model          | Goal                             |
| ------------- | --------------------- | -------------------------------- |
| `yes_no`      | Judgment Assistant    | Return a clear yes/no conclusion |
| `information` | Information Assistant | Extract and summarize facts      |
| `chitchat`    | Chitchat Assistant    | Provide conversational responses |
| `exception`   | Exception Assistant   | Guide user clarification         |

**Prompt Example:**

> You are an intelligent routing assistant for an FAQ chatbot.
> Based on the user’s most recent question, classify it into one of the following **five mutually exclusive categories** and return the corresponding **English key**.
>
> ### Category Definitions
>
> 1. **Yes/No Question** – key: `yes_no`
>    The user expects a confirmation or denial.
>    Examples: “Can I get a refund?” “Does this work on Android?”
> 2. **Informational Question** – key: `information`
>    The user asks for facts or instructions.
>    Examples: “What’s your customer service number?” “How do I reset my password?”
> 3. **Chitchat / Irrelevant** – key: `chitchat`
>    Small talk or unrelated input.
>    Examples: “How’s your day?” “Tell me a joke.”
> 4. **Exception / Complaint / Ambiguous** – key: `exception`
>    The user expresses confusion or dissatisfaction.
>    Examples: “The system’s broken!” “Why doesn’t this work?”
>
> ### Output Format
>
> ```json
> {
>   "category": "<Chinese category name>",
>   "key": "<routing key>",
>   "reason": "<brief explanation>"
> }
> ```

---

### 🎯 2. The Expert Models (1–4): Domain Precision

Each expert model focuses on one type of query and one output goal — ensuring clarity, reliability, and specialization.

---

#### 🔹 Expert 1: **Judgment Assistant **

**Goal:** Return a definitive binary answer.
**Prompt Strategy:** Allow only “yes,” “no,” or “uncertain.” Never fabricate or guess.
**Thinking:** When data is missing, admit uncertainty instead of hallucinating.

**Prompt Example:**

> You are a precise, reliable assistant.
> Determine whether the user’s question is true or false based on the reference data.
> Output only one clear conclusion (“Yes” or “No”), with a short explanation.
> If insufficient information is available, respond “Uncertain” and explain why.
> Never invent facts.

---

#### 🔹 Expert 2: **Information Assistant **

**Goal:** Provide concise, accurate, complete information.
**Prompt Strategy:** Use only retrieved knowledge (RAG results); summarize without adding assumptions.
**Thinking:** Shift from _generation_ to _information synthesis_ for factual reliability.

**Prompt Example:**

> You are a knowledgeable assistant.
> Using the reference materials, provide a clear, accurate, and complete answer.
>
> - Use only the given references.
> - Summarize concisely if multiple points are relevant.
> - If no answer is found, say “No related information found.”
> - Remain objective and factual.

---

#### 🔹 Expert 3: **Chitchat Assistant **

**Goal:** Maintain natural, empathetic small talk.
**Prompt Strategy:** Avoid facts or knowledge; focus on emotion and rapport.
**Thinking:** Filters out off-topic input and keeps the conversation human.

**Prompt Example:**

> You are a warm, friendly conversational partner.
> Continue the conversation naturally based on the chat history.
>
> 1. Keep it natural and sincere.
> 2. Avoid factual or technical content.
> 3. Reply in 1–2 short, human-like sentences.
> 4. Respond to emotion first, then to topic.
> 5. Do not include any system messages or tags.

---

#### 🔹 Expert 4: ** Exception Assistant **

**Goal:** Help users clarify vague or invalid inputs gracefully.
**Prompt Strategy:** Never fabricate; guide users to restate their problem politely.
**Thinking:** Treat “errors” as opportunities for recovery, not dead ends.

**Prompt Example:**

> You are a calm, helpful assistant.
> When the input is incomplete, confusing, or irrelevant, do **not** guess or output technical errors.
> Instead, help the user clarify their question politely.
> Keep replies under two sentences and focus on continuing the conversation.

---

## **Step 2: Implementing in LangGraph**

**LangGraph** lets you design and execute autonomous, controllable AI systems with a **graph-based mental model**.

### 1. Define the Shared State

```python
class AgentState(TypedDict):
    messages: List[BaseMessage]
    category: str = None
    reference: str = ""
    answer: str = ""
```

### 2. Router Node: Classify and Route

```python
def classify_question(state: AgentState) -> AgentState:
    result = llm_response(CLASSIFIER_PROMPT_TEMPLATE, state)
    state['category'] = result.get("key", "exception")
    return state
```

### 3. Expert Nodes

```python
def expert_yes_no(state: AgentState) -> AgentState:
    state["reference"] = tool_retrieve_from_rag(state)
    state["answer"]  = llm_response(YES_NO_PROMPT, state)
    return state
```

…and similarly for the other experts.

### 4. Routing Logic

```python
def route_question(state: AgentState) -> str:
    category = state.get("category")
    return {
        'yes_no': 'to_yes_no',
        'information': 'to_information',
        'chitchat': 'to_chitchat'
    }.get(category, 'to_exception')
```

### 5. Build the Graph

```python
workflow = StateGraph(AgentState)
workflow.add_node("classifier", classify_question)
workflow.add_node("yes_no_expert", expert_yes_no)
workflow.add_node("info_expert", expert_information)
workflow.add_node("chitchat_expert", expert_chitchat)
workflow.add_node("exception_expert", expert_exception)
workflow.set_entry_point("classifier")
workflow.add_conditional_edges("classifier", route_question, {
    "to_yes_no": "yes_no_expert",
    "to_information": "info_expert",
    "to_chitchat": "chitchat_expert",
    "to_exception": "exception_expert",
})
app = workflow.compile()
```

---

## **Step 3: Testing**

```python
run_agent("Can I get a refund?")
run_agent("What’s your support hotline?")
run_agent("How’s your day?")
```

This demonstrates how **strategic thinking** replaces linear scripting:

- **Classifier node:** doesn’t rush to answer; decides how to handle it.
- **Dynamic routing:** dispatches queries to the right expert in real time.
- **Expert execution:** each model stays focused on its purpose and optimized prompt.

---

## **Conclusion: The Shift in Agent Thinking**

| Dimension            | Traditional (Process Thinking)              | Agent (Strategic Thinking)                      |
| -------------------- | ------------------------------------------- | ----------------------------------------------- |
| **Architecture**     | Linear if/else logic, one model handles all | Expert routing network, multi-model cooperation |
| **Problem Handling** | Failure leads to fallback or human handoff  | Dynamic decision-making via routing             |
| **Prompt Design**    | One prompt tries to do everything           | Each prompt handles one precise sub-goal        |
| **Focus**            | Whether each step executes correctly        | Whether the overall strategy achieves the goal  |

---

This is the essence of **Agent-based design** — not just smarter models, but smarter systems that can reason, route, and self-optimize.
