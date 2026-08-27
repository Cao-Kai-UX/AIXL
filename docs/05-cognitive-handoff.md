# AIXL Cognitive Handoff v0.1

## 1. Why Cognitive Handoff

AIXL should not be limited to exchanging conversational text. The deeper objective is to allow one AI agent to transfer the **current state of its work** to another agent so that the receiver can continue reliably without reconstructing the entire preceding conversation.

The key distinction is:

> AIXL transfers a representation of completed cognitive work, not a transcript of conversation.

This does not mean that AIXL directly transfers the sender model's hidden neural state. In v0.1, cognitive state is represented explicitly as interoperable semantic objects.

## 2. Problem statement

Suppose Agent A is analyzing a complex task and Agent B must continue it.

A conventional handoff might be:

```text
Agent A → natural-language summary → Agent B
```

Agent B must infer what A knows, what A believes, what A has verified, what remains unresolved, and what should happen next.

An AIXL handoff instead exposes this state explicitly:

```text
Agent A
  ↓
Facts / Assumptions / Hypotheses / Evidence
  ↓
Inference / Belief / Uncertainty
  ↓
Completed work / Unresolved work
  ↓
Current state / Constraints / Next action
  ↓
AIXL Cognitive Handoff
  ↓
Agent B
```

## 3. Cognitive state model

A cognitive handoff should be able to represent at least:

- `objective` — what the work is trying to achieve;
- `context` — information needed to interpret the work;
- `fact` — information currently treated as established;
- `assumption` — a premise adopted for reasoning;
- `hypothesis` — a candidate explanation or conclusion;
- `evidence` — support for or against a claim or hypothesis;
- `inference` — reasoning-derived relationship or conclusion;
- `belief` — current confidence or assessment;
- `uncertainty` — unresolved ambiguity or missing information;
- `constraint` — conditions that must remain satisfied;
- `result` — work already completed;
- `unresolved` — work that remains open;
- `action` — a requested or recommended next operation;
- `capability` — abilities or limitations relevant to continuation.

## 4. Cognitive Handoff object

Conceptually:

```text
handoff {
  task: T001
  objective: identify_root_cause

  facts { ... }
  assumptions { ... }
  hypotheses { ... }
  evidence { ... }
  inferences { ... }
  beliefs { ... }
  uncertainty { ... }
  constraints { ... }

  completed { ... }
  unresolved { ... }
  next { ... }

  state: partial
}
```

The concrete syntax remains subject to the formal AIXL grammar.

## 5. Handoff continuity

A receiving agent should be able to determine:

1. what the original objective is;
2. what work has already been completed;
3. which facts are established and which are assumptions;
4. which hypotheses remain active;
5. what evidence supports or contradicts them;
6. what the sender currently believes and with what confidence;
7. what remains uncertain;
8. what constraints must not be violated;
9. what work remains;
10. what action should happen next.

The receiver is not required to reproduce the sender's internal reasoning process. It needs a sufficient semantic state to continue the task.

## 6. Progressive disclosure

Cognitive handoff should support multiple levels of disclosure.

### Level 0 — summary

```text
summary {
  conclusion: H1
  confidence: 0.82
  unresolved: 1
}
```

### Level 1 — structured state

The receiver can request facts, evidence, hypotheses, constraints, or other state objects by identifier.

### Level 2 — detailed reasoning artifacts

The sender can disclose the supporting inference and evidence required to evaluate a disputed or important conclusion.

This allows AIXL to remain compact when the receiver already has sufficient information while preserving access to deeper information when needed.

## 7. State operations

AIXL should eventually support operations on shared working state, including concepts such as:

```text
REQUEST verify(F3)
UPDATE confidence(H1, 0.91)
ADD fact(F3)
REJECT H2
RESOLVE unresolved(U1)
```

These are semantic examples, not final grammar.

The important principle is that AI-to-AI communication can modify an explicit work state rather than merely append conversational messages.

## 8. Cognitive state versus natural-language thinking

A natural-language reasoning trace is not identical to the internal computational state of a neural model.

Therefore AIXL v0.1 makes no claim that a textual `inference` object reproduces hidden states, activations, attention states, or KV cache contents.

Instead, it defines an **interoperable semantic representation** of the work performed by the sender.

This distinction is fundamental:

```text
internal neural state ≠ textual reasoning trace ≠ AIXL cognitive state
```

AIXL aims to make the third one explicit and interoperable.

## 9. Future neural-state interoperability

A future AIXL specification may investigate a neural-state layer for exchanging model-specific or model-compatible representations such as hidden states, latent representations, or KV-cache-derived state.

Such a layer is outside v0.1 because different models may use incompatible architectures, dimensions, tokenizers, representations, and semantic spaces.

The protocol should therefore remain useful without requiring neural-state compatibility.

## 10. Experimental hypothesis

The first implementation should test whether structured cognitive handoff improves continuation compared with conventional natural-language handoff.

### Control

```text
Agent A → natural-language summary → Agent B
```

### Experimental condition

```text
Agent A → AIXL cognitive state → Agent B
```

Measure at least:

- final task accuracy;
- information omitted during handoff;
- amount of duplicated reasoning;
- input/output token usage;
- continuation success rate;
- time or number of model calls required.

The purpose is not to assume AIXL is superior, but to establish measurable evidence for or against the design.

## 11. Core principle

> **AIXL should transfer the state of work, not merely the history of conversation.**

That principle connects AIXL's communication model, type system, progressive disclosure, verification model, and future multi-agent interoperability work.
