# TracingSurvival

Tracing what is happening to a single path of action across multiple components of a distributed system.

```
Client       ServiceA      ServiceB
  |
  | ----------> |
                | ----------> |
                              |
                | <---------- |
  | <---------- |
```
See https://www.w3.org/TR/trace-context/. Information is passed through both request and response (in HTTP headers).

## Attributes
* **version**: Always hex value 0x00.
* **trace-id**: The ID of the trace forest. Effectively a unique request ID that is created when the root request is created.
* **parent-id**: ID of the current request. Some systems call this span-id. Each new leg of the trace gets a new ID.
* **trace-flags**: Flags to control sampling and more.

## Components
* _Traceparent Header_: version, trace-id, parent-id and trace-flags It provides the essential information needed to trace a single request through multiple services.
* _Tracestate Header_: Optional. Allows for vendor-specific trace information to be included in a standardized way. It enables additional information to be carried along with the trace, which can be used by tracing systems for more detailed analysis.

## Example
### Client calls Service A
* Client creates trace-id: `4bf92f3577b34da6a3ce929d0e0e4736`
* Client creates parent-id: `00f067aa0ba902b7`
* Client calls Service A with HTTP header `traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01`

### Service A calls Service B
* Service A creates a new parent-id for the call to Service B: `b9c7c989f97918e1`
* Service A calls Service B with HTTP header `traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-b9c7c989f97918e1-01`

