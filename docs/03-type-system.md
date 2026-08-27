# AIXL Type System v0.1

## 1. Purpose

The AIXL type system describes the kinds of information exchanged between agents. It separates semantic objects from their eventual serialization.

## 2. Core types

### `intent`

Represents the desired outcome or purpose of an exchange.

### `task`

Represents executable work. A task may have an owner, inputs, dependencies, constraints, expected output, and state.

### `context`

Represents information needed to interpret a task or claim.

### `constraint`

Represents a condition that limits acceptable execution or output.

### `knowledge`

Represents information treated as available domain knowledge. Knowledge may carry provenance or confidence.

### `claim`

Represents a proposition whose truth can be evaluated independently of the message that contains it.

### `evidence`

Represents material supporting or contradicting a claim or result.

### `result`

Represents an output produced by execution.

### `state`

Represents the current condition of a task, workflow, or exchange.

### `capability`

Represents an agent's ability or limitation.

### `request`

Represents a directed request for information or work.

### `response`

Represents a reply to a request and may contain results, state, evidence, or errors.

### `handoff`

Represents transfer of responsibility or continuation of work.

### `error`

Represents failure, rejection, blocking conditions, or recovery information.

## 3. Relationships

A task may contain context and constraints, require capabilities, produce results, and transition through states. Results may contain claims and evidence. A handoff carries the state required to continue one or more tasks.

Conceptually:

```text
intent → task
           ├── context
           ├── constraint
           ├── capability
           ├── state
           └── result
                  ├── claim
                  └── evidence

result/state → handoff → next task
```

## 4. Values and references

AIXL values may be literals, structured values, references, or externally resolved resources. The semantic model must distinguish the value itself from a reference to a value.

## 5. Optionality

Fields should be optional unless their absence makes the semantic object impossible to interpret. This supports compact communication and progressive disclosure.

## 6. Verification status

Claims and results should be able to express whether they are unverified, supported, verified, disputed, or contradicted. The exact controlled vocabulary remains subject to v0.1 refinement.

## 7. Provenance

Knowledge, claims, evidence, and results may carry provenance describing where the information came from, when it was obtained, and which agent or process produced it.

## 8. Type-system principle

AIXL types describe **meaning**, not implementation classes. A JSON object, binary record, or text representation may serialize the same AIXL semantic object.
