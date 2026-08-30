# AIXL v0.1 Semantic Object Map

## 1. Purpose

This document consolidates the existing AIXL v0.1 semantic concepts into one object model. It does not introduce a new vocabulary. Its purpose is to clarify the layer, role, and relationships of concepts already defined across the communication model, type system, syntax/semantics, and cognitive handoff documents.

> **AIXL v0.1 is modeled as semantic objects connected by explicit relations and acted upon by semantic operations.**

## 2. High-level map

```text
                              AIXL
                                |
        +-----------------------+-----------------------+
        |                       |                       |
      ACTORS                  WORK                 KNOWLEDGE
        |                       |                 / COGNITIVE STATE
   +----+----+             +----+-----+               |
   |         |             |          |        +------+-------+
 Agent   Capability       Task      Context    |              |
             |             |          |       Proposition   Evidence
          Contract      Objective Constraint      |
                                                   +-- Fact
                                                   +-- Assumption
                                                   +-- Hypothesis
                                                   +-- Claim
                                                   +-- Inference
                                                   +-- Belief
                                                   +-- Uncertainty

 Task
  |
  +--> Action --> Result --> State
  |
  +--> Assignment / Delegation --> Agent
  |
  +--> Handoff --> receiving Agent

 Claim <--supports/contradicts-- Evidence
 Claim <--about----------------- Belief / Hypothesis
 Inference <--derived-from------ Evidence / prior state
 Claim <--verifies-------------- Verification

 Reference --> any identifiable semantic object
```

## 3. Semantic layers

### 3.1 Actor layer

| Object | Meaning | Primary relations |
|---|---|---|
| `agent` | Participant capable of AIXL work | `has`, `delegates`, `assigned-to` |
| `capability` | Ability or limitation relevant to work | `has`, `requires` |
| `contract` | Conditions governing an agreed interaction | constrains task/action exchange |

`Capability` answers **what an agent can do**. `Contract` answers **under what agreed conditions an interaction can occur**. They are related but not interchangeable.

### 3.2 Work layer

| Object | Meaning | Primary relations |
|---|---|---|
| `task` | Primary unit of work exchange | `contains`, `requires`, `depends-on`, `assigned-to`, `produces` |
| `objective` | Intended outcome of a task | contained by / associated with `task` |
| `context` | Information required to interpret the work | associated with `task` |
| `constraint` | Binding requirement or limitation | constrains `task` / `action` / workflow |
| `action` | Operation requested or recommended | required by / executed for `task` |
| `result` | Output or completed work produced by execution | produced by `action`; may update `state` |

For v0.1, `objective` is the canonical term for the desired outcome. `goal` may appear as an informal or syntax-level synonym, but it should not create a second semantic object unless a later version establishes a distinct meaning.

### 3.3 Knowledge and cognitive-state layer

These concepts are deliberately distinct:

```text
Fact         established information
Assumption   adopted premise not established
Hypothesis   candidate explanation or conclusion
Claim        evaluable proposition
Evidence     support for or against a proposition/result
Inference    reasoning-derived relationship or conclusion
Belief       current assessment about a proposition
Uncertainty  relevant unknown, ambiguity, or limitation
```

Typical relations:

```text
Evidence    --supports/contradicts--> Claim
Inference   --derived-from---------> Evidence / prior state
Belief      --about----------------> Claim
Hypothesis  --about----------------> Claim
```

A `belief` is not itself evidence, and `confidence` is not verification. Confidence describes an agent's current degree of belief; verification describes whether a defined checking process has been performed and what it concluded.

### 3.4 Workflow-state layer

`state` represents the lifecycle condition of work. The v0.1 lifecycle vocabulary should be unified around:

```text
pending
accepted
running
blocked
completed
failed
cancelled
```

`partial` and `awaiting-verification` should not be treated as competing lifecycle states. They are better represented as completion/verification attributes or conditions. This avoids maintaining two incompatible state vocabularies.

### 3.5 Handoff layer

`handoff` is a work-state transfer operation/object between agents. It packages enough interoperable state for a receiving agent to continue without reconstructing the entire conversation.

A handoff may carry or reference:

```text
Task / Objective
Context / Constraints
Facts / Assumptions / Hypotheses / Claims
Evidence / Inferences / Beliefs / Uncertainty
Completed / Unresolved work
Current State
Next Action
```

The receiver does not need the sender's private neural state or a complete reasoning transcript.

### 3.6 Reference layer

`identifier` gives a semantic object a stable identity. `reference` points to that object.

```text
Identifier
    |
    +--> identifies --> semantic object

Reference
    |
    +--> points-to --> semantic object
```

References support progressive disclosure, reuse, compact messages, deduplication, and cross-message continuity.

## 4. Core relations

The existing relationship vocabulary can be grouped as follows.

### Structural

```text
contains
references / points-to
```

### Workflow

```text
requires
produces
updates
depends-on
```

### Responsibility

```text
assigned-to
delegates
delegated-to
```

### Epistemic

```text
supports
contradicts
derived-from
about
```

### Verification

```text
verifies
```

### Handoff

```text
transfers
```

These relations should be preferred over inventing field-specific duplicates when the same meaning can be expressed relationally.

## 5. Semantic operations

AIXL v0.1 already identifies state-operation patterns such as:

```text
REQUEST verify(@F3)
UPDATE confidence(@H1, 0.91)
ADD fact(@F3)
REJECT @H2
RESOLVE @U1
```

These are currently semantic operation examples rather than final concrete grammar. They establish the direction for a future operation model without prematurely freezing the syntax.

## 6. Object / Relation / Operation model

The consolidated v0.1 model is therefore:

```text
AIXL Semantic Model
│
├── Objects
│   ├── Actors
│   ├── Work objects
│   ├── Knowledge / cognitive objects
│   ├── Workflow state
│   └── Handoff / verification objects
│
├── Relations
│   ├── structural
│   ├── workflow
│   ├── responsibility
│   ├── epistemic
│   ├── verification
│   └── reference
│
└── Operations
    ├── request
    ├── update
    ├── add
    ├── reject
    ├── resolve
    └── future operation forms
```

## 7. Open architectural decisions

This map deliberately records unresolved issues instead of silently inventing answers:

1. Whether `intent` remains a first-class semantic object or becomes a property of a task/request.
2. Whether `goal` is permanently an alias of `objective` or is given a distinct future meaning.
3. The exact boundary between `capability` and `contract`.
4. The final lifecycle vocabulary for `state`.
5. The formal operation model and its relationship to concrete syntax.
6. Whether `message` is a semantic object, an envelope/transport construct, or merely a serialization container.

These decisions should be resolved before freezing a new EBNF revision.

## 8. Design rule

> **Do not add a new AIXL v0.1 concept unless it can be placed unambiguously in the object, relation, or operation model and its necessity can be demonstrated.**
