# GSoC Review Checklist — Self-Check Before Sending for Review

Compiled from reviewer feedback (Ben Konyi, Samuel Rawlins, Kevin Moore) on the Milestone 1 CL (WebSocket timeline logging). 

## Code Style & Idioms
- [ ] Any string literal used more than once has a named constant instead (avoids typos, improves readability).
- [ ] Any set of related int/string "codes" (e.g. opcodes, directions) is modeled as an **enum**, not raw ints/strings, so names are derived from the enum instead of a separate lookup function.
- [ ] Repeated flags like a direction ("in"/"out") use `direction.name` from an enum rather than string literals scattered around.
- [ ] Used `Map` instead of `Expando` unless there's a specific reason (Expando has performance implications).
- [ ] No unnecessary indirection — if a helper method already checks an "enabled" flag internally, don't add an extra parameter/callback just to decide whether to call it.
- [ ] Switch statements that just return per-case are rewritten as switch **expressions** (`return switch (x) { ... }`).
- [ ] Removed unnecessary `break` statements (e.g. when using `switch` expressions or when they add no value).
- [ ] Nullable map entries use null-aware element syntax (`if (x != null) 'key': x` or `'key': ?x`) instead of manual conditional building.
- [ ] Fields/params that don't need to be public are made private (`_name`).
- [ ] Related logic lives in one class — don't split object/state management across multiple classes when it can stay in one (e.g. all timeline-object handling in a single logger class).
- [ ] Variable names are unambiguous, especially when two similar names coexist (e.g. `args` vs `arguments` → rename to `expectedArgs`/`actualArgs`); param names match what the API/test helper expects exactly.

## Comments & Documentation
- [ ] Doc comments use `///` (not `//`) for anything that should show up as API documentation.
- [ ] Doc comments reference variables using `[bracketed]` syntax so they render as links.
- [ ] Comments are placed directly above the declaration they describe, not squeezed inline before a return type/keyword.
- [ ] Multi-word "kind" labels used inside comments are quoted individually (`'type,' 'direction,'`) not run together.
- [ ] No typos — proofread comments specifically (e.g. "weather" vs "whether").
- [ ] Doc comment content actually matches the class/method it's attached to (move it down if it was written for the wrong scope).
- [ ] If adding new timeline events / instrumentation (or any new observable behavior with no public API change), check the CHANGELOG for precedent on whether this type of change is documented there.

## Testing
- [ ] Prefer a `CustomMatcher` over multiple ad hoc boolean assertions (`isTrue, isTrue`) — gives cleaner failure messages and simplifies call sites.
- [ ] Use built-in matcher helpers where they exist (e.g. `isNotEmpty`) instead of manual `expect(x.isNotEmpty, isTrue)`.
- [ ] Malformed/unexpected input in a helper throws an exception rather than silently `continue`-ing.
- [ ] Parameter names passed to shared test helpers match exactly what the helper expects (e.g. `testeeMain: testeeMain`).
- [ ] If a test fails intermittently, verify it's pre-existing flakiness (run it repeatedly, and also run it on untouched `main`) before assuming your change caused it — and say so explicitly in the CL comments if so.

## Error Handling & Async Correctness
- [ ] Any resource/task that gets "finished"/"closed" on the happy path is also finished/closed on failure paths — add `catchError`/`try-finally` around the whole operation, not just the success branch.
- [ ] If the same metric (e.g. payload size) is logged on both a send and receive path, double check both sides are measuring the same thing (e.g. both compressed or both uncompressed) — asymmetric logging is confusing later.

## Before Requesting Review / Landing
- [ ] All CI checks pass locally/in CI before pinging for a fresh review pass — don't ask for re-review on a red build.
- [ ] After fixing a build failure, explicitly trigger/confirm a rerun of checks rather than assuming.
- [ ] Confirm how many approvals ("+1"s) are needed to land, and call it out explicitly when asking to land (some repos need 2 reviewers, not 1).
- [ ] When following up on a stalled review, summarize what changed since the last look so the reviewer doesn't have to re-derive it.

## Quick Pre-Submit Pass (do this every patchset)
1. Grep your diff for repeated string literals → constants.
2. Grep for raw int/string "kind" values → consider an enum.
3. Check every `//` you added — should it be `///`?
4. Check every new async operation with a "finish"/"close" step — is there a failure path that skips it?
5. Run the full test suite twice if anything looks flaky, and note if it's flaky on `main` too.
6. Confirm CI is green before requesting review.
