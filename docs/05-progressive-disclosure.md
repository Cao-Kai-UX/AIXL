# AIXL Progressive Disclosure

## 1. Motivation

AI collaboration often fails because the receiver is given either too little context or an unnecessarily large context window. AIXL treats information disclosure as part of communication semantics.

## 2. Principle

A sender can provide a compact semantic representation first and disclose additional context only when needed.

```text
minimal → relevant detail → full context → source/evidence
```

## 3. Layers

### Layer 0 — Intent

What outcome is wanted?

### Layer 1 — Task

What work should be performed?

### Layer 2 — Required context

What information is necessary to execute the task?

### Layer 3 — Constraints and dependencies

What conditions, assumptions, or prerequisites affect execution?

### Layer 4 — Evidence and provenance

What supports the supplied claims or results?

### Layer 5 — Full detail

Additional raw material is disclosed only when required.

## 4. Negotiation

The receiver may request more detail when the current disclosure is insufficient. The sender should be able to answer with the missing semantic layer without retransmitting unrelated information.

## 5. Goal

Progressive disclosure is not only a token optimization. It reduces irrelevant context, makes dependencies explicit, and allows agents with different context capacities to collaborate.

## 6. Compatibility

Progressive disclosure must preserve semantic identity. Expanding a message should reveal additional information about the same task, claim, result, or workflow rather than silently changing its meaning.
