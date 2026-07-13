# Context Tracer

A lightweight, local, human-readable tracer for LLM pipelines built on Ollama.

Ollama already produces good logs — token usage, input, output, duration. Context Tracer parses those logs and writes them as clean, structured JSONL. One record per interaction. Human readable. Greppable. No dashboard. No server. No cloud.

---

## What it does

Parses Ollama logs and structures them into clean JSONL records:

- Timestamp
- Raw input sent to the model
- Raw output received
- Token counts in and out
- Duration
- Model used

One file. Open in any text editor. Search with grep. No special tools required.

---

## The honest limitation

Context Tracer on a standard Ollama setup is an outside observer.

It sees what went into the model and what came out. Everything before that — how the context was assembled, what retrieval actually returned, what memory was injected, what routing decisions were made — happens upstream, invisibly, before anything reaches Ollama.

This is not a limitation of the tool. It is a limitation of mainstream LLM architecture.

In most local AI setups today, context assembly is internal. The prompt arrives at the model already assembled. There is nothing to intercept before that point. Even the most sophisticated observability tools — including enterprise solutions — are observing from the outside on these architectures. Input and output. The middle stays dark.

---

## What full pipeline visibility actually requires

Full context tracing — seeing every retrieval step, every memory injection, every routing decision, every assembly choice before the brain call — is only possible when context is assembled externally.

This is what context engineering does. Instead of assembling context inside a monolithic call, the orchestrator builds it in discrete, logged, inspectable steps. Each step has a known input and a known output. Nothing is hidden because nothing happens invisibly.

Context Tracer was originally built for SoS (School of Specialists) — an architecture where context assembly is external by design. On SoS, Context Tracer is not an outside observer. It is in the room watching context get assembled, step by step, before anything reaches the brain.

The difference is not the tool. It is the architecture.

---

## What full visibility found in the wild

On a standard Ollama setup you would see:

Input: "who is the current president of indonesia?"
Output: "as of my last update in 2023, the president is Joko Widodo..."
Tokens in: 393. Duration: 21 seconds.

You would know the answer was wrong. You would not know why.

On SoS with full pipeline tracing, the trace showed exactly what the retrieval step returned before the brain was called:

"LPP Televisi Republik Indonesia is an Indonesian national public television network..."

No president. No name. Garbage. The brain received garbage and did the only thing available to it — answered from training data. The brain was not wrong. The context was wrong. Without full pipeline visibility, the model takes the blame for a retrieval failure every time.

---

## Two tiers

**Standalone — works with any Ollama setup**
Parses Ollama logs into clean JSONL. Outside observer. Input, output, tokens, duration. Useful for anyone running local models who wants structured, readable logs without a dashboard.

**Full SoS implementation — reference only**
Complete pipeline tracing across every step — rules, context assembly, retrieval, memory injection, brain calls. Requires external context assembly by design. Ships as reference implementation alongside Nanana when released.

---

## Hardware this was built and tested on

Intel Core i3 6th generation. No dedicated GPU. Windows native. Ollama for local model serving.

If it works here it works anywhere.

---

## License

Personal and non-commercial use — free under AGPL-3.0.

Commercial use — contact NLT for licensing.

---

## Part of the NLT ecosystem

Context Tracer is the first standalone release from NLT — No Laptop Trades.

Built on the philosophy: real tools for real use on modest machines.

Related: School of Specialists (SoS) — a network-first architecture for right-sized, model-agnostic local AI assistants. [Paper link]

Substack: nolaptoptrades.substack.com

If you find it useful, a Ko-fi keeps the i3 running. [link]
