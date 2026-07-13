# Context Tracer: Lightweight JSONL Log Parser for Local Ollama Pipelines

A minimalist, local, human-readable tracer for LLM pipelines built on Ollama. 

Ollama already produces good logs—token usage, input, output, duration. **Context Tracer** parses those logs and structures them into clean, append-only JSONL files. One record per interaction. Human readable. Greppable. No dashboard. No server. No heavy Docker stack. No cloud dependency.

---

## 🚀 What it Does

Parses raw Ollama server logs and structures them into clean JSONL records tracking:
*   **Timestamp:** Precision tracking of the turn execution.
*   **Model:** The specific local model layout utilized.
*   **Raw Input / Output:** Unaltered prompt payloads and token responses.
*   **Token Statistics:** Accurate prompt and completion token counts.
*   **Duration:** Generation latency metrics.

### Expected Output Example
```json
{"timestamp": "2026-07-13T16:21:00Z", "model": "qwen2.5:3b", "tokens_in": 393, "tokens_out": 42, "duration_ms": 21000, "input": "...", "output": "..."}

```

*One file. Open in any text editor. Search with standard command-line tools like grep. No special dashboard required.*

---

## ⚠️ Status: Alpha Architecture Release

> **Note:** The standalone Python log parsing script (`tracker.py`) is currently being refactored for portable, zero-dependency environment execution. The code will land in this repository within the next few days. The architectural specification below outlines the execution footprint.

---

## 🔍 The Honest Limitation: The Outside Observer

Context Tracer on a standard Ollama setup operates as an **outside observer**.

It logs what went into the model and what came out. Everything before that—how the context was assembled, what retrieval steps actually returned, what memory registers were injected, or what routing choices were made—happens upstream, invisibly, before anything reaches Ollama.

This is not a limitation of the tool. It is an inherent limitation of mainstream monolithic LLM frameworks.

In most local AI setups today, context assembly is locked internally. The prompt arrives at the model already pre-assembled. There is nothing to intercept mid-flight. Even the most sophisticated corporate observability tools observe strictly from the outside on these setups. Input and output are tracked, but the middle stays entirely dark.

---

## 🧠 What Full Pipeline Visibility Actually Requires

Full context tracing—tracking every retrieval step, every memory injection array, every routing decision, and every token assembly choice before the main brain call—is only possible when context is assembled **externally**.

This is the core foundation of **context engineering**. Instead of assembling context inside a hidden monolithic loop, the system orchestrator builds it in discrete, logged, inspectable steps. Each step has a known input and a known output. Nothing is hidden because nothing happens invisibly.

Context Tracer was originally engineered for the **SoS (School of Specialists)** framework—an architecture where context assembly is external by design. Within an SoS deployment, Context Tracer ceases to be an outside observer. It sits directly in the room watching context get built, step by step, register by register, before anything hits the model brain.

**The difference is not the utility tool. It is the core system architecture.**

---

## 🪵 What Full Visibility Found in the Wild

On a standard, closed Ollama setup you might see a execution turn like this:

* **Input:** *"Who is the current president of Indonesia?"*
* **Output:** *"As of my last update in 2023, the president is Joko Widodo..."*
* **Tokens In:** 393 | **Duration:** 21 seconds.

You would recognize the answer was wrong or outdated. But you would have absolutely no way to know **why** the logic failed.

Running an **SoS architecture** with full pipeline tracking active, the exact same interaction exposed the exact trace payload returned by the retrieval step *before* the brain was invoked:

> `"LPP Televisi Republik Indonesia is an Indonesian national public television network..."`

No political metrics. No relevant names. Pure contextual noise. The small model received garbage and did the only thing available to its parameters—it answered from its static training data. The model wasn't broken. The context retrieval was broken. Without full pipeline visibility, the model takes the blame for a infrastructure failure every single time.

---

## 🛠️ Deployment Tiers

1. **Standalone Tier (Works with any default Ollama setup):** Parses raw local Ollama logs directly into clean JSONL. Operates as an outside observer logging input, output, tokens, and duration. Built for anyone running local models who needs clean text ledgers without a bloated UI dashboard.
2. **Full SoS Implementation (Reference Only):** Delivers complete end-to-end pipeline tracing across every orchestrator step—hard system rules, context tag assembly, knowledge retrieval, memory injection, and final brain execution calls. Ships as the reference implementation alongside the *Nanana* engine release.

---

## 💻 Targeted Hardware Footprint

* **Processor:** Intel Core i3 6th Generation.
* **Graphics:** No dedicated GPU (Pure CPU/System RAM inference).
* **OS:** Windows Native environment.
* **Serving Engine:** Ollama local model management.

*If it runs efficiently here, it runs absolutely anywhere.*

---

## ⚖️ License

* **Personal and Non-Commercial Use:** Completely free under the **AGPL-3.0 License**.
* **Commercial Applications:** Please contact **NLT** directly for commercial licensing structures.

---

## 🌐 Part of the NLT Ecosystem

Context Tracer is the premier standalone utility release from **NLT — No Laptop Trades**.

We operate on a simple philosophy: **Real tools built for real use on modest, everyday machines.**

* **Related Research:** School of Specialists (SoS)—a network-first, model-agnostic local AI assistant architecture. [Read the Architecture Paper (DOI: 10.5281/zenodo.21190620)](https://doi.org/10.5281/zenodo.21190620)
* **Connect:** Follow real-time development logs and updates on [Telegram](https://t.me/Ynolaptoptrades) or visit the [NLT Website](https://nolaptoptrades.com).
* **Support:** If this architecture saves your CPU cycles, a Ko-fi keeps the i3 running. `[Ko-fi Link]`
