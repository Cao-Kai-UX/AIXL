# AIXL Syntax and Semantics v0.1

## 1. Syntax philosophy

AIXL syntax should be compact, explicit, nestable, and easy for an AI parser to process. Syntax serves the semantic model rather than defining the model itself.

AIXL v0.1 uses a small structural syntax:

```text
TYPE {
  field: value
}
```

Every complete message contains a version header and one top-level typed object.

## 2. Message form

Canonical form:

```text
@aixl v0.1
request {
  intent: "complete safety analysis"
  task: {
    id: "task-001"
    input: "inspection records"
    output: "risk report"
    state: "pending"
  }
}
```

The top-level object identifies the message's primary semantic kind. Nested objects are used when a field carries structured semantic content.

## 3. Named fields

Fields identify semantic roles rather than arbitrary application properties. Common fields include:

- `id`
- `intent`
- `objective`
- `task`
- `action`
- `context`
- `constraint`
- `input`
- `output`
- `state`
- `evidence`
- `confidence`
- `verification`
- `source`
- `owner`
- `requires`
- `depends`
- `next`
- `unresolved`

The v0.1 core vocabulary should remain stable; implementations may define extension fields without changing the meaning of core fields.

## 4. Semantic state objects

AIXL explicitly distinguishes the following work-state concepts:

```text
fact
assumption
hypothesis
claim
evidence
inference
belief
uncertainty
unresolved
```

This distinction is essential. For example:

- a `fact` is treated as established;
- an `assumption` is adopted but not established;
- a `hypothesis` is a candidate explanation or conclusion;
- an `evidence` object supports, contradicts, or qualifies a proposition;
- an `inference` records a derived relationship or conclusion;
- a `belief` records the sender's current assessment;
- an `uncertainty` records a relevant unknown or ambiguity.

These objects represent interoperable work state. They do not claim to reproduce hidden neural activations or a model's private internal state.

## 5. Nesting

AIXL objects can contain other AIXL objects. This allows a task to contain constraints and actions, a result to contain claims and evidence, and a handoff to contain or reference unfinished work and its state.

## 6. Identity and references

Semantic objects may have identifiers. References use the `@identifier` form:

```text
claim {
  id: "H1"
}

request {
  target: @H1
}
```

References allow objects to be reused without repeating their complete content. Resolution and expected type are semantic concerns rather than grammar rules.

## 7. State model

`state` describes the current condition of work. The initial controlled vocabulary is:

```text
pending
running
blocked
partial
complete
failed
awaiting-verification
```

A state transition should be represented as a meaningful change in work state, not merely as prose in a message.

## 8. Confidence and verification

Confidence and verification are separate semantic dimensions.

```text
confidence: 0.82
verification: "unverified"
```

`confidence` expresses the sender's assessment. `verification` expresses the status of a defined checking process. High confidence does not imply verification.

## 9. Evidence and provenance

Evidence may support, contradict, or qualify a claim, hypothesis, inference, or result. Relevant objects may include provenance such as:

```text
source
agent
acquired-at
source-time
method
```

The exact provenance vocabulary remains extensible, but provenance should be preserved when it materially affects trust or reproducibility.

## 10. Capability semantics

A `capability` describes what an agent can or cannot perform. A task may declare required capabilities:

```text
requires: ["web-search", "document-analysis"]
```

Capability negotiation is a semantic protocol layer. It may later define capability advertisement, requirements, limits, acceptance, and rejection. The core grammar does not encode these rules.

## 11. Progressive disclosure

Progressive disclosure means that an agent can initially exchange a compact representation and disclose additional semantic state only when needed.

Conceptually:

```text
summary
  ↓
request details
  ↓
structured state
  ↓
request evidence/inference
  ↓
detailed state
```

Disclosure level is therefore a property of communication behavior and semantic state, not merely message length. A future specification may define explicit disclosure metadata and operations.

## 12. State operations

AIXL may eventually support semantic operations such as:

```text
REQUEST verify(@F3)
UPDATE confidence(@H1, 0.91)
ADD fact(F3)
REJECT @H2
RESOLVE @U1
```

These are **not v0.1 concrete syntax**. They describe the operation model that future revisions may formalize. In v0.1, such operations can be represented as ordinary typed messages or actions.

## 13. Semantic invariants

Initial v0.1 invariants:

1. A `result` must be associated with work or an information request that produced it.
2. `evidence` must support, contradict, or qualify an identifiable proposition or result.
3. An `inference` should identify its relevant premises when those premises are necessary to interpret it.
4. A `handoff` must preserve or reference enough state for continuation.
5. A `constraint` must apply to an identifiable task, action, result, or workflow.
6. An `assumption` must not be represented as an established `fact` merely because the sender relies on it.
7. A `belief` must not be treated as equivalent to verification.
8. An `uncertainty` must identify a relevant unknown, ambiguity, or unresolved limitation.
9. A reference must resolve to an object of the expected semantic kind when such a kind is specified.

## 14. Syntax versus semantics

Syntactic validation answers:

> Is this structurally valid AIXL?

Semantic validation answers:

> Does this message represent a valid AIXL work state or operation?

For example, this may be syntactically valid:

```text
result {
  evidence: "something"
}
```

but semantic validation must determine whether the evidence is identifiable, relevant, and properly related to the result.
