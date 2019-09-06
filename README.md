# pipeline

Copyright (C) 2019 Abser Ari

A Pipeline manages a pipelined in-order request/response sequence.

To use a Pipeline p to manage multiple clients on a connection,
each client should run:

1. generate id
```go
id := p.Next() // take a number
```

2. do your request
```go
p.StartRequest(id) // wait for turn to send request
«send request»
p.EndRequest(id)   // notify Pipeline that request is sent
```

3. handle response
```go
p.StartResponse(id)    // wait for turn to read response
«read response»
p.EndResponse(id)  // notify Pipeline that response is read
```

A pipelined server can use the same calls to ensure that
responses computed in parallel are written in the correct order.
