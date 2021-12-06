# Messaging Protocol

Features:

- Multiple message formats
- Broker interop
- Custom message parsers
- Efficient broker queue ordering (based on message urgency, includes complex use-cases)
- Due to its generality it can be implemented on a large set of existing protocols (GraphQL, REST, HTTP, SSH, WebSockets, etc.)

## Components

### Message Broker



## Guarantees

- Message delivery -> a valid message will always lead to a state change in the system
- Eventual consistency -> messages will not be lost, and the state will eventually be consistent with the expected state for the given messages at `t + n`, where `t` is the send time, and `n` is the sum of temporal intervals for each
function `f` that is to be executed for the given set of messages
- Performance can be improved by tweaking the schedule inference algorithm (making it more optimistic in its estimates)
- Idempotence -> resources (paradoxically) are defined as stateless pure functions, meaning that in the case of faulty message duplication, no unexpected behaviour will occurr.

## Queue Ordering

Urgency is defined as the amount of time a broker may take to process a message (and optionally produce a message'),
this is determined by knowing the execution time of a function in both the best and worst-case scenarios (e.g. best = O(n), worst = O(n^2))

Since big O notation defines complexity in terms of number of operations (function "iterations", where each recursion is another step if applicable),
some extra work needs to be done to generate an accurate temporal interval. The time it will take for a resource `R` to process a message `M`, is defined
as follows:

let `MQ[R]` be the ordered sequence of messages in the queue for the resource `R`

let `m` be the message for which the temporal interval must be generated, the interval is defined as:


### Asking for an estimate

### Inferring a priority based on the execution schedule

`<<t = <<start, end>>, p>>` where `t` is the interval, and `p` is the probability of correctness


`T[F[M]] = `