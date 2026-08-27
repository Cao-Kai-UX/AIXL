# AIXL EBNF v0.1

> Status: working draft. This grammar defines the syntactic core of AIXL v0.1. Semantic validation remains separate from syntax.

## Grammar

```ebnf
message         = header , object ;
header          = "@aixl" , version , [ identifier ] ;
version         = "v" , digit , "." , digit ;
object          = type-name , "{" , { member } , "}" ;
member          = field , ":" , value , [ terminator ] ;
field           = identifier ;
value           = literal | reference | object | list | identifier ;
list            = "[" , [ value , { "," , value } ] , "]" ;
reference       = "@" , identifier ;
literal         = string | number | boolean | null ;
string          = '"' , { string-char } , '"' ;
number          = [ "-" ] , digit , { digit } , [ "." , digit , { digit } ] ;
boolean         = "true" | "false" ;
null            = "null" ;
type-name       = identifier ;
identifier      = letter , { letter | digit | "_" | "-" } ;
terminator      = ";" ;
letter          = "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z"
                | "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" ;
digit           = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
string-char     = ? any Unicode character except an unescaped double quote ? ;
```

## Canonical semantic object types

The v0.1 vocabulary is:

```text
intent
objective
request
response
task
action
context
constraint
capability
knowledge
fact
assumption
hypothesis
claim
evidence
inference
belief
uncertainty
unresolved
result
state
handoff
error
```

The grammar intentionally does not hard-code these names. Semantic validation determines whether an object is a recognized AIXL type and whether its fields satisfy the corresponding invariants.

## Canonical message form

Every AIXL message has exactly one top-level typed object after the header:

```text
@aixl v0.1
request {
  ...
}
```

Nested semantic objects use the same object form as values:

```text
request {
  task: {
    id: "task-001"
    input: "inspection records"
    output: "risk report"
    state: "pending"
  }
}
```

The field name gives the semantic role; the nested object's fields define its content. Semantic validation determines whether `task: { ... }` is a valid `task` value for that field.

## References

A reference uses `@identifier`:

```text
request {
  task: @task-001
}
```

References are resolved semantically rather than by the grammar. A semantic rule can require that a reference resolve to a `task`, `claim`, `evidence`, or another expected type.

## Lists

Lists may contain literals, identifiers, references, or nested objects:

```text
constraint: ["use supplied records", "cite evidence"]
evidence: [@E1, @E2]
```

## Terminators and whitespace

Semicolons are optional member terminators. Whitespace separates tokens and is otherwise insignificant. A future lexical specification may formalize comments, escape sequences, Unicode identifiers, and additional literal forms.

## Semantic versus syntactic validation

The grammar answers **whether a message is structurally AIXL syntax**. Semantic validation answers whether the message is meaningful and valid according to the AIXL type system.

For example, syntax alone cannot determine whether an `evidence` object actually supports a referenced claim. That is a semantic invariant.

## Progressive disclosure

Progressive disclosure is a semantic communication mechanism rather than a grammar trick. A compact object and a detailed object can represent the same semantic state at different disclosure levels. Later revisions may introduce explicit disclosure metadata and state operations.

## Design note

The v0.1 grammar deliberately remains small. Future revisions may add namespaces, typed scalar values, metadata, capability expressions, temporal expressions, formal reference resolution, comments, and explicit disclosure operations without changing the core semantic model.
