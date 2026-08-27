# AIXL

**AIXL — AI eXchange Language**

A language designed for **AI-to-AI working communication and cognitive handoff**.

## Vision

AIXL is intended to give AI agents a shared semantic language for exchanging the structure of work rather than merely imitating human conversation.

The central question is:

> What should one AI be able to tell another AI so that the second AI can reliably continue the work?

AIXL therefore focuses on exchanging **intent, tasks, context, constraints, knowledge, claims, evidence, reasoning artifacts, work state, capabilities, results, and handoffs**.

## Core idea

AIXL is not primarily a more compact natural language. Its deeper purpose is to represent and transfer **AI work state**.

A successful handoff should allow Agent B to understand:

- what Agent A was trying to achieve;
- what work has already been completed;
- which facts are established;
- which assumptions were made;
- which hypotheses remain active;
- what evidence supports or contradicts them;
- what Agent A currently believes and how certain it is;
- what remains uncertain or unresolved;
- which constraints remain binding;
- and what should happen next.

> **AIXL should transfer the state of work, not merely the history of conversation.**

## Design principles

- **AI-first** — optimized for machine interpretation.
- **Semantic explicitness** — important intent, constraints, dependencies, confidence, and provenance can be represented explicitly.
- **Progressive disclosure** — simple messages stay compact; additional detail is disclosed when required.
- **Composable** — messages can be nested and reused in agent workflows.
- **Verifiable** — claims, evidence, and results are distinct concepts.
- **State-aware** — work state and unfinished work are first-class communication objects.
- **Handoff-oriented** — one agent can transfer enough structured state for another to continue.
- **Tool-neutral** — independent of any specific model, vendor, framework, or transport.
- **Human-compatible** — humans can inspect and debug the language when necessary.

## Communication model

AIXL treats AI-to-AI interaction as a working process rather than a chat transcript:

```text
Intent
  ↓
Task definition
  ↓
Context + Constraints
  ↓
Capability / assignment
  ↓
Execution
  ↓
Work State
  ├── Facts
  ├── Assumptions
  ├── Hypotheses
  ├── Evidence
  ├── Inferences
  ├── Beliefs
  └── Uncertainty
  ↓
Result / Verification
  ↓
Handoff / Next Action
```

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
| `assumption` | A premise adopted during reasoning |
| `hypothesis` | A candidate explanation or conclusion |
| `inference` | A reasoning-derived relationship or conclusion |
| `belief` | Current confidence or assessment |
| `uncertainty` | Missing information or unresolved ambiguity |
| `result` | Output produced by work |
| `state` | Current workflow or execution state |
| `request` / `response` | Directed interaction |
| `handoff` | Transfer of work between agents |
| `capability` | What an agent can or cannot do |
| `action` | Requested or recommended next operation |
| `error` | Failure and recovery information |

## Progressive disclosure

AIXL should support a minimal-to-detailed exchange:

```text
Agent A → summary
Agent B → REQUEST evidence(H1)
Agent A → evidence(H1)
Agent B → REQUEST inference(H1)
Agent A → detailed inference(H1)
```

This allows agents to exchange only the information required at each stage while retaining access to deeper state when needed.

## What AIXL is — and is not

AIXL is **not primarily a token-saving format**. Compactness is useful, but the deeper objective is reliable AI collaboration through explicit semantics and transferable work state.

AIXL is also not intended to replace:

- human natural language;
- general-purpose programming languages;
- JSON or other serialization formats;
- existing agent protocols or transport layers.

Instead, AIXL aims to provide a **semantic layer** that can be serialized through existing technical mechanisms.

AIXL v0.1 also does **not** claim that textual reasoning reproduces a model's hidden neural state. Internal neural state, natural-language reasoning traces, and interoperable AIXL cognitive state are distinct concepts.

## Design sequence

AIXL is being designed in this order:

1. Communication model
2. Type system
3. Semantics
4. Syntax
5. EBNF grammar
6. Examples and interoperability rules
7. Reference implementation and experiments

This order is deliberate: **semantics come before syntax**.

## Experimental direction

The first prototype will compare:

```text
Control:
Agent A → natural-language summary → Agent B

AIXL:
Agent A → structured cognitive state → Agent B
```

The experiment will measure task accuracy, duplicated reasoning, omitted information, token usage, continuation success, and number of model calls.

## Repository structure

```text
AIXL/
├── README.md
├── docs/
│   ├── 01-positioning.md
│   ├── 02-communication-model.md
│   ├── 03-type-system.md
│   ├── 04-syntax-and-semantics.md
│   └── 05-cognitive-handoff.md
├── specs/
│   └── aixl-ebnf-v0.1.md
└── examples/
    └── workflow.aixl
```

## Status

**AIXL v0.1 — design and experimental prototype phase**

Current priority:

- establish the cognitive handoff model;
- refine semantic types and invariants;
- define progressive disclosure;
- define capability negotiation;
- define evidence and provenance;
- define error and recovery semantics;
- build a minimal AIXL parser/validator;
- run controlled multi-agent handoff experiments.

## Core philosophy

> **AIXL describes work between AIs, not conversation between humans.**

Two AI systems should be able to exchange not only *what was said*, but also *what is being attempted, what is known, what has been verified, what remains uncertain, what work has already been completed, and who should do what next*.
