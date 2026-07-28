# WillDo — Agent Instructions

> Status: Living Document  
> Purpose: This file defines how coding agents should work inside the WillDo repository.
>
> Product requirements are defined in `/docs`.
> Agents must follow those documents rather than inventing product behaviour for implementation convenience.

---

# 1. Project Overview

WillDo is a **capture-first AI product** built around:

> Future Action Capture (FAC)

WillDo helps users capture things they cannot deal with now but do not want to forget later.

The core product loop is:

```text
Capture
→ Understand
→ Review
→ Confirm
→ Remind
→ Resolve
```

WillDo is not primarily a task-management product.

Its differentiated value is reducing the effort between:

```text
"I need to deal with this later."
```

and:

```text
"It's captured and I trust that it will come back when it matters."
```

---

# 2. Source of Truth

Before implementing product behaviour, read the relevant documentation.

Core documents:

```text
docs/PRODUCT_BRIEF.md
docs/MVP_SCOPE.md
docs/DATA_SCHEMA.md
docs/AI_EXTRACTION_SPEC.md
docs/PRIVACY_RULES.md
docs/UX_FLOWS.md
docs/DECISIONS.md
docs/BUILD_PLAN.md
```

These documents define:

| Document | Purpose |
| --- | --- |
| `PRODUCT_BRIEF.md` | What WillDo is and why it exists |
| `MVP_SCOPE.md` | What is and is not part of the MVP |
| `DATA_SCHEMA.md` | Core product data contracts |
| `AI_EXTRACTION_SPEC.md` | AI behaviour and extraction rules |
| `PRIVACY_RULES.md` | Privacy and data-handling constraints |
| `UX_FLOWS.md` | Intended interaction flows |
| `DECISIONS.md` | Important product and technical decisions |
| `BUILD_PLAN.md` | Development phases and task sequence |

Do not silently contradict these documents.

If implementation exposes a genuine conflict:

```text
Stop
→ explain the conflict
→ propose options
→ make or request an explicit decision
→ update documentation if necessary
→ implement
```

Do not modify product direction merely because another implementation would be easier.

---

# 3. Product Guardrails

WillDo is:

```text
Capture-first
AI-assisted
Native mobile
Local-first
Account-free for MVP
Cloud-ready
User-controlled
Privacy-conscious
```

WillDo is not:

- a generic to-do app
- a Kanban board
- a project-management tool
- a habit tracker
- a productivity dashboard
- a notes application
- a calendar replacement
- a document vault
- an email assistant
- a financial-management platform
- a family-management platform
- a general-purpose AI assistant

Do not add generic productivity features unless they directly support:

```text
Capture
Understand
Review
Remind
Resolve
```

---

# 4. Core UX Rule

The primary UX principle is:

> Capture before structure.

Users should provide information first.

WillDo may then help structure it.

Do not require users to choose:

- category
- project
- priority
- task type
- folder
- complex reminder settings

before capture.

Prefer:

```text
User provides text / image / voice
→ AI proposes structure
→ user reviews
```

over:

```text
User fills structured task form
→ saves task
```

---

# 5. AI Product Rule

AI is an enabling layer, not the product identity.

WillDo should not feel like a chatbot.

The default AI interaction is:

```text
Capture
→ Processing
→ Structured result
```

not:

```text
User ↔ AI conversation
```

Conversation or clarification should appear only when meaningful ambiguity must be resolved.

---

# 6. AI Authority

AI output is always a proposal.

AI must not automatically:

- create a confirmed Reminder Card
- schedule a notification
- overwrite user-edited information
- invent missing dates
- invent locations
- invent amounts
- invent unsupported actions

The user remains authoritative.

The required flow is:

```text
Capture
→ AI Extraction
→ Review
→ User Confirm
→ Persist
→ Schedule
```

Once the user edits an AI-generated field, the user's value should be treated as authoritative.

---

# 7. AI Uncertainty

WillDo should prefer uncertainty over hallucination.

When important information is ambiguous:

```text
show uncertainty
or
ask a short clarification question
```

Do not silently guess an important field.

Prefer:

```text
When should you follow up?

[Thursday]
[Friday]
[Another date]
```

over a generic open-ended chatbot exchange.

Follow:

```text
docs/AI_EXTRACTION_SPEC.md
```

for extraction behaviour.

---

# 8. Core Data Boundaries

Do not merge the following concepts:

```text
CaptureInput
ExtractionResult
ReminderCard
```

They represent different lifecycle stages.

## CaptureInput

Temporary raw input.

## ExtractionResult

Temporary AI interpretation.

## ReminderCard

Confirmed persistent Future Action.

Use the canonical types defined in:

```text
docs/DATA_SCHEMA.md
```

Do not create duplicate competing domain models inside feature components.

---

# 9. Schema Stability

Do not change persistent product schema casually.

Before adding a field, ask:

1. Does the user need this when the Future Action returns later?
2. Does it support capture, understanding, reminding, or resolving?
3. Does it need to be persisted?
4. Is this a product need or simply something AI can extract?

Do not persist information merely because the AI model can detect it.

If a schema change is materially necessary:

```text
explain the reason
→ update DATA_SCHEMA.md
→ update affected specifications
→ record significant decisions in DECISIONS.md
→ implement
```

---

# 10. Time Model

Do not merge:

```text
targetAt
```

with:

```text
reminder.scheduledAt
```

They represent different concepts.

Example:

```text
Return deadline:
August 20

Reminder:
August 18
```

Also preserve date precision.

Do not convert:

```text
August 20
```

into:

```text
August 20 at 9:00 AM
```

and present that time as source information.

Product defaults and AI suggestions must remain distinguishable from extracted facts.

---

# 11. MVP Simplifications

For MVP v0.1:

```text
One Capture
→ One Primary Future Action
```

and:

```text
One Reminder Card
→ One Primary Reminder
```

Do not implement automatic multi-card extraction or complex multi-reminder systems unless the scope is explicitly changed.

---

# 12. Technology Direction

WillDo is a native mobile application.

Primary technology:

```text
React Native
Expo
TypeScript
Expo Router
```

The project should use native mobile interaction patterns rather than treating the app as a web page adapted to mobile.

The exact installed package versions are defined by:

```text
package.json
lockfile
```

Do not hard-code SDK version assumptions into product logic.

---

# 13. Expo Router

Use:

```text
Expo Router
```

as the primary application routing system.

Do not introduce a second competing routing architecture without a strong technical reason.

Routes should support future deep-linkable destinations such as:

```text
/reminders/[id]
```

which may later be opened by:

- notifications
- Share Sheet flows
- widgets
- external entry points

Routing decisions should preserve this capability where practical.

---

# 14. Development Build

Expo Go may be used for very early smoke testing.

WillDo should move to an:

```text
Expo Development Build
```

early in development.

Do not architect native features around Expo Go limitations.

This matters because WillDo will progressively use:

- notifications
- image access
- microphone access
- calendar access
- future native integrations

When reporting native verification, distinguish between:

```text
Expo Go
Simulator / Emulator
Development Build
Physical device
```

---

# 15. Incremental Development

Do not build the entire MVP in one step.

Follow the current sequence in:

```text
docs/BUILD_PLAN.md
```

Current phases:

```text
Phase 0
Project Foundation

Phase 1
Text Capture + Mock AI

Phase 2
Real Text AI

Phase 3
Image Capture

Phase 4
Voice Capture

Phase 5
Native Reminder Loop

Phase 6
Integration + UX Polish

Phase 7
MVP Validation
```

Do not implement later-phase features merely because they are convenient to add during an earlier task.

---

# 16. Local-First Architecture

The MVP does not require:

- authentication
- cloud Reminder Card database
- account recovery
- multi-device sync

Confirmed Reminder Cards are initially persisted locally.

However:

> Local-first must remain cloud-ready.

UI components must not directly depend on the underlying storage technology.

---

# 17. Primary Local Persistence

The initial Reminder Card persistence implementation should use:

```text
expo-sqlite
```

through:

```text
SQLiteCardRepository
```

The intended boundary is:

```text
UI / Application Logic
↓
CardRepository
↓
SQLiteCardRepository
↓
expo-sqlite
```

Do not use:

```text
AsyncStorage
```

as the primary database for Reminder Cards.

AsyncStorage or other simple key-value persistence may still be appropriate for small settings or non-domain preferences if a concrete need appears.

---

# 18. Repository Rule

All persistent Reminder Card operations must go through:

```ts
CardRepository
```

The UI must not directly:

- execute SQL
- access SQLite tables
- access future Supabase tables
- depend on storage-specific row shapes

Use the repository abstraction defined in:

```text
docs/DATA_SCHEMA.md
```

Conceptually:

```text
Today:

UI
↓
CardRepository
↓
SQLiteCardRepository
↓
expo-sqlite
```

Future:

```text
UI
↓
CardRepository
↓
Cloud / Sync implementation
↓
Supabase or another backend
```

The UI contract should remain stable.

---

# 19. Domain Model vs Database Model

Do not assume:

```text
ReminderCard
=
SQLite row
```

The persistent representation may differ from the domain model.

The repository is responsible for mapping between:

```text
Database representation
↔
Domain objects
```

where necessary.

Database implementation details should not leak into feature UI.

---

# 20. SQLite Scope

Use SQLite for structured local Reminder Card persistence.

Do not introduce an ORM during the initial MVP unless there is a concrete demonstrated need.

Do not automatically add:

```text
Drizzle
Prisma
Realm
WatermelonDB
```

for speculative future complexity.

Start with:

```text
expo-sqlite
+
SQLiteCardRepository
```

If database complexity later justifies an ORM, make that decision explicitly.

---

# 21. Extraction Service Rule

The UI must not directly call an AI provider.

Use:

```ts
ExtractionService
```

The intended implementations include:

```text
MockExtractionService
RealAIExtractionService
```

Both must return compatible:

```ts
ExtractionResult
```

structures.

The Review UI should not know:

- which model was used
- which AI provider was used
- what prompt was used
- which backend delivered the result

---

# 22. Phase 2 AI Backend

The initial real-AI backend direction is:

```text
WillDo App
↓
ExtractionService
↓
Supabase Edge Function
↓
AI Provider
```

Supabase is initially used to provide protected server-side AI access.

This does not automatically put the following into MVP scope:

```text
Supabase Auth
Supabase Postgres Reminder Card storage
Cloud sync
Multi-device sync
```

Do not expand Supabase usage beyond the current task simply because the infrastructure already exists.

---

# 23. AI Credentials

Never embed reusable private AI provider credentials in the mobile client.

Do not write:

```ts
const AI_API_KEY = "...";
```

inside distributed application code.

Private provider secrets belong in protected server-side infrastructure.

Phase 1 mock development does not require AI backend infrastructure.

---

# 24. Runtime Validation

Use:

```text
Zod
```

at untrusted runtime boundaries where TypeScript alone cannot guarantee the shape of received data.

Important examples include:

- AI structured responses
- backend responses
- future persisted-data migrations
- external payloads

Conceptually:

```text
Untrusted external data
↓
Zod validation
↓
Trusted domain structure
↓
Application logic
```

Do not treat:

```ts
externalResponse as ExtractionResult
```

as sufficient runtime validation.

---

# 25. Zod Scope

Zod is intended for runtime trust boundaries.

Do not unnecessarily create Zod schemas around every internal object.

Use:

```text
TypeScript
```

for normal internal static typing.

Use:

```text
Zod
```

when runtime data may not match the expected TypeScript shape.

---

# 26. Notification Service Rule

Native notification behaviour should be isolated behind:

```ts
NotificationService
```

where practical.

Intended implementation:

```text
Application Logic
↓
NotificationService
↓
ExpoNotificationService
↓
expo-notifications
```

Responsibilities may include:

```text
schedule
cancel
reschedule
```

Do not scatter direct Expo Notifications API calls across unrelated screens.

---

# 27. Notification Lifecycle

When reminder timing changes:

```text
Cancel old notification
→ schedule replacement
→ persist updated state
```

When a card becomes done:

```text
Cancel future scheduled notification
```

When a card is deleted:

```text
Cancel associated notification
→ delete card
```

Do not leave orphan native notifications scheduled.

---

# 28. Native Capability Direction

The current implementation direction is:

```text
Navigation
→ Expo Router

Local persistence
→ expo-sqlite

Image selection / camera
→ expo-image-picker

Voice recording
→ expo-audio

Local notifications
→ expo-notifications

Calendar integration
→ expo-calendar
```

These are deliberate current implementation choices.

Do not substitute another major library casually.

If implementation evidence shows a listed option is unsuitable:

```text
explain why
→ propose replacement
→ evaluate architecture impact
→ update DECISIONS.md if material
→ implement
```

---

# 29. State Management

Do not introduce a global state-management library by default.

Start with:

```text
React local state
Custom hooks
Repository / Service abstractions
```

Do not add:

```text
Redux
Zustand
MobX
TanStack Query
```

simply because they are common in larger applications.

Introduce state-management infrastructure only when an actual complexity problem appears.

---

# 30. Privacy Rules

Follow:

```text
docs/PRIVACY_RULES.md
```

as an implementation constraint.

Core rules include:

- capture is user-initiated
- no email scanning
- no bank integration
- no automatic photo-library scanning
- no background clipboard monitoring
- no continuous microphone listening
- no automatic device monitoring
- persist minimum useful information
- allow user deletion

Privacy requirements must not be bypassed for implementation convenience.

---

# 31. Raw Image Handling

Original images are temporary Capture inputs by default.

Do not automatically add:

```text
originalImage
imageArchive
attachmentLibrary
```

to persistent Reminder Cards.

The intended flow is:

```text
Select image
→ process
→ review
→ confirm
→ persist Future Action
→ discard unnecessary source image
```

Persistent source-image retention requires an explicit product and privacy decision.

---

# 32. Voice Handling

Voice is a Capture mechanism, not a recording-storage feature.

The intended flow is:

```text
Record
→ transcribe
→ extract
→ review
→ confirm
→ discard unnecessary raw audio
```

Do not retain raw voice recordings by default.

---

# 33. Logging

Never log raw private Capture data by default.

Avoid production logging such as:

```ts
console.log(captureInput);
console.log(aiPrompt);
console.log(aiResponse);
console.log(reminderCard);
```

when those objects may contain real user content.

Prefer metadata:

```ts
console.log("Extraction started", {
  sourceType,
  requestId,
});
```

Development-only debugging that exposes content must not remain in production builds.

---

# 34. Analytics

Analytics should measure behaviour, not private content.

Acceptable examples:

```text
capture_started
capture_submitted
extraction_completed
review_edited
reminder_confirmed
notification_opened
reminder_completed
```

Do not send private values such as:

```text
title
nextAction
context
location
amount
originalText
voiceTranscript
```

as analytics properties unless a future explicit privacy decision allows it.

---

# 35. Native Permissions

Request permissions contextually.

Do not ask for:

```text
Camera
Photos
Microphone
Notifications
Calendar
```

all during first launch.

Request each permission only when the user attempts the related capability.

Permission denial should degrade gracefully.

Example:

```text
Microphone denied
→ Text and Image remain usable
```

---

# 36. Error Handling

Do not silently fail.

Important async operations should expose meaningful states such as:

```text
idle
loading
success
error
```

where appropriate.

User input should not disappear unnecessarily after recoverable errors.

AI failure must provide a manual fallback.

Expected pattern:

```text
Couldn't understand this capture.

[Try Again]
[Enter Manually]
```

WillDo should remain useful when AI is unavailable.

---

# 37. Offline Behaviour

Because WillDo is local-first, existing Reminder Cards should remain available offline where practical.

Operations that do not inherently require network access should continue to work.

Examples:

- view Inbox
- open Card
- edit Card
- mark Done
- delete Card

Real AI extraction may require connectivity.

Do not block access to existing SQLite data because AI or Supabase is unavailable.

---

# 38. UX Complexity Rule

Do not add complex management UX unless explicitly required.

Avoid default introduction of:

- tags
- projects
- priority systems
- nested lists
- folders
- productivity scores
- analytics dashboards
- complex recurring-task builders

Ask:

> Does this reduce the effort required to capture, remember, or resolve a Future Action?

If not, it is probably outside the core product.

---

# 39. Component Design

Prefer:

- small focused components
- clear prop contracts
- reusable primitives when there is real reuse
- feature-local logic when behaviour is feature-specific

Avoid:

- large monolithic screens
- premature generic abstractions
- global state for everything
- abstractions created only for hypothetical future features

Use abstraction deliberately where it protects an established boundary, especially:

```text
Storage
AI
Notifications
```

---

# 40. TypeScript

Use TypeScript consistently.

Avoid:

```ts
any
```

unless there is a specific unavoidable reason.

Prefer:

- explicit shared domain types
- discriminated unions where useful
- accurate nullability
- runtime validation at external boundaries

Do not silently cast external data into trusted domain types.

---

# 41. External Data Validation

Treat data from:

- AI
- backend APIs
- native APIs
- future cloud APIs
- future migrated local data

as potentially untrusted.

Validate where necessary before relying on it.

AI structured output must follow:

```text
docs/AI_EXTRACTION_SPEC.md
```

Invalid data should cause a controlled failure instead of corrupting persistent product state.

---

# 42. Dependency Rule

Do not introduce a dependency without a concrete reason.

Before adding one, consider:

1. What problem does it solve?
2. Is it required in the current phase?
3. Does Expo or React Native already provide the capability?
4. What maintenance cost does it introduce?
5. Is the package actively maintained?

Current deliberate dependencies include:

```text
Expo Router
expo-sqlite
Zod
```

and later phase-specific Expo modules.

Additional dependencies are not forbidden.

They require justification.

---

# 43. Refactoring Rule

Do not perform unrelated large refactors during a focused task.

Refactoring is appropriate when it:

- removes relevant duplication
- restores architectural boundaries
- improves maintainability of the current feature
- fixes bugs
- supports the current phase

Avoid speculative architecture for:

```text
future enterprise scale
future teams
future web version
future multi-region infrastructure
```

Cloud readiness should come primarily from clean interfaces rather than premature cloud systems.

---

# 44. Scope Control

Before implementing a feature, confirm that it belongs to:

```text
docs/MVP_SCOPE.md
```

or:

```text
docs/BUILD_PLAN.md
```

If not, do not implement it automatically.

If a useful adjacent feature is discovered:

```text
report it
→ explain potential value
→ do not implement unless approved
```

---

# 45. Documentation Change Rule

When implementation intentionally changes an established product or technical decision, update documentation.

Examples include changes to:

```text
ReminderCard schema
AI extraction behaviour
privacy model
MVP scope
core UX flow
local persistence architecture
routing architecture
AI backend architecture
```

Significant changes should also be recorded in:

```text
docs/DECISIONS.md
```

Code and documentation should not knowingly contradict each other.

---

# 46. Task Scope

Each Codex task should be small and independently reviewable.

Good:

```text
Implement SQLiteCardRepository.
```

Good:

```text
Build editable Review screen for mock ExtractionResult.
```

Bad:

```text
Build Phase 1.
```

Bad:

```text
Finish the MVP.
```

A task should usually be small enough to:

```text
understand
→ implement
→ verify
→ review
→ commit
```

in one coherent change.

---

# 47. Before Starting a Task

Before making changes:

1. Read `AGENTS.md`.
2. Read the task description.
3. Read relevant `/docs` files.
4. Inspect existing implementation.
5. Identify established patterns to reuse.
6. Confirm the task belongs to the current phase.
7. Identify product or architecture conflicts before coding.

Do not immediately generate code before understanding the repository.

---

# 48. During a Task

While implementing:

- keep changes focused
- preserve unrelated existing behaviour
- follow established domain types
- follow repository and service boundaries
- do not expand scope silently
- handle loading and error states
- consider empty states where relevant
- consider permission denial where relevant
- consider privacy implications when new data is processed
- avoid unnecessary dependencies

If blocked by an important ambiguity, surface it rather than inventing product behaviour.

---

# 49. Verification

Before declaring a task complete, run the relevant available checks.

These may include:

```text
TypeScript
Lint
Tests
Expo development launch
SQLite persistence check
Manual flow verification
```

Do not state that something works if it was not verified.

For native features such as:

- camera
- microphone
- notifications
- calendar

physical-device testing may be required.

Clearly state when verification was limited to:

```text
static checks
simulator / emulator
development build
physical device
```

---

# 50. Task Completion Report

When completing a coding task, summarise:

## What changed

Briefly explain the implementation.

## Files changed

List important files created or modified.

## Verification

Explain which checks were actually run.

## Remaining limitations

State anything intentionally incomplete.

## Decisions / assumptions

Mention new assumptions or conflicts that may require product review.

Do not hide unresolved problems behind a successful-sounding completion message.

---

# 51. Build Plan Compliance

Development should follow:

```text
docs/BUILD_PLAN.md
```

The next task should normally come from the current phase.

Do not skip to later-phase functionality unless:

- earlier dependencies are complete
- or there is an explicit reason to change sequence

Major sequencing changes should be intentional.

---

# 52. Testing Priorities

Testing should focus on product-critical boundaries.

## Data

```text
Repository CRUD
SQLite persistence
Schema validity
Data mapping
```

## AI

```text
Structured output validation
Clear input
Ambiguous input
Missing dates
Relative dates
Extraction failure
```

## Reminder lifecycle

```text
Schedule
Edit
Cancel
Done
Delete
```

## UX

```text
Capture
Review
Confirm
Recover from failure
```

Do not pursue abstract coverage numbers at the expense of testing the core product loop.

---

# 53. Physical Device Rule

Because WillDo depends on:

- camera
- microphone
- local notifications
- permissions
- native mobile interaction

major milestones should eventually be tested on a physical mobile device.

Do not rely exclusively on:

```text
web preview
Expo Go
simulator
emulator
```

for validating the final native experience.

---

# 54. Accessibility

Consider accessibility while building components.

Important areas include:

- touch target size
- screen reader labels
- text scaling
- readable hierarchy
- colour-independent status
- clear error messaging
- keyboard behaviour
- reduced motion where appropriate

Do not deliberately create inaccessible UI to match a visual mockup.

---

# 55. Visual Design

Until a formal design system is defined:

- keep UI simple
- prioritise hierarchy
- prioritise touch usability
- avoid unnecessary visual complexity
- avoid premature design-system infrastructure
- do not add decorative interaction that slows Capture

Visual polish should not override Capture speed or Review clarity.

---

# 56. Current Product Hypotheses

The following are working assumptions, not permanent facts:

- young adults are the primary initial audience
- Future Action Capture moments occur frequently
- text, image, and voice all provide meaningful value
- one Capture → one Future Action is sufficient for MVP
- one Card → one Reminder is sufficient for MVP
- Inbox is the right management model
- native notifications provide sufficient reminder visibility
- users will accept a lightweight Review step
- AI will meaningfully reduce Capture friction
- original source retention is not required by default

Do not encode these assumptions so deeply that they become impossible to test or revise.

---

# 57. Product Metrics Mindset

When implementation allows measurement, prefer metrics that test the product hypothesis.

Important concepts include:

```text
Time to Confirm
Review Effort
Capture Abandonment
Clarification Rate
Reminder Completion
```

Do not optimise primarily for:

```text
number of AI calls
number of extracted fields
number of productivity features
```

The goal is lower user effort and better follow-through.

---

# 58. Security and Privacy Red Flags

Stop and raise the issue before implementing anything involving:

- reusable AI secrets in client code
- automatic email scanning
- bank integrations
- continuous background monitoring
- automatic screenshot scanning
- persistent storage of all source captures
- raw capture content in analytics
- raw personal content in production logs
- high-sensitivity document workflows
- major new external data integrations

These materially change WillDo's privacy model.

---

# 59. Architecture Red Flags

Stop and raise the issue before introducing:

- direct SQLite access from screens
- direct AI-provider calls from screens
- direct notification scheduling scattered through UI
- AsyncStorage as the main Reminder Card database
- a second routing framework
- ORM adoption without demonstrated database complexity
- Redux / Zustand / MobX without demonstrated state-management need
- Supabase Auth / Postgres merely because Edge Functions are already used

These may indicate architecture drift or premature complexity.

---

# 60. North Star

Before making a significant implementation decision, ask:

> Does this make it easier for users to capture something they need to deal with later and trust that it will return when it matters?

If the implementation primarily makes WillDo a more complicated task manager, it is probably the wrong direction.

---

# 61. Final Agent Rule

Do not optimise for:

> building the most features.

Do not optimise for:

> using the most sophisticated architecture.

Optimise for:

> building the smallest reliable version of the intended WillDo experience.

The quality of the Future Action Capture loop matters more than feature count.