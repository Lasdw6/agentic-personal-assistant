# Agentic Personal Assistant

## ⚠️ Disclaimer

This is a **public documentation repository**.

- The **implementation code is private**, as the system runs against my personal data and id rather not have it stolen.
- This repo exists to document **design and system architecture**.
- If you’re interested in a **walkthrough or live demo**, feel free to reach out to me.

---

## Overview

The Agentic Personal Assistant (APRIL) is an multi agentic system I designed to use my personal data (documents, emails, calendar) to make my life easier.


---

## 🏗️ High-Level Architecture

> **Diagram placeholder:** `diagrams/high-level-architecture.png`

<!--
INSERT IMAGE HERE
-->

At a high level, the system is structured as a backend service with clear separation between:

- API layer (request validation and orchestration)
- Agent execution runtime
- Memory systems (short-term vs long-term)
- External integrations (email, calendar, vector store)

Design goals:
- Make agent execution observable and debuggable
- Avoid hidden state and uncontrolled autonomy
- Treat memory as a first-class system concern

---

## ⚙️ Agent Execution Model

> **Diagram placeholder:** `diagrams/agent-execution-flow.png`

<!--
INSERT IMAGE HERE
-->

Each request flows through a constrained execution pipeline:

1. **Request ingestion**
   - Inputs are validated and normalized at the API boundary.

2. **Planning**
   - The agent determines whether retrieval or tools are required.
   - Autonomous chaining is intentionally limited.

3. **Tool execution**
   - Tools are invoked with explicit input/output schemas.
   - Tool outputs are validated before reuse.

4. **Response synthesis**
   - Final responses are generated using validated intermediate results only.

The system prioritizes **predictability and debuggability** over maximal autonomy.

---

## 🧩 Memory Model

> **Diagram placeholder:** `diagrams/memory-lifecycle.png`

<!--
INSERT IMAGE HERE
-->

The assistant uses **explicit memory separation**:

### Short-Term Memory
- Exists only for the duration of a request or task
- Stores intermediate reasoning and tool outputs
- Never persisted

### Long-Term Memory
- Stores durable information (documents, user-provided context)
- Backed by a vector store for retrieval
- Write access is intentionally constrained

Key principles:
- Not everything should be remembered
- Memory writes are explicit, not automatic
- Retrieval quality matters more than recall volume

---

## 🔎 Retrieval-Augmented Generation (RAG)

The system uses a retrieval pipeline to query personal documents and structured data.

Characteristics:
- Explicit chunking and embedding strategy
- Retrieval scoped to task-relevant context
- Iterated on after observing hallucinations and irrelevant context under real usage

RAG is treated as a **retrieval system**, not just a prompt enhancement.

---

## 📊 Observability & Debugging

The system is instrumented to support live operation and iteration:

- Structured logs for agent decisions and tool calls
- Execution traces for multi-step tasks
- Latency metrics to identify bottlenecks
- Visibility into failure modes instead of silent degradation

Observability exists because **agent systems fail in non-obvious ways**.

---

## ⚠️ Failure Modes & Tradeoffs

This project explicitly documents known limitations:

- Early designs overused autonomous agent chaining, leading to unpredictable behavior and difficult debugging.
- Naive similarity-based retrieval surfaced irrelevant context for long-tail queries.
- Stateless designs failed for multi-turn tasks and were refactored.

Current tradeoffs:
- Reduced autonomy → increased reliability
- Explicit control → easier debugging
- Simpler execution paths → predictable behavior

These choices are intentional.

---

## 🚀 Deployment & Operation

- Operated as a long-running, containerized backend service
- Schema changes and memory resets handled explicitly
- Backward-incompatible changes required migration or cleanup

This system is maintained as something that must **keep working**, not a one-off experiment.

---

## 🎥 Demo

> **Demo video placeholder:** (link coming soon)

<!--
INSERT DEMO VIDEO LINK HERE
-->

The demo will include:
- Live agent execution
- Retrieval traces
- Observability during a real task
- Discussion of constraints and failure handling

If you’re interested in a walkthrough before the video is available, feel free to reach out.

---

## 🔧 Status

This is an actively evolving personal system.
Current focus areas:
- Improving retrieval quality
- Tightening execution guardrails
- Making failures easier to reason about

