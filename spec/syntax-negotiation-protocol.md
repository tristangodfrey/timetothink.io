# Syntax Negotiation Protocol

## Definition Parser

Allows clients to define custom definitions for common types such as:

- Sets
- Sequences
- Functions

These definitions can then be passed to interfaces as definition metadata, helping them understand
the DSL.

An implementer of this protocol may choose to include a compiled "parser" in a given operation, such that
the resource receiving the instruction may remain as pure as possible, and simply call the parser before
processing the message.

### Backus-Naur Notation

The Backus-Naur Form is a way of defining syntax. It consists of:

- A set of terminal symbols (`T`)
- A set of non-terminal symbols (`NT`)
- A set of production rules of the form: `LHS ::= RHS` where `LHS` is a single non-terminal symbol (`LHS = [s \in NT]`), and `RHS` is a sequence of symbols (terminals or non-terminal)

### Protocol

If a client wishes to send a custom parser to an interface, the interface must support the following operation:

```haskell
O[op[r, p]] = Put(r, p[r]) == OP[r] where operationName = Put, and p[r] is a proper sequence of P[OP[r]] && p[r]["parser"] != FALSE
```

Where `p[r]["parser"]` is a sequence of bits using UTF-8 encoding

### Parser Metadata

Since reading only the bytecode of a parser does not yield enough information to know how to use the parser function,
metadata must be included that describes the set of allowed inputs.

`p[r]["validator"] = V[i]` where `i` is the input and `V` is the validator function

In order for the resource to execute the function, it must be compiled for the appropriate ISA. Since the ISA of
the resource cannot be known in advance (and sometimes may change without warning), the operation **must** include a set of binary
streams that is executable on each ISA:

`[amd64, arm, x86_64]`

The resource must then select the appropriate ISA binary stream depending on the platform, or execute them in order until it finds one that works (not reccommended due to performance reasons).

