# AIXL EBNF v0.1

> Status: working draft. The grammar captures the current conceptual design and is expected to evolve with semantic validation.

## Grammar

```ebnf
message         = header , body ;
header          = "@aixl" , version , [ identifier ] ;
version         = "v" , digit , "." , digit ;
body            = object | statement-list ;
object          = type-name , "{" , { member } , "}" ;
member          = field , ":" , value , [ terminator ] ;
field           = identifier ;
value           = literal | reference | object | list | identifier ;
list            = "[" , [ value , { "," , value } ] , "]" ;
reference       = "@" , identifier ;
literal         = string | number | boolean | null ;
string          = '"' , { string-char } , '"' ;
number          = [ "-" ] , digit , { digit } , [ "." , { digit } ] ;
boolean         = "true" | "false" ;
null            = "null" ;
type-name       = identifier ;
identifier      = letter , { letter | digit | "_" | "-" } ;
terminator      = ";" ;
letter          = "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z"
                | "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" ;
digit           = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
string-char     = ? any Unicode character except an unescaped double quote ? ;
statement-list  = statement , { statement } ;
statement       = object | member ;
```

## Canonical semantic object types

The initial vocabulary is:

```text
intent
request
response
task
context
constraint
knowledge
claim
evidence
result
state
capability
handoff
error
```

## Example

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
  constraint: ["use supplied records", "cite evidence"]
}
```

## Grammar notes

The EBNF intentionally defines a small syntactic core. Semantic validation determines whether a particular object is meaningful as an AIXL `task`, `result`, `handoff`, etc.

The grammar should not prematurely encode every semantic rule. Later revisions may add namespaces, typed values, metadata, capability expressions, disclosure levels, temporal expressions, and formal reference resolution.
