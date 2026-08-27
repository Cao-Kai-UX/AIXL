# AIXL Positioning

## 1. Definition

AIXL (AI eXchange Language) is a language for AI-to-AI working communication.

Its purpose is to let AI systems exchange the structure of work: intent, context, constraints, tasks, knowledge, evidence, results, state, capabilities, and handoffs.

## 2. Problem

AI systems currently communicate through natural language, JSON, tool calls, APIs, and framework-specific protocols. These mechanisms provide transport or serialization, but do not by themselves define a common semantic language for AI collaboration.

AIXL addresses the semantic layer.

## 3. Scope

AIXL is designed for:

- agent-to-agent task delegation;
- collaborative problem solving;
- structured context transfer;
- verification and evidence exchange;
- workflow state transfer;
- capability negotiation;
- work handoff and continuation.

AIXL is not intended to replace natural language, programming languages, JSON, APIs, or network transports.

## 4. Token efficiency

Compactness is a design benefit, not the primary objective. The primary objective is reliable exchange of work semantics.

## 5. Design order

The project follows:

**communication model → type system → semantics → syntax → EBNF → examples/interoperability**.

Semantics should be established before syntax so that syntax does not determine the conceptual model prematurely.

## 6. Design principles

### AI-first

The language prioritizes precise machine interpretation over conversational style.

### Semantic explicitness

Intent, constraints, dependencies, confidence, provenance, and verification state should be representable explicitly.

### Progressive disclosure

A sender should not have to transmit every available detail. Information can be disclosed in layers as the receiver requires it.

### Composability

Communication units should be nestable and reusable.

### Verifiability

Claims, evidence, and results should be distinguishable.

### Tool neutrality

The language should not depend on a particular model, vendor, framework, or transport.

### Human inspectability

Although AI-first, AIXL should remain inspectable and debuggable by humans.
