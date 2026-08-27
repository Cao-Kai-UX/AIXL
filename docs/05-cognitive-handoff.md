# AIXL Cognitive Handoff v0.1

## 1. Why Cognitive Handoff

AIXL is designed to transfer the **current state of work** between AI agents so that the receiving agent can continue reliably without reconstructing the entire preceding conversation.

The key distinction is:

> AIXL transfers a representation of completed work and current work state, not a transcript of conversation.

This does not mean that AIXL directly transfers a model's hidden neural state. In v0.1, cognitive state is represented explicitly as interoperable semantic objects.

## 2. Problem statement

A conventional handoff is often:

```text
Agent A → natural-language summary → Agent B
```

Agent B must infer what A knows, what A assumes, what A has verified, what remains unresolved, and what should happen next.

An AIXL handoff instead exposes relevant state explicitly:

```text
Agent A
  ↓
Facts / Assumptions / Hypotheses / Evidence
  ↓
Inferences / Beliefs / Uncertainty
  ↓
Completed / Unresolved work
  ↓
Constraints / Current state / Next action
  ↓
AIXL Handoff
  ↓
Agent B
```

## 3. Cognitive/work state model

A cognitive handoff should be able to represent at least:

- `objective` — what the work is trying to achieve;
- `context` — information needed to interpret the work;
- `fact` — information currently treated as established;
- `assumption` — a premise adopted for reasoning but not established;
- `hypothesis` — a candidate explanation or conclusion;
- `claim` — a proposition that can be evaluated;
- `evidence` — support for or against a proposition or result;
- `inference` — a reasoning-derived relationship or conclusion;
- `belief` — the sender's current assessment, optionally with confidence;
- `uncertainty` — a relevant unknown, ambiguity, or limitation;
- `constraint` — conditions that must remain satisfied;
- `result` — work already completed or an output produced;
- `unresolved` — work or questions that remain open;
- `action` — a requested or recommended next operation;
- `capability` — abilities or limitations relevant to continuation.

The model deliberately distinguishes **fact, assumption, hypothesis, inference, belief, and uncertainty**. These are not interchangeable descriptions of “thinking.”

## 4. Cognitive Handoff object

Conceptually:

```text
handoff {
  task: {
    id: "T001"
  }
  objective: "identify_root_cause"

  facts: [ ... ]
  assumptions: [ ... ]
  hypotheses: [ ... ]
  claims: [ ... ]
  evidence: [ ... ]
  inferences: [ ... ]
  beliefs: [ ... ]
  uncertainty: [ ... ]
  constraints: [ ... ]

  completed: [ ... ]
  unresolved: [ ... ]
  next: [ ... ]

  state: "partial"
}
```

The concrete syntax follows the formal v0.1 grammar. The example uses nested objects and lists but does not prescribe every field as mandatory.

## 5. Handoff continuity invariant

A receiving agent should be able to determine, directly or through references:

1. the original objective;
2. work already completed;
3. established facts versus assumptions;
4. active hypotheses and claims;
5. evidence supporting or contradicting them;
6. relevant inferences;
7. the sender's current beliefs and confidence where provided;
8. relevant uncertainties;
9. binding constraints;
10. unresolved work;
11. the next recommended or required action.

The receiver does not need to reproduce the sender's private reasoning process. It needs sufficient **interoperable work state** to continue the task.

## 6. Progressive disclosure

Progressive disclosure is a core communication behavior of AIXL.

### Level 0 — summary

Transmit only enough information for the receiver to understand the current outcome and whether further detail is needed.

```text
summary {
  conclusion: @H1
  confidence: 0.82
  unresolved: 1
}
```

### Level 1 — structured state

The receiver requests or receives selected facts, assumptions, hypotheses, evidence, constraints, or other state objects by identifier.

### Level 2 — detailed artifacts

The sender discloses the supporting evidence, inference relationships, provenance, or other details required to evaluate an important or disputed conclusion.

The principle is:

> **Do not transmit all available state when the receiver does not need it; preserve the ability to disclose it when needed.**

Disclosure level is a communication policy, not a claim about the depth of the sender's hidden neural computation.

## 7. Capability-aware handoff

Before or during handoff, an agent may declare required capabilities and the receiver may advertise capabilities or limitations.

Conceptually:

```text
request {
  task: @T1
  requires: ["document-analysis", "web-search"]
}

response {
  capability: {
    name: "document-analysis"
    status: "available"
  }
}
```

Capability negotiation remains a semantic extension area in v0.1; the core grammar does not hard-code capability names.

## 8. State operations

AIXL should eventually support explicit operations on shared work state, for example:

```text
REQUEST verify(@F3)
UPDATE confidence(@H1, 0.91)
ADD fact(@F3)
REJECT @H2
RESOLVE @U1
```

These are semantic operation examples, **not v0.1 concrete grammar**. Until operations are formally specified, they can be represented through ordinary AIXL requests, actions, and state-bearing messages.

## 9. Cognitive state versus natural-language thinking

A natural-language reasoning trace is not identical to a model's internal computational state.

Therefore AIXL v0.1 makes no claim that an `inference` object reproduces hidden states, activations, attention states, or KV-cache contents.

The distinction is:

```text
internal neural state ≠ textual reasoning trace ≠ AIXL cognitive/work state
```

AIXL targets the third: an explicit, interoperable representation of work state.

## 10. Future neural-state interoperability

A future specification may investigate model-specific or model-compatible representations such as hidden states, latent representations, or KV-cache-derived state. Such a layer is outside v0.1 because models can differ in architecture, dimensions, tokenization, representation spaces, and compatibility.

AIXL v0.1 therefore remains useful without neural-state compatibility.

## 11. Experimental hypothesis

The first implementation should test whether structured handoff improves continuation compared with conventional natural-language handoff.

### Control

```text
Agent A → natural-language summary → Agent B
```

### Experimental condition

```text
Agent A → AIXL work state → Agent B
```

Measure at least:

- final task accuracy;
- information omitted during handoff;
- duplicated reasoning;
- input/output token usage;
- continuation success rate;
- time or number of model calls.

The experiment should test the hypothesis rather than assume AIXL is superior.

## 12. Core principle

> **AIXL should transfer the state of work, not merely the history of conversation.**
