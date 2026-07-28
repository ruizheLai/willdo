# WillDo — Agent Instructions

> Status: Living Document  
> Purpose: This file defines how coding agents should work inside the WillDo repository.
>
> Product requirements are defined in `/docs`.
> Agents must follow those documents rather than inventing product behaviour for implementation convenience.

---

# 1. Project Overview

WillDo is a **capture-first AI product** built around the concept of:

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
| `AI_EXTRACTION_SPEC.md` | AI behaviour and structured extraction rules |
| `PRIVACY_RULES.md` | Privacy and data-handling constraints |
| `UX_FLOWS.md` | Intended user interaction flows |
| `DECISIONS.md` | Important product and technical decisions |
| `BUILD_PLAN.md` | Development phases and task sequence |

Do not silently contradict these documents.

If implementation exposes a conflict:

```text
Stop
→ explain the conflict
→ propose options
→ wait for or make an explicit documented decision
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
- invent actions unsupported by the capture

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

Once a user edits an AI-generated field, the user's value should be treated as authoritative.

---

# 7. AI Uncertainty

WillDo should prefer uncertainty over hallucination.

When information is ambiguous:

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

over a generic chatbot question.

Follow:

```text
docs/AI_EXTRACTION_SPEC.md
```

for extraction behaviour.

---

# 8. Core Data Boundaries

Do not merge the following concepts into one object:

```text
CaptureInput
ExtractionResult
ReminderCard
```

They represent different lifecycle stages.

### CaptureInput

Temporary raw input.

### ExtractionResult

Temporary AI interpretation.

### ReminderCard

Confirmed persistent Future Action.

Use the canonical types defined in:

```text
docs/DATA_SCHEMA.md
```

Do not create duplicate competing versions of the same types inside feature components.

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
→ then implement
```

---

# 10. Time Model

Do not merge:

```text
targetAt
```

and:

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

and represent that as source information.

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
```

Use native mobile patterns rather than designing the application as a web page adapted to mobile.

The application should eventually support:

- native local notifications
- image capture / selection
- microphone access
- calendar integration
- system-level capture features

but these should be implemented according to the phases in `BUILD_PLAN.md`.

---

# 13. Incremental Development

Do not build the entire MVP in one step.

Follow the current development order:

```text
Phase 0
Project foundation

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

Do not implement later-phase features merely because they are easy to add while working on an earlier task.

---

# 14. Local-First Architecture

The MVP does not require:

- authentication
- cloud database
- account recovery
- cross-device sync

Confirmed Reminder Cards are initially persisted locally.

However:

> Local-first must remain cloud-ready.

UI components must not directly depend on a storage technology.

---

# 15. Repository Rule

All persistent Reminder Card access must go through:

```ts
CardRepository
```

The UI must not directly:

- read AsyncStorage
- write AsyncStorage
- access SQLite
- access future Supabase tables

or depend on another persistence implementation.

Use the repository abstraction defined in:

```text
docs/DATA_SCHEMA.md
```

Conceptually:

```text
UI
↓
CardRepository
↓
LocalCardRepository
```

Future:

```text
UI
↓
CardRepository
↓
SupabaseCardRepository
```

The UI contract should remain stable.

---

# 16. Extraction Service Rule

The UI must not directly call an AI provider.

Use:

```ts
ExtractionService
```

The initial implementations may include:

```text
MockExtractionService
RealAIExtractionService
```

Both must return compatible:

```ts
ExtractionResult
```

structures.

The Review UI should not know which model or provider produced the result.

---

# 17. AI Credentials

Never embed reusable private AI provider credentials in the mobile client.

Do not:

```ts
const OPENAI_API_KEY = "...";
```

inside distributed client code.

Production AI architecture should use protected infrastructure such as:

```text
WillDo App
↓
Backend / Edge Function
↓
AI Provider
```

Phase 1 mock development does not require AI backend infrastructure.

---

# 18. Notification Service Rule

Native notification behaviour should be isolated behind a service abstraction where practical.

Conceptually:

```text
UI / Application Logic
↓
NotificationService
↓
Expo / Native Notification API
```

Responsibilities may include:

```text
schedule
cancel
reschedule
```

Do not scatter native notification calls throughout unrelated UI components.

---

# 19. Notification Lifecycle

When reminder timing changes:

```text
Cancel previous notification
→ schedule new notification
→ persist updated state
```

When a card becomes done:

```text
Cancel any future scheduled notification
```

When a card is deleted:

```text
Cancel associated scheduled notification
→ delete card
```

Do not leave orphan notifications active.

---

# 20. Privacy Rules

Follow:

```text
docs/PRIVACY_RULES.md
```

as a product constraint.

Core rules:

- capture is user-initiated
- no email scanning
- no bank integration
- no automatic photo-library scanning
- no background clipboard monitoring
- no continuous microphone listening
- no automatic device monitoring
- persist minimum useful information
- allow user deletion

Privacy requirements should not be bypassed for implementation convenience.

---

# 21. Raw Image Handling

Original images are temporary capture inputs by default.

Do not automatically add:

```text
originalImage
imageArchive
attachmentLibrary
```

to persisted Reminder Cards.

The intended flow is:

```text
select image
→ process
→ review
→ confirm
→ persist Future Action
→ discard unnecessary raw image
```

Persistent source-image retention requires a deliberate product and privacy decision.

---

# 22. Voice Handling

Voice is a capture mechanism, not a recording-storage feature.

The intended flow is:

```text
record
→ speech-to-text
→ extraction
→ review
→ discard raw audio
```

Do not retain raw voice recordings by default.

---

# 23. Logging

Never log raw private capture data by default.

Avoid:

```ts
console.log(captureInput);
console.log(aiPrompt);
console.log(aiResponse);
console.log(reminderCard);
```

when these may contain real user content.

Prefer metadata:

```ts
console.log("Extraction started", {
  sourceType,
  requestId,
});
```

Development-only debugging that exposes content must not remain in production builds.

---

# 24. Analytics

Analytics should measure product behaviour, not user content.

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

Do not send private fields such as:

```text
title
nextAction
context
location
amount
originalText
voiceTranscript
```

as analytics properties unless an explicit future privacy decision allows it.

---

# 25. Native Permissions

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

Request permissions only when the user chooses the relevant capability.

Permission denial should degrade gracefully.

Example:

```text
Microphone denied
→ Text and Image remain usable
```

---

# 26. Error Handling

Do not silently fail.

Important operations should expose:

```text
loading
success
error
```

where relevant.

User input should not disappear unnecessarily after recoverable errors.

AI failure must provide a manual fallback.

Expected pattern:

```text
Couldn't understand this capture.

[Try Again]
[Enter Manually]
```

WillDo must remain useful when AI is unavailable.

---

# 27. Offline Behaviour

Because WillDo is local-first, existing Reminder Cards should remain available offline where practical.

Operations that do not inherently require network access should continue to work.

Examples:

- view Inbox
- open Card
- edit Card
- mark Done
- delete Card

Real AI extraction may require internet access.

Do not block access to existing local data because AI is unavailable.

---

# 28. UX Complexity Rule

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

> Does this reduce the effort required to capture or resolve a Future Action?

If not, it is probably outside the core product.

---

# 29. Component Design

Prefer:

- small focused components
- clear prop contracts
- reusable UI primitives when there is real reuse
- feature-local code when functionality is feature-specific

Avoid:

- large monolithic screens
- premature generic abstractions
- global state for everything
- abstractions created only for hypothetical future features

Use abstraction where it protects a deliberate boundary, especially:

```text
Storage
AI
Notifications
```

---

# 30. TypeScript

Use TypeScript consistently.

Avoid:

```ts
any
```

unless there is a specific unavoidable reason.

Prefer:

- explicit shared domain types
- discriminated unions where appropriate
- validation at external boundaries
- nullability that reflects the schema

Do not silently cast external AI responses into trusted domain types without validation.

---

# 31. External Data Validation

Treat data from:

- AI responses
- device APIs
- future backend APIs
- persisted legacy data

as external input.

Validate before relying on it.

AI structured output must follow:

```text
docs/AI_EXTRACTION_SPEC.md
```

Invalid data should produce controlled failure rather than corrupt persistent product state.

---

# 32. Dependency Rule

Do not introduce a new dependency without a clear reason.

Before adding one, consider:

1. What problem does it solve?
2. Does Expo / React Native already provide the capability?
3. Is it required in the current phase?
4. Does it create meaningful maintenance cost?
5. Is the package actively maintained?

Avoid dependency accumulation during vibe coding.

If a dependency materially changes architecture, explain the choice before introducing it.

---

# 33. Refactoring Rule

Do not perform unrelated large refactors while implementing a focused task.

Refactoring is appropriate when it:

- removes relevant duplication
- fixes architectural boundary violations
- improves maintainability of the current feature
- fixes bugs
- supports the current phase

Avoid speculative architecture for:

```text
future enterprise scale
future cloud sync
future teams
future web version
```

Cloud readiness should come primarily from clean interfaces.

---

# 34. Scope Control

Before implementing a feature, confirm that it appears in:

```text
MVP_SCOPE.md
or
BUILD_PLAN.md
```

If not, do not implement it automatically.

If a useful adjacent feature is discovered:

```text
report it
→ explain potential value
→ do not implement unless approved
```

---

# 35. Documentation Change Rule

When implementation intentionally changes an established product decision, update the documentation.

Examples:

A change to:

```text
ReminderCard schema
AI extraction behaviour
privacy model
MVP scope
core UX flow
storage architecture
```

should update the corresponding document.

Significant changes should also be recorded in:

```text
docs/DECISIONS.md
```

Code and documentation should not knowingly contradict each other.

---

# 36. Task Scope

Each Codex task should be small and independently reviewable.

Good:

```text
Implement CardRepository interface and local implementation.
```

Good:

```text
Build editable Review screen for mock ExtractionResult.
```

Bad:

```text
Build the MVP.
```

Bad:

```text
Finish all AI features.
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

# 37. Before Starting a Task

Before making changes:

1. Read `AGENTS.md`.
2. Read the task description.
3. Read relevant `/docs` files.
4. Inspect existing implementation.
5. Identify existing patterns that should be reused.
6. Confirm the task belongs to the current development phase.
7. Identify any product or architecture conflict before coding.

Do not immediately generate code before understanding the existing repository.

---

# 38. During a Task

While implementing:

- keep changes focused
- preserve existing behaviour unless change is required
- follow established types and interfaces
- do not add unrelated features
- do not silently modify scope
- do not bypass repository/service abstractions
- handle loading and error states
- consider empty states where relevant
- consider permission denial where relevant
- consider privacy implications when processing new data

If blocked by an important ambiguity, explain it instead of inventing product behaviour.

---

# 39. Verification

Before declaring a task complete, run the relevant available checks.

These may include:

```text
TypeScript
Lint
Tests
Expo build / development launch
Manual flow verification
```

Do not state that something works if it was not verified.

For native features such as:

- camera
- microphone
- notifications

physical-device testing may eventually be required.

Clearly state when verification was limited to a simulator or static checks.

---

# 40. Task Completion Report

When completing a coding task, summarise:

## What changed

A brief explanation of implementation.

## Files changed

List important files created or modified.

## Verification

Explain what checks were run.

## Remaining limitations

State anything intentionally incomplete.

## Decisions / assumptions

Mention any new assumption that may need product review.

Do not hide unresolved problems behind a successful-sounding completion message.

---

# 41. Build Plan Compliance

Development should follow:

```text
docs/BUILD_PLAN.md
```

The next task should normally come from the current phase.

Do not skip to later-phase functionality unless:

- earlier dependencies are complete, or
- there is an explicit reason to change sequence

Major sequencing changes should be intentional.

---

# 42. Testing Priorities

Testing should focus on product-critical boundaries.

Especially important:

### Data

```text
Repository CRUD
Persistence after restart
Schema validity
```

### AI

```text
Structured output validation
Clear input
Ambiguous input
Missing dates
Relative dates
Extraction failure
```

### Reminder lifecycle

```text
Schedule
Edit
Cancel
Done
Delete
```

### UX

```text
Capture
Review
Confirm
Recover from failure
```

Do not pursue high test coverage numbers at the expense of testing the core product loop.

---

# 43. Accessibility

Consider accessibility while building components.

Important areas include:

- touch target size
- screen reader labels
- text scaling
- readable hierarchy
- colour-independent status
- clear error messaging
- keyboard interaction
- reduced motion where applicable

Do not deliberately create inaccessible UI to match a visual mockup.

---

# 44. Visual Design

Until a formal design system is defined:

- keep UI simple
- prioritise hierarchy and usability
- design for mobile touch interaction
- avoid unnecessary visual complexity
- do not invent a large design system
- do not introduce decorative features that slow core development

Visual polish should not override Capture speed or Review clarity.

---

# 45. Current Product Hypotheses

The following are working assumptions, not permanent facts:

- young adults are the primary initial audience
- Future Action Capture moments occur frequently
- text, image, and voice all provide meaningful value
- one Capture → one Future Action is sufficient for MVP
- one Card → one Reminder is sufficient for MVP
- Inbox is the right management model
- native notifications provide sufficient reminder visibility
- users will accept a lightweight Review step
- AI will meaningfully reduce capture friction
- original source retention is not required by default

Do not encode these assumptions so deeply that they become impossible to test or revise.

---

# 46. Product Metrics Mindset

When implementation allows future measurement, prefer metrics that test the product hypothesis.

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
number of task-management features
```

The goal is lower user effort and better follow-through.

---

# 47. Security and Privacy Red Flags

Stop and raise the issue before implementing anything involving:

- reusable AI secrets in client code
- automatic email scanning
- bank integrations
- continuous background monitoring
- automatic screenshot scanning
- persistent storage of all original captures
- raw capture content in analytics
- raw personal content in production logs
- high-sensitivity document workflows
- large new external data integrations

These change WillDo's privacy model and require deliberate product decisions.

---

# 48. North Star

Before making a significant implementation decision, ask:

> Does this make it easier for users to capture something they need to deal with later and trust that it will return when it matters?

If the implementation primarily makes WillDo a more complicated task manager, it is probably the wrong direction.

---

# 49. Final Agent Rule

Do not optimise for:

> building the most features.

Optimise for:

> building the smallest reliable version of the intended WillDo experience.

The quality of the Future Action Capture loop matters more than feature count.