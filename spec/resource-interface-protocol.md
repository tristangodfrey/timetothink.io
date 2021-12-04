# Resource Interface Protocol

# Definitions

`R` - the set of all valid interfaces  
`P` - the set of all valid parameters  
`O[op[r, p]]` - the set of all valid outputs for a given operation where `r \in R` and `p \in P[r]`  
`OP` - the set of all valid operations  
`D[r]` - the domain of interface `r`  
`P[r]` - the set of allowed parameters for interface `r`  
`P[op]` - the set of allowed parameters for operation `op`  
`OP[r]` - the set of all valid operations for interface `r`  
`P[op[r]]` - the set of all valid parameters for operation `op[r]`  

# Invariants

`interfaceValid(r) = r \in P[R]`

Checks if a interface contains only the allowed parameters, and that their values are of the correct format.

# Model Example

`OP = {Get, Put, Pipe}`  
`R = {r1, r2, r3}` - The set of resource interfaces  
`P = {p1, p2, p3}` - The set of parameter sequences  
`O` - The set of valid outputs for all interfaces  
`OP[r1] = {Get}` - Read-only interface  
`OP[r2] = {Put}` - Write-only interface  
`OP[r3] = {Put}` - Write-only interface  
`O[r1, p1] = {1, 2, 3}` - The set of valid outputs for interface `r1` and parameter sequence `p1`  
`O[r2, p2] = {}` - The set of valid outputs for interface `r2`, since it is write-only, the output will always be empty  
`O[r3, p3] = {}` - The set of valid outputs for interface `r2`, since it is write-only, the output will always be empty  
`P[OP[r1]] = {[name: S]}` - Where `name` is a proper sequence of `S`, and `S` being the set of valid ASCII characters  
`O[op[r1, p1]] = {}`
`P[OP[r2]] = {[name: S]}` - Where `name` is a proper sequence of `S`, and `S` being the set of valid ASCII characters  

# Interface Operations

## Get

```haskell
O[op[r, p]] = Get(r, p[r]) == O[r] where operationName = Get, and p[r] is a proper sequence of P[OP[r]]
```

Where `r` is the interface, and `p[r]` is the sequence of parameters, and `O` is the discrete (finite) set defining the domain of `Get`

---

## Put

```haskell
O[op[r, p]] = Put(r, p[r]) == OP[r] where operationName = Put, and p[r] is a proper sequence of P[OP[r]]
```

Where `r` is the interface, and `p[r]` is the sequence of parameters

---

## Pipe

The output of the "Pipe" operation is defined as follows:

```haskell
O[Pipe(<<s, p>>, <<d, p>>)] = Get(s, p[s]) . Put(d, p[d])
```

Where `s` is the source interface, and `d` is the destination interface, and `.` is the operation that passes the left-hand expression as an input to the right-hand expression (e.g. `Put(d, p, Get(s, p))`),
the output of LHE is passed as `K[O[op]] = O[op]` to the sequence of parameters, where `K` is the name of the output



# Theorems

