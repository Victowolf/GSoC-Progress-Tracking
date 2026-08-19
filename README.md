# Google Summer of Code 2026

## Project Title

**Add WebSocket/gRPC Support to the Flutter DevTools Network Panel**

---

## Project Description

Modern Flutter and Dart applications rely heavily on WebSockets for real-time communication, yet the DevTools Network panel currently provides visibility primarily into HTTP traffic. This project extends the existing Dart SDK networking infrastructure to enable WebSocket profiling throughout the complete tooling stack.

The implementation introduces lightweight timeline instrumentation inside `dart:io`, integrates WebSocket profiling with the existing VM Service network profiling workflow, and lays the foundation for exposing WebSocket traffic inside Flutter DevTools without introducing additional APIs or profiling flags.

The project spans three major components:

- Dart SDK (`dart:io`)
- Dart VM Service
- Flutter DevTools Network Panel

The overall objective is to allow developers to inspect WebSocket communication with the same workflow currently used for HTTP requests while maintaining minimal runtime overhead.

---

# Progress Summary

| Phase | Status |
|--------|--------|
| Community Bonding | ✅ Completed |
| Milestone 1 — WebSocket Timeline Instrumentation | ✅ Completed & Merged |
| Milestone 2 — VM Service Integration | ✅ Completed & Merged |
| Milestone 3 — WebSocket Profiling & VM Service Support | ✅ Completed & Merged                         |
| Milestone 4 — DevTools Network Panel Integration       | 🟡 Implementation Complete — PR Under Review |
| Overall Progress                                       | **On Schedule ✅**                            |

---

# Community Bonding Period

### Timeline

`06 May 2026`
- Official GSoC coding period started
- Project planning finalized
- Initial implementation roadmap prepared

`09 May 2026`
- Development environment setup
- Dart SDK cloned and configured locally

`11 May 2026`
- Began work on SDK Issue #45733
- Investigated existing implementation
- Submitted initial CL

`13–16 May 2026`
- Deep investigation of `WebSocket.addError`
- Studied stream semantics and error propagation

`19–21 May 2026`
- Documentation improvements
- Added extensive validation tests
- Refined understanding of WebSocket behavior

`21–29 May 2026`
- Review phase
- Executed all tests
- Addressed review feedback
- Final CL merged

### Explanation

The Community Bonding period focused on preparing a strong technical foundation before beginning the primary project milestones.

The first task involved setting up the complete Dart SDK development environment and understanding the repository structure, build system, and testing infrastructure.

Before implementing new functionality, work began on SDK Issue #45733. This provided an opportunity to become familiar with the contribution workflow used by the Dart SDK while gaining experience with the code review process, Continuous Integration, and Gerrit-based development. Additional documentation improvements and comprehensive test cases were contributed to validate the expected behavior across multiple scenarios.

The Community Bonding period concluded successfully with all reviews completed and the change merged into the Dart SDK.

### Submitted CL

> *[Gerrit Change Link](https://dart-review.googlesource.com/c/sdk/+/502500)*

### Tracking Issue

> *[Issue Link](https://github.com/dart-lang/sdk/issues/45733)*

> *[Tracking Issue Link](https://github.com/Victowolf/GSoC-Progress-Tracking/issues/2)*

### Status
> **Merged ✅**

---

# Milestone 1 — Enable WebSocket Timeline Logging in Dart

### Timeline

`08 May 2026`
- Investigated existing SDK issues
- Studied Timeline APIs
- Researched profiling architecture

`11 May 2026`
- Finalized implementation strategy
- Designed instrumentation architecture

`05 June 2026`
- Identified instrumentation hooks
- Mapped WebSocket lifecycle events

`07 June 2026`
- Implemented timeline instrumentation
- Added VM Service integration tests
- Submitted CL for review

`08–23 June 2026`
- Addressed review comments
- Improved implementation
- Passed all builders
- CL merged

### Explanation

Milestone 1 introduced comprehensive WebSocket timeline instrumentation into `dart:io`, forming the backend telemetry required for WebSocket inspection in Flutter DevTools.
Rather than introducing a new profiling mechanism, the implementation extends the existing `HttpClient.enableTimelineLogging` infrastructure so that WebSocket traffic automatically participates in Dart's existing network profiling workflow.

The instrumentation captures the complete WebSocket lifecycle, including:

- Connection establishment
- HTTP upgrade handshake
- Message send
- Message receive
- Ping/Pong frames
- Connection closure
- Runtime and protocol errors

A dedicated `_WebSocketTimelineLogger` helper was introduced to isolate all instrumentation logic from protocol implementation, improving maintainability while preserving existing behavior.
Timeline events emit lightweight metadata—including connection identifiers, direction, opcode type, payload size, close information, and error details—without serializing payload contents, ensuring minimal runtime overhead.

Comprehensive VM Service integration tests were added to verify the emission of:

- `WebSocket.Connect`
- `WebSocket.Send`
- `WebSocket.Receive`
- `WebSocket.Close`

The milestone concluded successfully with all tests passing and the implementation merged into the Dart SDK.

### Submitted CL

> *[Gerrit Change Link](https://dart-review.googlesource.com/c/sdk/+/509860)*

### Tracking Issue

> *[Tracking Issue Link](https://github.com/Victowolf/GSoC-Progress-Tracking/issues/5)*

### Status

> **Merged ✅**

---

# Milestone 2 — VM Service Integration for WebSocket Profiling

### Timeline

`25 June 2026`
- Updated implementation strategy
- Planned documentation updates
- Planned VM Service integration tests
- Planned DevTools compatibility validation

`03 July 2026`
- Verified Milestone 1 coverage
- Identified required VM Service verification

`04 July 2026`
- Reviewed existing HTTP timeline tests
- Verified existing WebSocket timeline tests already validate the complete pipeline

`06 July 2026`
- Updated `dart:io` documentation
- Updated VM Service documentation
- Submitted documentation CL

### Explanation

Milestone 2 focused on integrating WebSocket profiling into Dart's existing VM Service network profiling workflow.

Instead of introducing a new service extension or additional profiling flags, the implementation extends the existing `ext.dart.io.HttpClient.enableTimelineLogging` workflow so that enabling HTTP timeline logging automatically enables WebSocket timeline instrumentation.

During this milestone, the complete profiling pipeline was analyzed:

```
   DevTools
      │
      ▼
VM Service
      │
httpEnableTimelineLogging(true)
      │
      ▼
HttpClient.enableTimelineLogging = true
      │
      ▼
HTTP + WebSocket Timeline Events
```

Existing HTTP VM Service tests were reviewed alongside the WebSocket timeline tests introduced in Milestone 1.

This confirmed that the instrumentation already validated the complete runtime pipeline from the VM Service through `HttpClient.enableTimelineLogging` to WebSocket timeline emission.

The remaining work therefore focused on updating documentation to accurately describe the new behavior.

Documentation across `dart:io`, VM Service, and service extensions was updated to clearly state that enabling HTTP timeline logging now enables timeline logging for both HTTP and WebSocket traffic.

This milestone preserves complete backward compatibility while extending existing profiling capabilities without requiring changes to DevTools activation workflows.

### Submitted CL

> *[Gerrit Change Link](https://dart-review.googlesource.com/c/sdk/+/520800)*

### Tracking Issue

> *[Tracking Issue Link](https://github.com/Victowolf/GSoC-Progress-Tracking/issues/6)*

### Status

> **Merged ✅**

---

# Milestone 3 — WebSocket Profiling & VM Service Support

### Timeline

`08 July 2026`

* Started implementation of the WebSocket profiling infrastructure
* Designed the profiler architecture based on existing HTTP profiling

`16–20 July 2026`

* Implemented the WebSocket profiler
* Implemented profile data and profile events
* Worked on VM Service integration
* Added support for connection-level and frame-level profiling data

`21–25 July 2026`

* Added and refined VM Service tests
* Defined the WebSocket profiling data exposed to DevTools
* Finalized connection and frame-level metadata

`26 July 2026`

* Completed code formatting and cleanup
* Submitted CL for review

`27 July – 12 August 2026`

* Addressed VM Service review comments
* Updated the `vm_service` package SDK constraints
* Resolved build and CBuild issues
* Refined the implementation based on review feedback

`13 August 2026`

* CL merged into the Dart SDK

### Explanation

Milestone 3 introduced the WebSocket profiling infrastructure required to aggregate connection-level and frame-level WebSocket activity and expose it through the existing Dart VM Service profiling architecture.

A dedicated `WebSocketProfiler` was implemented to continuously maintain profiling information for active and completed WebSocket connections. The profiler uses connection identifiers to maintain stable profiles across multiple updates and stores structured information about the lifecycle and activity of each connection.

The profiling architecture consists of:

* `WebSocketProfiler`
* `_WebSocketProfileData`
* `_WebSocketProfileEvent`

The implementation follows the existing HTTP profiling architecture where appropriate while adapting it for persistent WebSocket connections.

Each WebSocket connection records information including:

* Connection ID
* Isolate ID
* URI
* Start and end timestamps
* Connection state
* Bytes sent and received
* Frames sent and received
* Ping/Pong counts
* Close code and reason
* Error information
* Last updated timestamp

Individual WebSocket events are also recorded, allowing DevTools to inspect the activity occurring over a persistent connection.

The recorded event types include:

* Connect
* Open
* Send
* Receive
* Ping
* Pong
* Close
* Error

Each event contains structured metadata such as:

* Frame/event number
* Timestamp
* Connection ID
* URI
* Direction
* Event type
* Opcode
* Payload size
* Error type
* Error message

The profiler was integrated with VM Service so that WebSocket profiling data can be retrieved, updated incrementally, looked up by connection, and cleared using the existing profiling workflow.

VM Service tests were added to validate:

* Profile creation
* Connection lookup
* Frame/event aggregation
* Profile clearing
* VM Service serialization
* WebSocket profiling data retrieval

The implementation was reviewed through multiple iterations, including changes to SDK constraints and build configuration. After addressing the review feedback and resolving build issues, the CL was successfully merged into the Dart SDK.

### Submitted CL

> *[Gerrit Change Link](https://dart-review.googlesource.com/c/sdk/+/527880)*

### Tracking Issue

> *[Tracking Issue Link](https://github.com/Victowolf/GSoC-Progress-Tracking/issues/7)*

### Status

> **Merged ✅**

---

# Milestone 4 — WebSocket Integration into the Flutter DevTools Network Panel

### Timeline

`14 August 2026`

* Designed the DevTools UI mapping for WebSocket connections
* Mapped WebSocket profiling data to existing Network panel fields

`15 August 2026`

* Finalized the DevTools integration architecture
* Designed WebSocket Network models
* Planned Network Service and Controller integration
* Designed Overview and Frames inspection views

`16–19 August 2026`

* Implemented WebSocket support in the DevTools Network panel
* Integrated WebSocket profiling data with the existing Network polling workflow
* Added WebSocket Network models
* Implemented WebSocket Overview and Frames inspection
* Extended tests
* Submitted PR for review

### Explanation

Milestone 4 focuses on bringing the WebSocket profiling data collected by the Dart SDK and exposed through VM Service into the Flutter DevTools Network panel.

The implementation reuses the existing Network infrastructure instead of introducing a separate WebSocket screen or controller. WebSocket connections are represented as `NetworkRequest` implementations and therefore participate in the same Network table, polling, filtering, selection, and inspection workflows used by existing network traffic.

The overall data flow is:

```text
Dart SDK
   │
   ▼
WebSocketProfiler
   │
   ▼
VM Service
   │
   ▼
getWebSocketProfile()
   │
   ▼
NetworkService
   │
   ▼
NetworkController
   │
   ▼
CurrentNetworkRequests
   │
   ▼
NetworkScreen
   │
   ├── Overview
   │
   └── Frames
```

### WebSocket Network Integration

WebSocket connections are represented directly in the existing Network request table.

The WebSocket model maps profiling information to the standard Network fields:

| Network Field | WebSocket Value             |
| ------------- | --------------------------- |
| Method        | `websocket`                    |
| Address       | WebSocket URI               |
| Status        | Connection state            |
| Duration      | Connection duration         |
| Size          | Total bytes sent + received |
| Timestamp     | Connection start timestamp  |

The WebSocket model maintains the connection ID as its stable identity, allowing subsequent profiling updates to modify the existing Network entry rather than creating duplicate entries.

### WebSocket Inspection

Selecting a WebSocket connection opens a dedicated inspection experience containing:

```text
WebSocket
   ├── Overview
   └── Frames
```

The **Overview** section exposes connection-level information including:

* URI
* Connection ID
* Connection state
* Protocol
* Bytes sent
* Bytes received
* Frames sent
* Frames received
* Ping count
* Pong count
* Start time
* End time
* Duration
* Last updated timestamp

The **Frames** section displays individual WebSocket profiling events.

Each event contains:

* Frame number
* Direction
* Event type
* Opcode
* Payload size
* Timestamp
* Error information when applicable

Supported event types include:

* Connect
* Open
* Send
* Receive
* Ping
* Pong
* Close
* Error

Selecting an individual event provides expanded details including the connection ID, URI, direction, opcode, payload size, timestamp, and error information.

### Testing

Tests were extended to cover the DevTools WebSocket integration, including:

#### Model

* WebSocket `NetworkRequest` implementation
* Stable connection identity
* URI and connection state mapping
* Statistics mapping
* Timestamp and duration handling
* Updating existing connections
* Retaining new WebSocket events
* Listener notifications when profile data changes

#### Network Integration

* WebSocket profile retrieval
* Incremental `updatedSince` polling
* Adding new WebSocket connections
* Updating existing connections without duplication
* Clearing WebSocket profiling data
* Maintaining selection across updates

#### UI

* WebSocket entries in the Network table
* WebSocket Overview
* WebSocket Frames tab
* Frame/event rendering
* Separate Event and Opcode fields
* Error information rendering
* Live updates to Overview and Frames

### Submitted PR

> *[Github PR Link](https://github.com/flutter/devtools/pull/9968)* 

### Tracking Issue

> *[Tracking Issue Link](https://github.com/Victowolf/GSoC-Progress-Tracking/issues/8)*

### Status

> **In Review 🟡**

---

# What's Remaining

With the core WebSocket profiling pipeline implemented across the Dart SDK, VM Service, and DevTools, the remaining work focuses on extending ecosystem support, implementing planned WebSocket enhancements, and progressing toward gRPC profiling.

### 1. Third-Party WebSocket Package Support

Extend WebSocket profiling support to third-party Dart packages including `cupertino_http`, `web_socket`, and `web_socket_channel`. These packages provide alternative or wrapper-based WebSocket implementations, so integration will allow their traffic to participate in the same DevTools profiling workflow.

### 2. WebSocket Enhancements

Implement additional WebSocket profiling enhancements where appropriate, including improved lifecycle tracking, connection-level summaries, and handling of high-volume WebSocket traffic. These improvements will further enhance the usability and debugging experience of WebSocket profiling in DevTools.

### 3. gRPC Support

Begin implementation of gRPC profiling as the next protocol-level extension. The initial work will focus on client-side timeline instrumentation and structured RPC profiling, followed by exploring VM Service and DevTools integration based on the existing gRPC profiling architecture and mentor guidance.

---

## Community Interactions
Throughout the coding period, valuable feedback from mentors and Dart SDK contributors helped improve the implementation, testing strategy, documentation quality, coding style, performance considerations, and overall maintainability of the project. Multiple rounds of review resulted in refinements that aligned the implementation with existing Dart SDK conventions and ensured production-quality contributions.

- Samuel Rawlins
- Hangyu Jin
- Brian Quinlan
- Ben Konyi
- Kevin Moore
- Alexander Aprelev

# Thank you..!!
