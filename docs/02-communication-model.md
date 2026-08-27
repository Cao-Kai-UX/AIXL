# AIXL Communication Model

## 1. Core question

AIXL starts from one question:

> What must one AI communicate so that another AI can reliably understand and continue the work?

The answer is not merely a text message. A working exchange may contain intent, context, task, constraints, knowledge, capability requirements, execution state, claims, evidence, results, and a next action.

## 2. Communication unit

An AIXL exchange is a structured working message. Conceptually:

```text
message = intent + context + task + constraints + knowledge + state + expected/result
```

Not every field is mandatory. The message should contain only the semantic information required for the current exchange.

## 3. Work lifecycle

A typical collaboration can be represented as:

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
State updates
  ↓
Result + Evidence
  ↓
Verification
  ↓
Handoff / next task
```

## 4. Agent roles

AIXL does not require a fixed hierarchy. Agents may act as:

- initiator — establishes an intent or request;
- executor — performs a task;
- verifier — evaluates claims/results;
- coordinator — decomposes and assigns work;
- provider — supplies knowledge or capability;
- receiver — accepts a handoff and continues execution.

One agent can occupy multiple roles in a workflow.

## 5. Request and response

A request identifies work another agent is expected to perform. A response communicates the execution result, state, evidence, or inability to complete the request.

A response does not necessarily mean the whole workflow is finished. It may return an intermediate state and request further action.

## 6. Handoff

A handoff transfers enough structured state for another agent to continue work without reconstructing the entire previous conversation.

A useful handoff therefore distinguishes:

- objective;
- completed work;
- unresolved work;
- relevant context;
- constraints;
- assumptions;
- evidence;
- current state;
- required next action.

## 7. Verification

AIXL distinguishes a claim from evidence supporting that claim. A result can therefore be accompanied by evidence and a verification state rather than being treated as automatically true.

## 8. Capability awareness

Agents should be able to communicate capabilities and limitations. A task can therefore be routed to an agent based on what it can actually perform.

## 9. State

State is part of working communication rather than hidden application metadata. This permits an agent receiving a message to understand whether work is pending, executing, partially complete, complete, blocked, failed, or awaiting verification.

## 10. Minimality

AIXL should support a minimal message and an expanded message with the same semantic identity. This is the foundation for progressive disclosure.
