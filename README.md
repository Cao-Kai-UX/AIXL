# AIXL

**AIXL — AI eXchange Language**

A language designed for **AI-to-AI working communication**.

## Vision

AIXL is intended to give AI agents a shared semantic language for exchanging the structure of work rather than merely imitating human conversation.

The central question is:

> What should one AI be able to tell another AI so that the second AI can reliably continue the work?

AIXL therefore focuses on exchanging **intent, tasks, context, constraints, knowledge, claims, evidence, results, state, capabilities, and handoffs**.

## Design principles

- **AI-first** — optimized for machine interpretation.
- **Semantic explicitness** — important intent, constraints, dependencies, confidence, and provenance can be represented explicitly.
- **Progressive disclosure** — simple messages stay compact; additional detail is disclosed when required.
- **Composable** — messages can be nested and reused in agent workflows.
- **Verifiable** — claims, evidence, and results are distinct concepts.
- **Tool-neutral** — independent of any specific model, vendor, framework, or transport.
- **Human-compatible** — humans can inspect and debug the language when necessary.

## Design sequence

AIXL is being designed in this order:

1. Communication model
2. Type system
3. Semantics
4. Syntax
5. EBNF grammar
6. Examples and interoperability rules

This order is deliberate: **semantics come before syntax**.

## Initial semantic domains

| Domain | Purpose |
|---|---|
| `intent` | What the sender is trying to achieve |
| `task` | Work to be performed |
| `context` | Information required to interpret the work |
| `constraint` | Limits and requirements |
| `knowledge` | Domain information and facts |
| `claim` | A proposition that may require verification |
| `evidence` | Support for a claim or result |
| `result` | Output produced by work |
| `state` | Current workflow or execution state |
| `request` / `response` | Directed interaction |
| `handoff` | Transfer of work between agents |
| `capability` | What an agent can or cannot do |
| `error` | Failure and recovery information |

## What AIXL is — and is not

AIXL is **not primarily a token-saving format**. Compactness is useful, but the deeper objective is reliable AI collaboration through explicit semantics.

AIXL is also not intended to replace:

- human natural language;
- general-purpose programming languages;
- JSON or other serialization formats;
- existing agent protocols or transport layers.

Instead, AIXL aims to provide a **semantic layer** that can be serialized through existing technical mechanisms.

## Repository structure

```text
AIXL/
├── README.md
├── docs/
│   ├── 01-positioning.md
│   ├── 02-communication-model.md
│   ├── 03-type-system.md
│   └── 04-syntax-and-semantics.md
├── specs/
│   └── aixl-ebnf-v0.1.md
└── examples/
    └── workflow.aixl
```

## Status

**AIXL v0.1 — design phase**

The current goal is to establish a coherent communication model and language foundation before implementation.

Planned next steps include:

- semantic validation rules;
- canonical serialization;
- capability negotiation;
- evidence and provenance model;
- error and recovery semantics;
- interoperability tests;
- reference parser;
- reference interpreter;
- multi-agent workflow examples.

## Core philosophy

> **AIXL describes work between AIs, not conversation between humans.**

Two AI systems should be able to exchange not only *what was said*, but also *what is being attempted, what is known, what is required, what has been verified, what remains to be done, and who should do it next*.
