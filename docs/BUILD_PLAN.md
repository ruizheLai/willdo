# WillDo — Build Plan

> Status: Draft v0.2  
> Purpose: This document defines the implementation sequence for the WillDo MVP.
>
> It translates the product direction, MVP scope, data model, AI behaviour, privacy rules, UX flows, and technical decisions into small, testable development tasks.
>
> Development should follow this plan incrementally.
>
> Do not skip ahead to later phases unless the current milestone is stable enough to support it.

---

# 1. Build Strategy

WillDo should be developed vertically rather than building every layer independently and connecting them later.

The preferred approach is:

```text
Build one complete small flow
↓
Test it
↓
Stabilise the architecture
↓
Add the next capability
```

Avoid:

```text
Build all screens
→ build all storage
→ build all AI
→ build all native integrations
→ connect everything at the end
```

The first priority is establishing a working Future Action Capture loop.

---

# 2. Core Development Principle

The product should evolve in this order:

```text
Interaction first
↓
Architecture stability
↓
Real AI
↓
Multimodal capture
↓
Native reminder loop
↓
System integration
↓
Polish
↓
Validation
```

The MVP should not begin with:

- authentication
- cloud database
- multi-device sync
- calendar integration
- Share Sheet
- widgets
- advanced notification behaviour
- complex design-system infrastructure

These do not validate the primary Future Action Capture hypothesis.

---

# 3. Core Technology Stack

The WillDo MVP uses:

```text
React Native
Expo
TypeScript
Expo Router
```

Development should move to an Expo Development Build early rather than depending on Expo Go as the long-term development environment.

Local structured persistence uses:

```text
expo-sqlite
```

Runtime validation at untrusted boundaries uses:

```text
Zod
```

The initial real AI architecture is expected to use:

```text
WillDo App
↓
ExtractionService
↓
Supabase Edge Function
↓
AI Provider
```

Supabase Auth, Postgres-backed Reminder Card storage, and cloud sync are not part of the initial local-first MVP.

---

# 4. Technical Principles

The application is:

```text
Native mobile
Local-first
Account-free for MVP
Cloud-ready
AI-assisted
User-controlled
```

Technology choices should support these principles without introducing infrastructure before it is required.

---

# 5. High-Level Architecture

The intended architecture is:

```text
UI
↓
Application / Feature Logic
↓
Domain Interfaces
├── CardRepository
├── ExtractionService
└── NotificationService
↓
Infrastructure Implementations
├── SQLiteCardRepository
├── MockExtractionService
├── RealAIExtractionService
├── ExpoNotificationService
└── Native Expo modules
```

The UI should not directly depend on:

- SQLite
- Supabase
- an AI provider
- Expo Notifications
- future cloud storage

where a deliberate abstraction exists.

---

# 6. Local Data Architecture

Confirmed Reminder Cards should be persisted locally through:

```text
CardRepository
↓
SQLiteCardRepository
↓
expo-sqlite
```

The domain model must remain separate from the SQLite representation.

Do not treat:

```text
ReminderCard
```

as identical to:

```text
SQLite database row
```

The repository implementation is responsible for mapping between persistent representation and domain objects where necessary.

---

# 7. AI Architecture

The AI layer should use:

```ts
ExtractionService
```

as the stable application contract.

Development sequence:

```text
Phase 1
MockExtractionService

↓

Phase 2
RealAIExtractionService

↓

Supabase Edge Function

↓

AI Provider
```

The Review UI should not depend on:

- model provider
- prompt format
- backend implementation

All implementations should return compatible:

```ts
ExtractionResult
```

objects.

---

# 8. External Data Validation

Data crossing an untrusted boundary should be runtime validated.

The initial validation library is:

```text
Zod
```

Important validation boundaries include:

- AI responses
- backend responses
- future migrated persisted data
- other external payloads where TypeScript alone cannot guarantee runtime shape

Conceptually:

```text
External data
↓
Zod validation
↓
Trusted domain structure
↓
Application
```

Do not use type assertions such as:

```ts
response as ExtractionResult
```

as a substitute for runtime validation of external AI responses.

Zod does not need to wrap every internal object.

Use it where runtime trust boundaries justify it.

---

# 9. Native Capability Direction

The current implementation direction is:

```text
Navigation
→ Expo Router

Local database
→ expo-sqlite

Image selection / camera entry
→ expo-image-picker

Voice recording
→ expo-audio

Local reminders
→ expo-notifications

Calendar integration
→ expo-calendar
```

These are implementation choices for the current architecture.

They may be changed later if implementation evidence provides a strong reason.

Major changes should be documented in `DECISIONS.md`.

---

# 10. Development Build Strategy

Expo Go may be useful for very early smoke testing.

However, WillDo should move to an Expo Development Build early because later phases rely on native capabilities and configuration.

The intended development environment is therefore:

```text
Expo project
↓
Development Build
↓
Simulator / Emulator
+
Physical device testing
```

Do not design the project around Expo Go limitations.

---

# 11. Suggested Project Structure

The exact structure may evolve, but the initial direction should resemble:

```text
willdo/
│
├── app/
│   ├── _layout.tsx
│   ├── index.tsx
│   │
│   ├── capture/
│   │   └── ...
│   │
│   ├── review/
│   │   └── ...
│   │
│   └── reminders/
│       └── [id].tsx
│
├── src/
│   ├── components/
│   │   ├── capture/
│   │   ├── reminder/
│   │   └── ui/
│   │
│   ├── features/
│   │   ├── capture/
│   │   ├── extraction/
│   │   ├── inbox/
│   │   └── reminders/
│   │
│   ├── repositories/
│   │   └── cards/
│   │
│   ├── services/
│   │   ├── extraction/
│   │   └── notifications/
│   │
│   ├── storage/
│   │   └── sqlite/
│   │
│   ├── schemas/
│   │
│   ├── types/
│   │
│   ├── utils/
│   │
│   └── constants/
│
├── assets/
│
├── docs/
│
├── AGENTS.md
├── README.md
└── package.json
```

This is a starting direction.

Do not create every folder before it is actually needed.

Avoid speculative architecture.

---

# 12. Development Phases

The MVP is developed through:

```text
Phase 0 — Project Foundation

Phase 1 — Text Capture + Mock AI

Phase 2 — Real Text AI

Phase 3 — Image Capture

Phase 4 — Voice Capture

Phase 5 — Native Reminder Loop

Phase 6 — Integration + UX Polish

Phase 7 — MVP Validation Build
```

Each phase should produce a usable increment.

---

# 13. Phase 0 — Project Foundation

## Goal

Create a stable native project foundation before implementing core product features.

At the end of Phase 0:

```text
The app runs reliably

+

Routing works

+

Development Build works

+

core domain contracts exist

+

the application is ready for incremental feature development
```

---

## Task 0.1 — Initialise Expo Application

### Requirements

Create the project using:

```text
React Native
Expo
TypeScript
```

Use:

```text
Expo Router
```

for application routing.

Do not introduce a second competing navigation architecture without a strong reason.

### Acceptance Criteria

- application installs successfully
- application launches without runtime errors
- TypeScript is configured
- Expo Router works
- a basic route can be opened
- project is connected to Git
- existing project documentation remains available

---

## Task 0.2 — Configure Development Build

### Requirements

Prepare WillDo to run using an Expo Development Build.

The architecture should not depend on Expo Go for long-term development.

### Acceptance Criteria

- development client configuration exists
- project can build successfully for the current development platform
- application launches in the Development Build
- navigation works inside the Development Build

---

## Task 0.3 — Configure Basic Code Quality

### Requirements

Set up appropriate checks for:

- TypeScript
- linting
- formatting

Do not introduce unnecessary tooling.

### Acceptance Criteria

Developers and coding agents can run predictable checks before considering a task complete.

The project should not rely entirely on visual verification.

---

## Task 0.4 — Add Runtime Validation Foundation

### Requirements

Add:

```text
Zod
```

for runtime schema validation at external boundaries.

Do not immediately create schemas for every internal object.

Initial focus should be on supporting future validation of:

```text
ExtractionResult
External AI responses
Backend responses
```

### Acceptance Criteria

- Zod is installed
- there is a predictable location for runtime schemas
- architecture distinguishes TypeScript domain types from runtime validation

---

## Task 0.5 — Create Core Domain Types

Implement the v0.1 data structures defined in:

```text
docs/DATA_SCHEMA.md
```

Including:

```text
CaptureInput
ReminderCardDraft
ExtractionResult
ReminderCard
ReminderSchedule
```

and related enums/types.

### Acceptance Criteria

- shared types exist in a predictable location
- feature components do not redefine duplicate domain schemas
- TypeScript checks succeed

---

## Task 0.6 — Define CardRepository Contract

Create:

```ts
CardRepository
```

according to `DATA_SCHEMA.md`.

The interface should support:

```text
listCards
getCard
createCard
updateCard
deleteCard
```

Do not expose SQLite details through the interface.

### Acceptance Criteria

Application code can depend on:

```ts
CardRepository
```

without knowing how cards are persisted.

---

## Task 0.7 — Define ExtractionService Contract

Create:

```ts
ExtractionService
```

according to:

```text
docs/AI_EXTRACTION_SPEC.md
```

### Acceptance Criteria

The application can later switch between:

```text
MockExtractionService
RealAIExtractionService
```

without changing Review UI behaviour.

---

## Task 0.8 — Define NotificationService Contract

Create a minimal notification abstraction for future use.

The initial interface may support concepts such as:

```text
schedule
cancel
reschedule
```

Actual notification behaviour does not need to be implemented yet.

### Acceptance Criteria

Future notification code will not require Expo notification calls to be scattered across UI components.

---

## Phase 0 Exit Gate

Do not proceed until:

- native Expo project runs
- Expo Router works
- Development Build works
- TypeScript checks work
- core domain types exist
- CardRepository contract exists
- ExtractionService contract exists
- NotificationService boundary exists
- Zod foundation exists
- documentation remains consistent with implementation

---

# 14. Phase 1 — Text Capture + Mock AI

## Goal

Validate the core Future Action Capture interaction without relying on real external AI.

The first complete flow is:

```text
Inbox
→ Text Capture
→ Mock Extraction
→ Review
→ Confirm
→ SQLite
→ Inbox
```

This is the most important early milestone.

---

# 15. Phase 1A — SQLite Persistence

## Task 1.1 — Implement SQLite Database Foundation

### Requirements

Use:

```text
expo-sqlite
```

for structured local persistence.

Create only the database structures required for the current MVP.

Do not introduce:

- cloud storage
- Supabase database
- ORM
- sync engine

at this phase.

### Acceptance Criteria

- SQLite database opens successfully
- required tables can be created
- database survives app restart
- database setup is isolated from UI components

---

## Task 1.2 — Implement SQLiteCardRepository

Create:

```text
SQLiteCardRepository
```

implementing:

```ts
CardRepository
```

using `expo-sqlite`.

### Acceptance Criteria

The repository supports:

```text
Create
Read
Update
Delete
List
```

Reminder Cards remain available after restarting the application.

UI code does not contain SQL.

---

## Task 1.3 — Repository Error Handling

Repository failures should be predictable and observable.

Avoid silent failure.

### Acceptance Criteria

If storage fails:

- the application does not silently lose data
- UI can react appropriately
- errors can be diagnosed during development
- private Reminder Card content is not unnecessarily logged

---

# 16. Phase 1B — Inbox

## Task 1.4 — Build Basic Inbox Screen

Create the initial home screen.

It should contain:

- WillDo product identity
- obvious Capture entry point
- active Reminder Card list
- empty state

Do not implement advanced grouping or filtering yet.

### Acceptance Criteria

When no cards exist:

```text
the user understands the purpose of the screen

+

Capture is obvious
```

When cards exist:

```text
the user can identify what requires future attention
```

---

## Task 1.5 — Build Reminder Card Component

A compact Inbox card should show enough information to answer:

```text
What?
When?
```

Potential fields:

- title
- target date/time
- reminder timing where useful
- status where necessary

Do not display every schema field.

### Acceptance Criteria

Cards remain easily scannable on a mobile screen.

---

# 17. Phase 1C — Text Capture

## Task 1.6 — Build Capture Entry

Create the primary Capture entry point.

For Phase 1:

```text
Text
```

is the only required functional capture method.

Do not implement Image or Voice simply because placeholders exist.

### Acceptance Criteria

Users can move from Inbox to Text Capture with minimal interaction.

---

## Task 1.7 — Build Text Capture Screen

Support:

- free-form typing
- pasted text
- multiline content
- submission

Do not require:

- title
- category
- date
- reminder
- priority
- project

before submission.

### Acceptance Criteria

A user can directly submit:

```text
Need to cancel my gym membership before August 12.
```

---

# 18. Phase 1D — Mock Extraction

## Task 1.8 — Implement MockExtractionService

Create:

```text
MockExtractionService
```

implementing:

```ts
ExtractionService
```

The mock service must return valid:

```ts
ExtractionResult
```

objects.

Support representative scenarios such as:

```text
Deadline
Appointment
Follow-up
General reminder
Ambiguous input
```

### Acceptance Criteria

- Review UI contains no mock-specific implementation logic
- mock output follows the same contract expected from real AI
- replacing the service later does not require Review screen redesign

---

## Task 1.9 — Build Processing State

After submission, show a clear processing state.

Example:

```text
Finding what matters...
```

### Acceptance Criteria

Users can distinguish:

```text
submitted
```

from:

```text
still processing
```

The app must not appear frozen.

---

# 19. Phase 1E — Review

## Task 1.10 — Build Reminder Review Screen

Display the proposed Future Action.

Primary fields:

```text
Title
Target date/time
Reminder timing
Next action
```

Secondary fields may include:

```text
Category
Location
Amount
Contact/provider
Context
```

### Acceptance Criteria

Users can understand the proposed Future Action without seeing internal implementation details.

---

## Task 1.11 — Make Core Review Fields Editable

Allow users to correct at least:

- title
- target date/time
- reminder timing
- next action

### Acceptance Criteria

User changes become authoritative.

The app does not reapply mock or AI values over user edits.

---

## Task 1.12 — Confirm Reminder Card

Confirmation flow:

```text
ReminderCardDraft
↓
User confirms
↓
ReminderCard created
↓
CardRepository.createCard()
↓
SQLiteCardRepository
↓
expo-sqlite
```

### Acceptance Criteria

- confirmed card appears in Inbox
- card persists after restart
- raw CaptureInput does not automatically become permanent product data

Notifications are not yet required in this phase.

---

# 20. Phase 1F — Card Lifecycle

## Task 1.13 — Build Card Detail Screen

Use an Expo Router route capable of identifying a Reminder Card by ID.

Conceptually:

```text
/reminders/[id]
```

Display enough information for the Future Action to remain useful later.

### Acceptance Criteria

The user can reopen a card and understand:

```text
What is this?
When does it matter?
What should I do?
```

---

## Task 1.14 — Mark Card as Done

Support:

```text
active
→ done
```

### Acceptance Criteria

Completed cards no longer appear as active Inbox items.

The change persists after restart.

---

## Task 1.15 — Edit Existing Card

Allow confirmed card information to be modified.

Do not automatically rerun AI extraction.

### Acceptance Criteria

Edits persist through the repository.

---

## Task 1.16 — Delete Card

Support deletion.

### Acceptance Criteria

- destructive action is confirmed
- deleted card disappears from local persistence
- application remains stable

---

# 21. Phase 1 Exit Gate

At the end of Phase 1, a user must be able to:

```text
Open WillDo

↓

Capture text

"Cancel gym membership before August 12"

↓

See mock extraction

↓

Review and edit it

↓

Confirm

↓

Persist through expo-sqlite

↓

See the card in Inbox

↓

Open the card

↓

Edit / complete / delete it
```

Do not connect real AI until this flow is stable.

---

# 22. Phase 2 — Real Text AI

## Goal

Replace mock interpretation with real AI while preserving the existing application contracts.

The core UI should not need to know that the implementation changed.

---

## Task 2.1 — Create Supabase Project for AI Infrastructure

### Requirements

Use Supabase initially only where needed to support the protected AI extraction backend.

Do not introduce:

```text
Supabase Auth
Cloud Reminder Card database
Cross-device sync
```

as part of this task.

### Acceptance Criteria

The project is ready to host an Edge Function for extraction.

---

## Task 2.2 — Create AI Extraction Edge Function

Initial architecture:

```text
WillDo App
↓
Supabase Edge Function
↓
AI Provider
```

The Edge Function is responsible for protecting reusable provider credentials.

### Acceptance Criteria

- private AI provider credentials are not embedded in the mobile client
- Edge Function can receive extraction input
- Edge Function can invoke the selected AI provider
- response can be returned to the application

---

## Task 2.3 — Define Structured AI Output Schema

Create a runtime Zod schema representing the external extraction response expected by WillDo.

The schema should align with:

```text
docs/DATA_SCHEMA.md
docs/AI_EXTRACTION_SPEC.md
```

### Acceptance Criteria

Invalid external values cannot be treated as trusted:

```ts
ExtractionResult
```

without validation.

---

## Task 2.4 — Implement RealAIExtractionService

Create:

```text
RealAIExtractionService
```

implementing:

```ts
ExtractionService
```

It should communicate with the WillDo backend rather than directly exposing provider secrets.

### Acceptance Criteria

Switching:

```text
MockExtractionService
→ RealAIExtractionService
```

does not require changes to:

- Capture UI
- Review UI
- ReminderCard domain structure

---

## Task 2.5 — Provide Extraction Context

Send relevant context such as:

```text
current date/time
timezone
locale where useful
```

to extraction.

### Acceptance Criteria

Relative expressions such as:

```text
tomorrow
next Friday
in three days
```

can be interpreted relative to the actual capture moment.

---

## Task 2.6 — Validate AI Responses

The client or trusted boundary must validate AI-derived structured data before using it as a trusted application object.

Conceptually:

```text
AI response
↓
Zod
↓
Valid ExtractionResult
↓
Review
```

### Acceptance Criteria

Values such as:

```text
category = "shopping"
```

or:

```text
targetAt = "next Friday probably"
```

cannot silently enter the normal Review flow as valid domain data.

---

## Task 2.7 — AI Error Flow

Handle:

- network failure
- backend failure
- model timeout
- invalid structured output
- provider error

Expected UX:

```text
Couldn't understand this capture.

[Try Again]
[Enter Manually]
```

### Acceptance Criteria

AI failure does not prevent users from creating a Future Action manually.

---

## Task 2.8 — AI Uncertainty Flow

Support:

```text
confidence
uncertainFields
clarificationQuestion
```

from `ExtractionResult`.

### Acceptance Criteria

Important ambiguity is not silently converted into confirmed information.

---

## Task 2.9 — Clarification Interaction

Prefer quick structured choices where possible.

Example:

```text
When should you follow up?

[Thursday]
[Friday]
[Another date]
```

### Acceptance Criteria

Clarification remains lightweight and does not turn WillDo into a chatbot.

---

# 23. Phase 2 Exit Gate

Real text AI should support representative cases including:

```text
Clear Future Action
Ambiguous Future Action
Missing date
Relative date
No obvious Future Action
Invalid AI response
Network / provider failure
```

before Image Capture begins.

---

# 24. Phase 3 — Image Capture

## Goal

Validate the important Capture Moment where the user already has the information as a screenshot or photo.

---

## Task 3.1 — Add Image Capture Entry

Initial implementation direction:

```text
expo-image-picker
```

Support choosing an existing image.

Camera capture may also be supported where useful.

### Acceptance Criteria

The user intentionally chooses Image Capture.

---

## Task 3.2 — Contextual Permission Handling

Request relevant image or camera permissions only when the user attempts to use the capability.

### Acceptance Criteria

If permission is denied:

```text
Text Capture
```

continues to work.

---

## Task 3.3 — Image Preview

Before processing:

```text
Preview image

[Use this image]
[Choose another]
```

### Acceptance Criteria

The user knows which image is about to be processed.

---

## Task 3.4 — Extend Extraction Pipeline for Images

Image extraction must still return:

```ts
ExtractionResult
```

through:

```ts
ExtractionService
```

### Acceptance Criteria

The Review screen remains based on the same domain contract used for Text Capture.

Do not create an independent Image Reminder architecture.

---

## Task 3.5 — Temporary Image Lifecycle

The original image is temporary by default.

It should not become a persisted Reminder Card attachment automatically.

### Acceptance Criteria

After confirmation or cancellation, core WillDo functionality does not depend on retaining the original image.

Follow:

```text
docs/PRIVACY_RULES.md
```

---

## Task 3.6 — Image Error States

Support cases such as:

- unreadable image
- unsupported image
- no Future Action
- multiple relevant dates
- extraction failure

### Acceptance Criteria

Recovery options are available where appropriate:

```text
Retry
Choose another image
Enter manually
```

---

# 25. Phase 3 Exit Gate

Test representative image scenarios:

```text
Appointment screenshot
Event poster
Receipt with return deadline
Bill
Chat follow-up
Multiple-date screenshot
```

AI should preserve useful Future Action information without unnecessarily persisting unrelated content.

---

# 26. Phase 4 — Voice Capture

## Goal

Validate fast Future Action Capture when typing or taking a screenshot is inconvenient.

---

## Task 4.1 — Add Voice Capture Entry

Initial implementation direction:

```text
expo-audio
```

### Acceptance Criteria

Recording only begins after explicit user action.

---

## Task 4.2 — Contextual Microphone Permission

Request microphone access only when Voice Capture is used.

### Acceptance Criteria

If permission is denied:

```text
Text
Image
```

remain available.

---

## Task 4.3 — Recording State

Clearly communicate:

```text
Recording
Stop
Cancel
```

### Acceptance Criteria

The user is never uncertain about whether recording is active.

---

## Task 4.4 — Speech-to-Text

Convert captured speech into text using an appropriate transcription path.

Then reuse the existing extraction flow:

```text
Voice
↓
Transcript
↓
ExtractionService
↓
ExtractionResult
↓
Review
```

### Acceptance Criteria

Voice does not introduce a separate Reminder Card schema.

---

## Task 4.5 — Transcription Recovery

Handle transcription failure or obvious recognition errors.

Possible recovery:

```text
Retry
Edit transcript
Type instead
```

### Acceptance Criteria

A voice error does not force users to restart the entire product flow unnecessarily.

---

# 27. Phase 4 Exit Gate

Test:

- short reminder
- follow-up
- relative date
- imperfect transcription
- cancelled recording
- permission denied

---

# 28. Phase 5 — Native Reminder Loop

## Goal

Complete the core product promise:

```text
I captured it
→ it comes back when it matters
```

---

## Task 5.1 — Implement ExpoNotificationService

Use:

```text
expo-notifications
```

behind:

```ts
NotificationService
```

Responsibilities should include:

```text
schedule
cancel
reschedule
```

### Acceptance Criteria

UI components do not directly own native notification implementation.

---

## Task 5.2 — Contextual Notification Permission

Do not request notification permission during initial launch.

Preferred flow:

```text
User creates reminder
↓
Explain reminder value
↓
Request permission
```

### Acceptance Criteria

Permission is requested with meaningful product context.

---

## Task 5.3 — Schedule Local Notification

When a confirmed Reminder Card contains an enabled schedule:

```text
save card
↓
schedule notification
```

### Acceptance Criteria

The notification fires at the expected time on a physical device.

---

## Task 5.4 — Persist Notification Binding

Persist native notification identifiers separately from WillDo's own business identifiers.

### Acceptance Criteria

WillDo can reliably identify which native notification should be:

```text
cancelled
rescheduled
```

---

## Task 5.5 — Edit Reminder Timing

When timing changes:

```text
cancel previous native notification
↓
schedule replacement
↓
persist updated state
```

### Acceptance Criteria

Old notifications do not remain scheduled accidentally.

---

## Task 5.6 — Mark Done

When a card becomes:

```text
done
```

cancel future notification where applicable.

---

## Task 5.7 — Delete Reminder

Deletion should also cancel associated scheduled notifications.

---

## Task 5.8 — Notification Navigation

Use the application's routing structure so a notification can open the relevant Reminder Card.

Conceptually:

```text
notification
↓
Expo Router
↓
/reminders/[id]
```

### Acceptance Criteria

Users can return directly to the relevant Future Action rather than losing context.

---

## Task 5.9 — Notification Privacy Review

Define the initial notification content according to:

```text
docs/PRIVACY_RULES.md
```

Do not unnecessarily expose sensitive Reminder Card information.

---

# 29. Phase 5 Exit Gate

The full core WillDo loop must work on a physical device:

```text
Capture
↓
AI
↓
Review
↓
Confirm
↓
SQLite persistence
↓
Schedule local notification
↓
Receive notification
↓
Open relevant card
↓
Resolve
↓
Mark Done
```

This is the first point at which the full core product promise exists.

---

# 30. Phase 6 — Integration and UX Polish

## Goal

Improve the validated core experience without expanding WillDo into a generic productivity application.

P1 work should be prioritised based on observed friction.

---

## Task 6.1 — Inbox Information Architecture

Evaluate grouping such as:

```text
Needs Attention
Today
Tomorrow
Coming Up
Later
```

Do not add complexity without evidence.

---

## Task 6.2 — Completed View

Support access to completed cards.

Potential actions:

```text
View
Restore
Delete
```

---

## Task 6.3 — Search

Implement lightweight local search over relevant structured fields.

Possible fields:

- title
- nextAction
- provider/contact
- context

Do not introduce semantic AI search unless there is evidence it is required.

SQLite should support the initial local querying needs.

---

## Task 6.4 — Calendar Integration

Initial implementation direction:

```text
expo-calendar
```

Calendar integration should appear only where it provides clear value.

Example:

```text
Dentist appointment
→ Add to Calendar
```

Not every Reminder Card should become a calendar event.

### Acceptance Criteria

- calendar permission is requested contextually
- user explicitly initiates calendar creation
- exported event information can be reviewed
- WillDo remains usable without calendar access

---

## Task 6.5 — Follow-Up Suggestions

Improve support for scenarios such as:

```text
If they haven't replied by Friday...
```

Suggestions remain user-confirmed.

---

## Task 6.6 — Empty States

Refine Capture-first empty states.

Avoid:

```text
You have no tasks.
```

Prefer framing around future attention.

---

## Task 6.7 — Onboarding

Create lightweight onboarding that communicates:

```text
Capture now
↓
WillDo understands
↓
You review
↓
It comes back later
```

Do not build a long feature-tour onboarding flow.

---

## Task 6.8 — Accessibility Pass

Review:

- touch targets
- text scaling
- screen reader labels
- contrast
- colour-independent status
- keyboard behaviour
- motion

---

## Task 6.9 — Error-State Pass

Review:

```text
Offline
AI failure
Permission denied
Invalid date
SQLite failure
Notification failure
Image failure
Voice failure
```

---

## Task 6.10 — Performance Pass

Review:

- app startup
- SQLite loading
- Inbox rendering
- Capture responsiveness
- AI loading behaviour
- large image handling

Capture should feel fast even when AI processing itself takes time.

---

# 31. Share Sheet — Post-Core Direction

System Share Sheet capture strongly supports the Capture-first strategy.

Ideal future experience:

```text
User sees something in another app
↓
Share
↓
WillDo
↓
Understand
↓
Review
↓
Confirm
```

However, Share Sheet behaviour should not be a dependency of the initial core MVP.

Do not architect the core Capture flow around an assumption that Share Sheet integration must already exist.

It can be explored after the core native Capture experience is validated.

---

# 32. State Management Strategy

Do not add a global state-management library initially.

Start with:

```text
React state
Custom hooks
Repository / Service state where appropriate
```

Do not introduce:

```text
Redux
Zustand
MobX
TanStack Query
```

unless application complexity creates a concrete need.

State-management infrastructure should solve demonstrated problems rather than anticipated ones.

---

# 33. ORM Strategy

Do not introduce an ORM during the initial MVP.

Start with:

```text
expo-sqlite
+
SQLiteCardRepository
```

Potential ORM adoption may be evaluated later if:

- schema complexity increases significantly
- migrations become difficult to manage
- query duplication becomes substantial

Do not introduce an ORM purely because one may be useful in a larger future application.

---

# 34. Phase 7 — MVP Validation Build

## Goal

Prepare a build suitable for real usability testing.

The focus is product validation, not full public commercial launch.

---

## Task 7.1 — Build Representative Test Cases

Use realistic but non-sensitive scenarios:

```text
Appointment
Return deadline
Bill
Event deadline
Landlord follow-up
Subscription cancellation
General reminder
```

---

## Task 7.2 — Instrument Privacy-Safe Product Metrics

Possible events:

```text
capture_started
capture_submitted
extraction_completed
review_edited
reminder_confirmed
notification_opened
reminder_completed
```

Do not include Reminder Card content in analytics.

---

## Task 7.3 — Measure Capture Friction

Potential metrics:

```text
Time to Confirm
Interaction count
Capture abandonment rate
```

---

## Task 7.4 — Measure Review Effort

Potential metrics:

```text
Fields edited per capture
% confirmed without editing
Clarification rate
Review duration
```

---

## Task 7.5 — Usability Test Core Flow

Test whether users can:

```text
Capture
Understand
Correct
Confirm
Find
Receive reminder
Resolve
```

without external assistance.

---

# 35. MVP Definition of Done

WillDo is not complete merely because the screens exist.

The complete MVP requires:

```text
User encounters something they need to deal with later

↓

User captures it with:
Text
Image
or Voice

↓

WillDo identifies a Future Action

↓

User reviews and corrects it

↓

User confirms

↓

Reminder Card is stored locally in SQLite

↓

Native notification is scheduled

↓

The Future Action returns to attention

↓

User opens the Reminder Card

↓

User resolves it

↓

User marks it Done
```

---

# 36. Functional Definition of Done

Before calling the MVP functionally complete:

- [ ] Expo native application works
- [ ] Expo Router navigation works
- [ ] Development Build works
- [ ] Text Capture works
- [ ] Image Capture works
- [ ] Voice Capture works
- [ ] Real AI extraction works
- [ ] AI requests use protected backend infrastructure
- [ ] reusable AI provider secret is not present in the mobile client
- [ ] AI structured output is runtime validated
- [ ] AI uncertainty can be handled
- [ ] Review allows correction
- [ ] user edits remain authoritative
- [ ] Reminder Cards persist through `expo-sqlite`
- [ ] Inbox persists across app restart
- [ ] Cards can be edited
- [ ] Cards can be completed
- [ ] Cards can be deleted
- [ ] local notifications work
- [ ] reminder timing can be edited
- [ ] stale notifications are cancelled
- [ ] notification can open relevant context where practical
- [ ] AI failure has manual fallback
- [ ] permission denial has graceful fallback
- [ ] original images are not retained by default
- [ ] raw voice recordings are not retained by default
- [ ] important error states are handled
- [ ] core native flows work on a physical device

---

# 37. Product Definition of Done

Before treating the MVP as validated, users should demonstrate that they:

- understand what WillDo is for
- recognise Future Action Capture moments
- can capture information without learning a task-management system
- understand AI output
- can correct AI mistakes
- perceive Review as lightweight
- trust the confirmed reminder
- understand the Inbox model
- notice reminders when they return
- can resolve Future Actions

---

# 38. Out of Scope During Initial MVP Development

Do not implement unless scope is explicitly changed:

```text
Authentication
Cloud Reminder Card database
Cross-device sync
Email scanning
Bank integration
Kanban
Projects
Habit tracking
Family sharing
Team features
Complex recurrence
Advanced analytics dashboard
Document storage vault
Original screenshot archive
General AI chatbot
Automatic actions without review
```

Supabase Edge Functions used for protected AI requests do not mean Supabase Auth or Cloud Reminder Card storage are automatically in scope.

---

# 39. Codex Working Method

Do not assign:

```text
Build Phase 1
```

as one task.

Each implementation task should be small enough to:

```text
Understand
Implement
Verify
Review
Commit
```

independently.

---

# 40. Codex Task Template

Use:

```markdown
## Task

[One specific implementation task]

## Context

Read:

- AGENTS.md
- relevant documentation under /docs
- relevant existing source files

Do not assume product behaviour that is already defined elsewhere.

## Goal

[Describe the user or technical outcome.]

## Requirements

- requirement 1
- requirement 2
- requirement 3

## Non-goals

Do not:

- add unrelated features
- expand MVP scope
- refactor unrelated code
- change domain schema without explicit reason
- introduce new dependencies unless justified

## Acceptance Criteria

- criterion 1
- criterion 2
- criterion 3

## Verification

Before completing:

- run TypeScript checks
- run linting where configured
- run relevant tests
- verify the relevant flow
- state when native behaviour still requires physical-device verification

## Output

Summarise:

1. what changed
2. important files changed
3. how the work was verified
4. remaining limitations
5. any assumption or decision requiring review
```

---

# 41. Codex Change Rules

Coding agents should not independently redefine:

```text
Product positioning
MVP scope
Core data model
AI behaviour
Privacy rules
Core UX flow
```

for implementation convenience.

If implementation exposes a genuine conflict:

```text
Identify conflict
↓
Explain why
↓
Propose options
↓
Make an intentional decision
↓
Update documentation
↓
Implement
```

Do not silently change product behaviour.

---

# 42. Dependency Rule

Before adding a dependency, ask:

```text
What problem does it solve?
Is it needed in the current phase?
Does Expo / React Native already provide the capability?
What maintenance cost does it introduce?
```

Avoid library accumulation during AI-assisted development.

Current deliberate dependencies include:

```text
Expo Router
expo-sqlite
Zod
```

and later phase-specific Expo modules.

This does not mean additional dependencies are forbidden.

They require a concrete reason.

---

# 43. Refactoring Rule

Refactoring is appropriate when it:

- removes relevant duplication
- improves maintainability
- restores architectural boundaries
- fixes bugs
- supports the current task

Avoid speculative refactoring for:

```text
Future enterprise scale
Future team features
Future web client
Future cloud architecture
```

Cloud readiness should primarily come from clean interfaces.

---

# 44. Testing Strategy

Testing should grow with the application.

## Foundation

Prioritise:

```text
Type correctness
Repository behaviour
Routing
SQLite persistence
```

## AI

Prioritise:

```text
Runtime output validation
Known extraction examples
Ambiguous cases
Error handling
Relative date handling
```

## Native

Prioritise physical-device verification for:

```text
Camera
Voice recording
Notifications
Calendar
```

## Validation

Prioritise:

```text
Real user behaviour
Capture friction
Review effort
Reminder effectiveness
```

---

# 45. Physical Device Rule

Major native milestones must eventually be tested on a physical mobile device.

Simulator or emulator testing alone is not sufficient to fully validate:

```text
Camera
Microphone
Notifications
Permissions
Real-world Capture interaction
```

Clearly distinguish:

```text
static verification
simulator verification
physical-device verification
```

when reporting task completion.

---

# 46. Development Checkpoint After Each Phase

After every phase, review:

## Product

```text
Does this still strengthen Future Action Capture?
```

## UX

```text
Did we introduce unnecessary interaction?
```

## AI

```text
Is AI reducing user effort?
```

## Privacy

```text
Did we add new data collection, transmission, or persistence?
```

## Architecture

```text
Did implementation bypass repository or service boundaries?
```

## Technical complexity

```text
Did we introduce infrastructure without a current need?
```

## Scope

```text
Did WillDo drift toward generic task management?
```

Record significant changes in:

```text
docs/DECISIONS.md
```

---

# 47. Approximate Project Sequence

```text
Product definition
✓

Core documentation
✓

↓

Phase 0
Expo foundation
Router
Development Build
Core contracts

↓

Phase 1
Text Capture
Mock AI
SQLite
Review
Inbox

↓

Early interaction test

↓

Phase 2
Real AI
Supabase Edge Function
Zod validation

↓

AI evaluation

↓

Phase 3
Image

↓

Phase 4
Voice

↓

Phase 5
Native reminder loop

↓

End-to-end usability test

↓

Phase 6
Integration + polish

↓

Phase 7
MVP validation

↓

Evaluate:
Cloud
Accounts
Sync
Share Sheet
Widgets
Public launch
```

The sequence may change based on product evidence.

---

# 48. Validation Before Expansion

Do not automatically proceed from MVP to:

```text
Authentication
Cloud database
Subscription
Public App Store launch
```

without evaluating the core concept first.

Questions include:

1. Are Future Action Capture moments frequent enough?
2. Does WillDo reduce capture friction?
3. Does AI reduce manual structuring?
4. Is Review lightweight enough?
5. Are Image and Voice genuinely useful?
6. Do users notice WillDo reminders?
7. Do users return to the Inbox?
8. Does WillDo feel meaningfully different from a normal reminder app?
9. Which capture methods produce repeated value?
10. What would users miss if WillDo disappeared?

---

# 49. Post-MVP Technical Direction

If the core concept validates, likely future work may include:

```text
Supabase Auth
Supabase Postgres
Cloud sync
Multi-device support
Share Sheet
Widgets
Home-screen quick capture
Advanced notification actions
Calendar refinement
Optional source retention
```

These are future possibilities, not automatic roadmap commitments.

The local-first architecture should make future migration possible without forcing cloud complexity into the initial MVP.

---

# 50. Build North Star

At every development stage, ask:

> Are we making it easier to capture something now and trust that it will come back when it matters?

Technical sophistication is not the goal.

The goal is to build the smallest reliable version of the WillDo Future Action Capture experience and validate whether it creates meaningful value.