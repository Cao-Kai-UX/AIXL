# AIXL v0.1 Design Specification (Draft)

## 1. Positioning

AIXL (AI eXchange Language) is a structured semantic exchange language designed for AI-to-AI work communication.

The primary goal is not merely to replace JSON or shorten text. AIXL aims to let AI systems exchange reusable, verifiable, cacheable semantics instead of repeatedly explaining the same concepts in natural language.

## 2. Communication Model

AIXL communication is centered on:

- Agent identity
- Capability
- Type
- Context
- Task
- Contract
- Constraint
- Result
- Reference

A typical flow is:

```text
Agent A
  ↓ task
Agent B
  ↓ capability / contract validation
Execution
  ↓
Result
```

## 3. Design Principles

1. Semantic First
2. Syntax ≠ Semantics
3. Canonical Stability
4. Lossless Compactness
5. Progressive Disclosure
6. Cache Before Compression
7. Implementation Neutrality
8. Benchmark Driven
9. Minimal Core

## 4. Type System

Initial value types:

```text
string
integer
float
boolean
null
identifier
reference
list<T>
set<T>
object
```

Structured types support fields, optional fields, defaults, inheritance, and constraints.

Important semantic distinctions:

```text
missing ≠ null
reference ≠ embedded object
list ≠ set
```

Example:

```aixl
type risk {
    id: identifier
    severity: severity
    confidence: float[0..1]
    evidence: list<evidence>
}
```

## 5. Agent Model

An Agent has a stable identity and a set of capabilities.

```aixl
agent "SafetyAI" {
    id: A001
    capabilities: [risk_analysis]
}
```

Stable IDs are preferred over names for references and historical compatibility.

## 6. Capability Model

A Capability describes what an Agent can do and forms a machine-verifiable contract.

```aixl
capability risk_analysis {
    input: document
    output: list<risk>

    constraints: {
        evidence_required(true)
    }

    quality: {
        accuracy >= 0.90
    }
}
```

A capability contract may describe input, output, constraints, quality, cost, permissions, and other execution requirements.

## 7. Task Model

A Task is the primary work-exchange unit.

```aixl
task {
    id: T001
    from: agent("Planner")
    to: agent("SafetyAI")
    action: risk_analysis
    target: document("report.pdf")
    priority: high
}
```

Conceptual fields include:

```text
id
version
from
to
reply_to
context
action
target
goal
payload
constraints
status
priority
deadline
```

## 8. Semantic Validation

Validation is separated into six layers:

```text
Structural
   ↓
Type
   ↓
Reference
   ↓
Capability
   ↓
Protocol
   ↓
Constraint
```

Error families:

```text
E1xx Syntax
E2xx Type
E3xx Reference
E4xx Protocol
E5xx Capability
E6xx Constraint
E7xx Authorization
E8xx Runtime
```

Examples:

```text
E201 type_mismatch
E301 unresolved_reference
E401 invalid_state_transition
E501 capability_not_found
E502 contract_mismatch
E601 constraint_violation
E701 permission_denied
E801 execution_failed
E802 timeout
```

Example contract mismatch:

```text
expected: document
received: image
```

The validator should return structured errors so another Agent can understand and repair a Task rather than merely receiving a boolean failure.

## 9. Task State Machine

```text
pending
  ↓
accepted
  ↓
running
  ├── completed
  ├── failed
  ├── cancelled
  └── blocked
```

Invalid transitions produce protocol errors.

## 10. Progressive Capability Disclosure

AI capability information should be disclosed progressively:

```text
Discovery
   ↓
Summary
   ↓
Schema
   ↓
Contract
   ↓
Validation
   ↓
Execution
```

If the receiving Agent already has the relevant Contract cached, the sender can provide a stable capability hash instead of transmitting the full description again.

This is a central AIXL efficiency mechanism.

## 11. Canonical Representation

AIXL defines a normalization layer between parsing and compact transport.

Core rule:

> Equivalent AIXL semantics MUST produce an identical canonical representation.

Canonicalization covers:

- whitespace
- field ordering
- map ordering
- set ordering
- numeric normalization
- booleans
- identifiers
- references
- default values

Lists remain ordered; Sets are normalized as unordered collections.

Canonical representation can be hashed for:

- Capability Cache
- Context Cache
- Message Deduplication
- Contract Verification
- Compact Encoding

## 12. Compact Encoding

Compact Encoding is a low-redundancy transport representation, not a separate language.

The fundamental invariant is:

```text
decode(encode(X)) ≡ X
```

The representation pipeline is:

```text
Human AIXL
    ↓
Canonical AIXL
    ↓
Compact AIXL
```

Compact mechanisms include:

- message codes
- field codes
- stable references
- session dictionaries
- capability dictionaries
- capability hashes
- schema-aware positional encoding

Example conceptual compression:

```text
task {
    id: T001
    from: @A002
    to: @A001
    action: risk_analysis
    target: @D001
}
```

may become, after dictionaries and schemas are negotiated:

```text
t[T001 @2 @1 #1 @4]
```

Exact encoding rules remain Draft until benchmarked and implemented.

## 13. AST

The AST is a semantic intermediate representation rather than a direct copy of the concrete syntax.

```text
AIXLDocument
├── Module[]
├── Import[]
├── Type[]
├── Enum[]
├── Capability[]
├── Agent[]
├── Context[]
└── Message[]
```

Message forms include Task, Query, Response, Result, Event, and Error.

Value nodes include:

```text
String
Integer
Float
Boolean
Null
Identifier
Reference
List
Set
Object
Constructor
```

## 14. Syntax / EBNF Direction

The language includes declarations such as:

```text
module
import
type
enum
capability
agent
context
task
query
response
result
event
error
```

The EBNF is still a Draft and should be frozen only after the reference parser and tests agree with it.

## 15. Reference Implementation Plan

Implementation order:

```text
AST
 ↓
Lexer
 ↓
Parser
 ↓
Validator
 ↓
Canonicalizer
 ↓
Compact Encoder
 ↓
Benchmark
```

The first executable core should support:

```text
agent
capability
context
task
```

Golden scenario:

```text
Planner
  ↓
SafetyAI
  ↓
risk_analysis
  ↓
document
  ↓
risk[]
```

A deliberate failure case should verify that an image supplied to a document-only capability produces a structured contract mismatch.

## 16. Benchmark Plan

AIXL should not claim a fixed Token saving percentage before measurement.

The same semantic task should be encoded as:

1. Natural Language
2. JSON
3. AIXL Human
4. AIXL Canonical
5. AIXL Compact

Measure:

- Token count
- Character count
- Parse time
- Semantic completeness
- Validation behavior
- Error recovery
- Repeated-context savings

The key hypothesis is that major efficiency gains come from eliminating repeated semantic descriptions through schema, capability, context, and dictionary caching—not merely from shortening keywords.

## 17. Roadmap

```text
v0.1  Language core / Draft specification
  ↓
v0.2  Parser + Semantic Validator
  ↓
v0.3  Canonical + Compact implementation
  ↓
v0.4  Agent communication protocol
  ↓
v0.5  Multi-Agent experiments + benchmarks
  ↓
v1.0  Stable specification
```

## 18. Status

This document consolidates the AIXL design developed in the project conversation. Items explicitly described as Draft should be treated as provisional until implemented and tested.
