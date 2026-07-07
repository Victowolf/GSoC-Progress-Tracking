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
| Milestone 2 — VM Service Integration | 🔄 In Progress |
| Overall Progress | **On Schedule ✅** |

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

> **In Progress 🔄**

---

## Community Interactions
Throughout the coding period, valuable feedback from mentors and Dart SDK contributors helped improve the implementation, testing strategy, documentation quality, coding style, performance considerations, and overall maintainability of the project. Multiple rounds of review resulted in refinements that aligned the implementation with existing Dart SDK conventions and ensured production-quality contributions.

- Samuel Rawlins
- Hangyu Jin
- Brian Quinlan
- Ben Konyi
- Kevin Moore

# Thank you..!!
