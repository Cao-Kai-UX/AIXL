# AIXL Syntax and Semantics v0.1

## 1. Syntax philosophy

AIXL syntax should be compact, explicit, nestable, and easy for an AI parser to process. Syntax must serve the semantic model rather than become the model itself.

## 2. Semantic form

The conceptual form is:

```text
TYPE { field: value }
```

A message may contain nested semantic objects:

```text
request {
  intent: ...
  task: ...
  context: ...
  constraint: ...
}
```

The exact concrete punctuation remains part of the v0.1 grammar work.

## 3. Named fields

Fields identify semantic roles rather than arbitrary application properties. Common fields include:

- `id`
- `intent`
- `task`
- `context`
- `constraint`
- `input`
- `output`
- `state`
- `evidence`
- `confidence`
- `source`
- `owner`
- `requires`
- `depends`
- `next`

Implementations may extend the field vocabulary while preserving core semantics.

## 4. Nesting

AIXL objects can contain other AIXL objects. This allows a task to contain constraints, a result to contain claims and evidence, and a handoff to contain an unfinished task and its state.

## 5. Identity and references

Semantic objects may have identifiers. References allow another object to refer to an existing object without repeating its complete content. This is important for compact multi-step workflows.

## 6. State transitions

State describes where work currently is. A task can transition through states such as pending, running, blocked, partial, complete, failed, and awaiting-verification. Implementations should avoid treating state names as mere prose.

## 7. Confidence and verification

Confidence describes the sender's assessment; verification describes whether a claim or result has been independently checked. They are related but not equivalent.

## 8. Errors

An error should identify what failed, its impact, whether recovery is possible, and what action is expected next when known.

## 9. Semantic invariants

Initial invariants:

1. A `result` must be associated with work or an information request that produced it.
2. `evidence` supports, contradicts, or qualifies a `claim` or `result`.
3. A `handoff` must preserve enough state for continuation.
4. A `constraint` applies to an identifiable task, result, or workflow.
5. A reference must resolve to an object of the expected semantic kind.

These invariants will become machine-validatable in later specifications.
