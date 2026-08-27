# AIXL Type System v0.1

## 1. Purpose

The AIXL type system defines the semantic objects exchanged between AI agents. Types describe **meaning**, not serialization or implementation classes.

The v0.1 model has two closely related layers:

1. **Work communication types** — request, response, task, result, handoff, etc.
2. **Cognitive/work-state types** — fact, assumption, hypothesis, evidence, inference, belief, uncertainty, etc.

The second layer is the distinguishing feature of AIXL: it makes the state of work explicit enough for another agent to continue it.

## 2. Core semantic types

### `intent`

The desired outcome or purpose of an exchange.

### `objective`

The enduring goal of a task or workflow. An objective may remain stable while individual tasks and actions change.

### `request`

A directed request for information, work, verification, or an action.

### `response`

A reply to a request. A response may contain results, state updates, evidence, errors, or further requests.

### `task`

Executable work. A task may have an owner, inputs, dependencies, constraints, required capabilities, expected outputs, and state.

### `action`

A concrete operation to perform or recommend. An action may be attached to a task or handoff.

### `context`

Information required to interpret a task, claim, state, or result.

### `constraint`

A condition that limits acceptable execution, state transitions, or output.

### `capability`

An ability, limitation, or service an agent can provide. Capabilities may be advertised, requested, required, negotiated, or declined.

### `knowledge`

Information available as domain knowledge. Knowledge may carry provenance, time, and confidence.

## 3. Cognitive/work-state types

### `fact`

Information currently treated as established within the work state. A fact should be distinguishable from an assumption or hypothesis and may carry provenance and verification status.

### `assumption`

A premise adopted for the current work but not established as fact. Assumptions should be explicit because downstream reasoning may depend on them.

### `hypothesis`

A candidate explanation, interpretation, or conclusion that remains open to evaluation.

### `claim`

A proposition that can be evaluated as true, false, supported, disputed, or otherwise qualified. A claim may be used independently of a particular reasoning step.

### `evidence`

Information or an artifact that supports, contradicts, or qualifies a claim, hypothesis, inference, or result.

### `inference`

A relationship or conclusion derived from one or more inputs such as facts, assumptions, claims, or evidence. An inference should identify its relevant premises where practical.

### `belief`

The sender's current assessment of a proposition or hypothesis, including confidence where available. A belief is not equivalent to a verified fact.

### `uncertainty`

An unresolved ambiguity, missing information, unknown variable, or confidence limitation that materially affects the work.

### `unresolved`

A work item, question, or issue that remains open and prevents or potentially affects completion.

## 4. Execution and transfer types

### `result`

An output produced by execution, analysis, or an information request. A result may contain claims, evidence, verification status, and provenance.

### `state`

The current condition of a task, workflow, or exchange. State is semantic work information, not merely application metadata.

### `handoff`

A transfer of responsibility or continuation of work. A handoff must contain or reference enough work state for the receiving agent to continue without reconstructing the entire prior conversation.

### `error`

A failure, rejection, blocking condition, or recovery state. An error should identify impact and, where known, the expected recovery action.

## 5. Relationships

A typical work graph is:

```text
intent → objective → task → action
                    ├── context
                    ├── constraint
                    ├── capability
                    ├── state
                    └── result

work state
├── fact
├── assumption
├── hypothesis ──┐
├── claim ───────┼── evidence
├── inference ───┘
├── belief
├── uncertainty
└── unresolved

result / state / unresolved work → handoff → next task
```

These relationships are semantic expectations, not all mandatory containment rules.

## 6. Verification and confidence

Verification and confidence are distinct:

- **confidence** expresses the sender's assessment;
- **verification** expresses the status of an independent or defined checking process.

A proposition can have high confidence while remaining unverified. Conversely, a verified observation does not require a subjective confidence score.

Initial verification vocabulary:

```text
unverified
supported
verified
disputed
contradicted
```

The controlled vocabulary may be refined in later revisions.

## 7. Provenance

`knowledge`, `fact`, `claim`, `evidence`, `inference`, and `result` may carry provenance describing, where available:

- source;
- originating agent or process;
- acquisition time;
- source timestamp;
- transformation or derivation;
- verification information.

Provenance is semantic information and should not be confused with transport metadata.

## 8. Identity and references

Any semantic object may have an identifier. References allow another object to refer to an existing object without repeating its complete content.

A reference must resolve to an object of the expected semantic kind when the surrounding semantic rule requires a particular type.

## 9. Optionality

Fields are optional by default. A field becomes mandatory only when its absence makes the semantic object impossible to interpret or violates a defined invariant.

This rule supports progressive disclosure: a sender can provide a compact representation first and disclose additional state when requested or required.

## 10. Type-system invariants

Initial v0.1 invariants:

1. A `result` must be associated with work or an information request that produced it.
2. `evidence` supports, contradicts, or qualifies an identifiable proposition or result.
3. An `inference` should identify its relevant premises when those premises are necessary to interpret it.
4. A `handoff` must preserve or reference enough state for continuation.
5. A `constraint` must apply to an identifiable task, result, action, or workflow.
6. An `assumption` must not be represented as an established `fact` merely because the sender currently relies on it.
7. A `belief` must not be treated as equivalent to `verification`.
8. An `uncertainty` represents something unresolved or unknown that is relevant to the work state.
9. A reference must resolve to an object of the expected semantic kind when such a kind is specified.

## 11. Serialization independence

AIXL types can be serialized as text, JSON, binary records, or another transport representation. Serialization does not change the semantic type.
