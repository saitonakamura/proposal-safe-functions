# Proposal safe functions

Proposal for error handling

## Motivation

Error handling is hard and dynamic nature javascript doesn't help the issue. You can't be sure if anything will throw an error unless you check on docs. Even callback based api can throw an error, e.g. WebSocket constructor can throw if port is blocked. Functional languages like OCaml of F# use monadic approach to do error handling

