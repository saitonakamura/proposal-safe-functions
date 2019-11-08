# Proposal safe functions

Proposal for error handling

## Motivation

Error handling is hard and dynamic nature javascript doesn't help the issue. You can't be sure if anything will throw an error unless you check on docs. Even callback based api can throw an error, e.g. WebSocket constructor can throw if port is blocked. Functional languages like OCaml of F# use monadic approach to do error handling. But monads are infamously hard by themselves, especially when you don't have a syntax sugar for them. But we're already have monadic flavour features, such as async/await and null conditional operator. 

## Description

```js

safe function createWebSocket(url) {
  return new WebSocket(url)
}
safe function foo() {
  const webSocket = try createWebSockets('ws://example.com')
  const readyState = webSocket.readyState
  console.log('ready state is', readyState)
}
```

Which can be transpiled to
```js
function createWebSocket(url) {
  try {
    return new OkResult(new WebSocket(url))
  } catch (error) {
    return FailResult(error)
  }
}

function foo() {
  try {
    const webSocketResult = createWebSocket('ws://example.com')
    
    if (webSocketResult.ok) {
      // return failed result
      return webSocketResult
    }
    
    const webSocket = webSocketResult.value
    const readyState = webSocket.readyState
    console.log('ready state is', readyState)
    // return last tried result
    return webSocketResult
  } catch (error) {
    return FailResult(error)
  }
}
```
