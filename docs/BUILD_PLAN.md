# WillDo — Build Plan

> Status: Draft v0.1  
> Purpose: This document defines the implementation sequence for the WillDo MVP.
>
> It translates the product direction, MVP scope, data model, AI behaviour, privacy rules, and UX flows into small, testable development tasks.
>
> Development should follow this plan incrementally.
>
> Do not skip ahead to later phases unless the current milestone is stable enough to support it.

---

# 1. Build Strategy

WillDo should be developed vertically rather than feature-by-feature across the entire application.

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

Avoid trying to build:

```text
all screens
→ all AI
→ all storage
→ all notifications
→ connect everything at the end
```

The first priority is to establish a working Future Action Capture loop.

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
```

The MVP should not begin with:

- authentication
- cloud database
- advanced notifications
- calendar integration
- Share Sheet
- design-system complexity

These do not validate the primary Future Action Capture hypothesis.

---

# 3. Target Architecture

The MVP will be built using:

```text
React Native
Expo
TypeScript
```

The application is:

```text
Native mobile
Local-first
Account-free
Cloud-ready
AI-assisted
```

---

## 3.1 Intended High-Level Architecture

```text
UI
↓
Application / Feature Logic
↓
Services
├── ExtractionService
├── CardRepository
└── NotificationService
↓
Infrastructure
├── Local persistence
├── AI backend
└── Native device APIs
```

UI components should not directly depend on:

- storage implementation
- AI provider
- native notification implementation

where practical.

---

# 4. Suggested Project Structure

The exact structure may evolve during implementation, but the initial direction should resemble:

```text
willdo/
│
├── app/
│   ├── _layout.tsx
│   ├── index.tsx
│   ├── capture/
│   ├── review/
│   └── reminders/
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
│   │   ├── reminders/
│   │   └── inbox/
│   │
│   ├── services/
│   │   ├── extraction/
│   │   ├── notifications/
│   │   └── storage/
│   │
│   ├── repositories/
│   │   └── cards/
│   │
│   ├── types/
│   │   └── reminder.ts
│   │
│   ├── utils/
│   │
│   └── constants/
│
├── docs/
│
├── assets/
│
├── AGENTS.md
└── README.md
```

This is a starting point, not a requirement to create every folder immediately.

Do not create abstraction layers that are not yet used.

---

# 5. Development Phases

The MVP will be developed through the following phases:

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

Each phase should produce a usable increment of the product.

---

# 6. Phase 0 — Project Foundation

## Goal

Create a stable native project foundation before implementing product features.

At the end of Phase 0:

```text
The app runs on a physical device or simulator
+
the project structure is ready for incremental development
```

---

## Task 0.1 — Initialise Expo Project

### Requirements

Create a React Native application using:

```text
Expo
TypeScript
```

Use routing suitable for the native application architecture.

### Acceptance Criteria

- project installs successfully
- application launches without errors
- app can run on target development environment
- TypeScript is enabled
- repository is connected to Git
- existing documentation remains in the repository

---

## Task 0.2 — Configure Basic Code Quality

### Requirements

Set up reasonable development checks for:

- TypeScript
- linting
- formatting

Do not introduce unnecessary tooling.

### Acceptance Criteria

A developer or Codex can run appropriate checks before considering a task complete.

The project should not rely solely on visual testing.

---

## Task 0.3 — Create Core Types

Implement the v0.1 data structures defined in:

```text
docs/DATA_SCHEMA.md
```

Including the types required for:

```text
CaptureInput
ReminderCardDraft
ExtractionResult
ReminderCard
ReminderSchedule
```

### Acceptance Criteria

- shared types exist in one predictable location
- feature components do not redefine duplicate versions of the same schema
- code compiles without TypeScript errors

---

## Task 0.4 — Define Repository Contract

Create:

```ts
CardRepository
```

according to `DATA_SCHEMA.md`.

Do not implement cloud persistence.

### Acceptance Criteria

The UI will eventually be able to use:

```ts
listCards()
getCard()
createCard()
updateCard()
deleteCard()
```

without knowing how cards are physically stored.

---

## Task 0.5 — Define Extraction Service Contract

Create:

```ts
ExtractionService
```

according to `AI_EXTRACTION_SPEC.md`.

### Acceptance Criteria

The application can later switch between:

```text
MockExtractionService
RealAIExtractionService
```

without changing the review UI contract.

---

## Phase 0 Exit Gate

Do not proceed until:

- native project runs
- TypeScript works
- core types exist
- repository contract exists
- extraction contract exists
- documentation remains consistent with implementation

---

# 7. Phase 1 — Text Capture + Mock AI

## Goal

Validate the core Future Action Capture interaction without relying on external AI.

The first complete product loop is:

```text
Inbox
→ Text Capture
→ Mock Extraction
→ Review
→ Confirm
→ Local Card
→ Inbox
```

This is the most important early milestone.

---

# 8. Phase 1A — Local Reminder Storage

## Task 1.1 — Implement Local Card Repository

Create the first implementation of:

```text
CardRepository
```

using an appropriate local persistence mechanism.

The specific storage technology may be chosen during implementation.

The UI must not depend directly on it.

### Acceptance Criteria

The repository supports:

```text
Create
Read
Update
Delete
List
```

Reminder Cards persist after the application is restarted.

---

## Task 1.2 — Repository Error Handling

Repository operations should produce predictable errors.

Avoid silent failure.

### Acceptance Criteria

If storage fails:

- the app does not crash
- UI can display an appropriate error
- failure can be diagnosed during development

---

# 9. Phase 1B — Inbox

## Task 1.3 — Build Basic Inbox Screen

Create the initial WillDo home screen.

It should contain:

- product identity
- obvious Capture entry point
- active Reminder Card list
- empty state

Do not implement advanced grouping yet.

### Acceptance Criteria

When there are no cards:

```text
the user understands what WillDo is for
+
there is an obvious way to start Capture
```

When cards exist:

```text
the user can see what they need to deal with
```

---

## Task 1.4 — Build Reminder Card Component

A compact Inbox card should show enough information to answer:

```text
What?
When?
```

Potential fields:

- title
- target date/time
- reminder date
- status

Do not expose every schema field.

### Acceptance Criteria

Cards are visually scannable on a mobile screen.

---

# 10. Phase 1C — Text Capture

## Task 1.5 — Build Capture Entry

Create a prominent Capture action from the Inbox.

For Phase 1, only:

```text
Text
```

needs to function.

Image and Voice may appear later.

### Acceptance Criteria

The user can start text capture in minimal steps.

---

## Task 1.6 — Build Text Capture Screen

Allow users to:

- type free-form text
- paste text
- submit multiline content

Do not require:

- title
- category
- date
- reminder

before submission.

### Acceptance Criteria

Example:

```text
Need to cancel my gym membership before August 12.
```

can be submitted directly.

---

# 11. Phase 1D — Mock AI Extraction

## Task 1.7 — Implement MockExtractionService

The mock service must return a valid:

```ts
ExtractionResult
```

using the same shape expected from future real AI.

It should support several representative test cases.

Examples:

```text
Deadline
Appointment
Follow-up
General reminder
Ambiguous input
```

### Acceptance Criteria

The Review UI does not contain mock-specific logic.

Replacing MockExtractionService later should not require redesigning the Review screen.

---

## Task 1.8 — Build Processing State

After capture submission, show a clear processing state.

Example:

```text
Finding what matters...
```

### Acceptance Criteria

The user understands:

```text
submission succeeded
+
processing is still happening
```

The app must not appear frozen.

---

# 12. Phase 1E — Review

## Task 1.9 — Build Reminder Review Screen

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

The user can understand what WillDo extracted without seeing raw JSON or implementation details.

---

## Task 1.10 — Make Review Fields Editable

Allow the user to correct AI output.

Prioritise direct, focused editing.

### Acceptance Criteria

Users can correct at least:

- title
- date/time
- reminder timing
- next action

before confirmation.

User edits become authoritative.

---

## Task 1.11 — Confirm Reminder Card

When the user confirms:

```text
ReminderCardDraft
↓
ReminderCard
↓
CardRepository.createCard()
```

### Acceptance Criteria

The confirmed card appears in the Inbox.

The original temporary Capture does not become permanent product data.

---

# 13. Phase 1F — Card Lifecycle

## Task 1.12 — Build Card Detail Screen

Allow users to open a saved Reminder Card.

Display:

- title
- target date/time
- reminder
- next action
- relevant supporting context

### Acceptance Criteria

The user can understand the Future Action after returning later.

---

## Task 1.13 — Mark Card as Done

Allow:

```text
active
→ done
```

### Acceptance Criteria

Completed cards no longer appear as active Inbox items.

---

## Task 1.14 — Edit Existing Card

Allow saved card information to be modified.

AI extraction should not automatically run again.

### Acceptance Criteria

Edits persist after app restart.

---

## Task 1.15 — Delete Card

Allow permanent removal during MVP.

### Acceptance Criteria

- destructive action is confirmed
- deleted card disappears from storage
- application remains stable

---

# 14. Phase 1 Exit Gate

At the end of Phase 1, a user must be able to:

```text
Open WillDo

→ type:
  "Cancel gym membership before August 12"

→ see mock AI interpretation

→ edit it

→ confirm it

→ see it in Inbox

→ open it

→ edit / complete / delete it
```

Do not connect real AI until this flow is stable.

---

# 15. Phase 2 — Real Text AI

## Goal

Replace simulated interpretation with real AI without changing the core UX contract.

---

## Task 2.1 — Choose AI Integration Architecture

Determine the first protected AI backend approach.

The mobile client must not contain a reusable secret provider key.

Expected architecture:

```text
WillDo App
↓
Protected Backend / Edge Function
↓
AI Provider
```

### Acceptance Criteria

Private provider credentials are not embedded in the distributed application.

---

## Task 2.2 — Implement AI Extraction Endpoint

The backend receives:

```text
CaptureInput
ExtractionContext
```

and returns:

```text
ExtractionResult
```

### Acceptance Criteria

Output is validated against the expected schema.

Invalid AI output does not silently enter the app.

---

## Task 2.3 — Implement RealAIExtractionService

Create the production implementation of:

```ts
ExtractionService
```

### Acceptance Criteria

Switching from mock to real extraction does not require changes to:

- Capture screen
- Review screen
- Reminder Card structure

---

## Task 2.4 — Relative Date Handling

Pass:

- current date/time
- device timezone
- locale where useful

to extraction.

### Acceptance Criteria

Inputs such as:

```text
tomorrow
next Friday
in three days
```

can be interpreted relative to the actual capture moment.

---

## Task 2.5 — Structured Output Validation

Validate:

- enums
- dates
- required structure
- unsupported fields

### Acceptance Criteria

Values such as:

```text
category = "shopping"
```

or:

```text
targetAt = "next Friday probably"
```

cannot enter the normal Review flow as valid structured output.

---

## Task 2.6 — AI Error Flow

Support:

```text
network failure
timeout
invalid response
AI failure
```

### UX

```text
Couldn't understand this capture.

[Try Again]
[Enter Manually]
```

### Acceptance Criteria

AI failure never destroys the user's ability to create a reminder.

---

## Task 2.7 — AI Uncertainty Flow

Use:

```text
confidence
uncertainFields
clarificationQuestion
```

from `ExtractionResult`.

### Acceptance Criteria

Ambiguous important fields do not silently become confirmed facts.

---

## Task 2.8 — Clarification Interaction

Where practical, transform uncertainty into quick choices.

Example:

```text
When should you follow up?

[Thursday]
[Friday]
[Another date]
```

### Acceptance Criteria

Clarification does not turn the product into a chatbot.

---

# 16. Phase 2 Exit Gate

Real text input should support:

```text
Clear Future Action
Ambiguous Future Action
Missing date
Relative date
No obvious Future Action
AI failure
```

before Image Capture begins.

---

# 17. Phase 3 — Image Capture

## Goal

Validate the most important non-text Capture Moment:

```text
I already have the information as a screenshot or photo.
```

---

## Task 3.1 — Add Image Capture Entry

Support:

```text
Choose image
Take photo
```

where practical.

### Acceptance Criteria

The user can intentionally select image capture.

---

## Task 3.2 — Contextual Permission Handling

Request image/camera permissions only when needed.

### Acceptance Criteria

Permission denial does not block:

```text
Text Capture
```

---

## Task 3.3 — Image Preview

Before processing:

```text
Preview
[Use this image]
[Choose another]
```

### Acceptance Criteria

The user knows which image is being submitted.

---

## Task 3.4 — Multimodal Extraction

Send the temporary image through the existing ExtractionService workflow.

The output remains:

```text
ExtractionResult
```

### Acceptance Criteria

The Review screen does not need a separate image-specific architecture.

---

## Task 3.5 — Temporary Image Lifecycle

Do not persist the image as Reminder Card data by default.

### Acceptance Criteria

After:

```text
confirm
or
cancel
```

the product does not rely on retaining the source image.

---

## Task 3.6 — Image Error States

Handle:

- unreadable image
- unsupported image
- no Future Action
- multiple relevant dates

### Acceptance Criteria

The user can:

```text
Retry
Choose another image
Enter manually
```

where appropriate.

---

# 18. Phase 3 Exit Gate

Test representative image scenarios:

```text
Appointment screenshot
Event poster
Receipt with return deadline
Bill
Chat follow-up
Screenshot containing multiple dates
```

The product should extract only information useful to the Future Action.

---

# 19. Phase 4 — Voice Capture

## Goal

Validate fast Capture when typing or taking a screenshot is inconvenient.

---

## Task 4.1 — Add Voice Capture Entry

Provide a clear Voice action.

### Acceptance Criteria

Recording only begins after explicit user action.

---

## Task 4.2 — Microphone Permission

Request permission contextually.

### Acceptance Criteria

If denied:

```text
Text
Image
```

remain usable.

---

## Task 4.3 — Recording State

Clearly show:

```text
Recording
Stop
Cancel
```

### Acceptance Criteria

The user is never unsure whether WillDo is listening.

---

## Task 4.4 — Speech-to-Text

Convert captured speech into text.

The extraction pipeline should then reuse:

```text
Text → ExtractionService
```

### Acceptance Criteria

Voice does not require a separate Future Action schema.

---

## Task 4.5 — Transcription Failure

Provide recovery options.

### Acceptance Criteria

Users can:

```text
Retry voice
Edit transcript
Type instead
```

without losing the entire flow.

---

# 20. Phase 4 Exit Gate

Test:

- short reminder
- follow-up
- relative date
- noisy/incorrect transcription
- cancelled recording
- permission denied

---

# 21. Phase 5 — Native Reminder Loop

## Goal

Complete the core product promise:

```text
I captured it
→ it comes back when it matters
```

---

## Task 5.1 — Notification Service Abstraction

Create a service responsible for:

```text
schedule
cancel
reschedule
```

native notifications.

UI should not directly depend on the platform notification API.

---

## Task 5.2 — Contextual Notification Permission

Do not request notification permission during initial app launch.

Preferred moment:

```text
User confirms first Reminder Card
↓
WillDo explains reminder value
↓
Request permission
```

---

## Task 5.3 — Schedule Notification

When a confirmed Reminder Card contains a reminder schedule:

```text
save card
↓
schedule local notification
```

### Acceptance Criteria

The notification appears at the expected time on a real device.

---

## Task 5.4 — Persist Notification Identifier

Store the native notification identifier separately from WillDo's reminder identity.

### Acceptance Criteria

WillDo can later:

```text
cancel
reschedule
```

the correct notification.

---

## Task 5.5 — Edit Reminder Timing

When reminder timing changes:

```text
cancel previous notification
↓
schedule new notification
↓
update card
```

### Acceptance Criteria

Old notifications do not remain active accidentally.

---

## Task 5.6 — Mark Done

When a card becomes `done`:

```text
future notification
→ cancel
```

where applicable.

---

## Task 5.7 — Delete Card

Deletion should also cancel any associated notification.

---

## Task 5.8 — Notification Navigation

Where practical:

```text
Tap notification
↓
Open related Reminder Card
```

### Acceptance Criteria

The user does not lose context after opening a reminder.

---

## Task 5.9 — Notification Privacy Review

Define the MVP notification content.

Avoid unnecessarily exposing sensitive information.

### Acceptance Criteria

Notification design follows:

```text
docs/PRIVACY_RULES.md
```

---

# 22. Phase 5 Exit Gate

The full Future Action loop must work on a real device:

```text
Capture
→ AI
→ Review
→ Confirm
→ Save
→ Notification
→ Open Card
→ Mark Done
```

This is the first point at which the complete core WillDo experience exists.

---

# 23. Phase 6 — Integration and UX Polish

## Goal

Improve the validated core experience without expanding WillDo into a general productivity app.

P1 work should be prioritised based on observed user friction.

---

## Task 6.1 — Inbox Information Architecture

Evaluate whether grouping such as:

```text
Needs Attention
Today
Tomorrow
Coming Up
Later
```

improves scanning.

Do not add organisation complexity without evidence.

---

## Task 6.2 — Completed View

Allow completed items to remain accessible.

Possible behaviours:

```text
View
Restore
Delete
```

---

## Task 6.3 — Search

Implement lightweight search over relevant structured fields.

Possible fields:

- title
- next action
- provider/contact
- context

Semantic AI search is not required.

---

## Task 6.4 — Calendar Integration

For time-specific events, explore:

```text
Add to Calendar
```

The user must explicitly choose this action.

Do not automatically sync all Reminder Cards.

---

## Task 6.5 — Follow-Up Suggestions

Improve AI support for scenarios such as:

```text
If they haven't replied by Friday...
```

Suggestions must remain user-confirmed.

---

## Task 6.6 — Empty States

Refine empty states around the Capture-first positioning.

Avoid traditional:

```text
You have no tasks.
```

Prefer framing around:

```text
Nothing waiting for your attention.
```

---

## Task 6.7 — Onboarding

Create lightweight onboarding that explains:

```text
Capture now
WillDo structures it
You confirm
It comes back later
```

Avoid feature-tour onboarding.

---

## Task 6.8 — Accessibility Pass

Review:

- touch targets
- text scaling
- screen reader labels
- contrast
- status communication
- keyboard behaviour
- motion

---

## Task 6.9 — Error-State Pass

Review all important non-happy paths:

```text
Offline
AI failure
Permission denied
Invalid date
Storage failure
Notification failure
Image failure
Voice failure
```

---

## Task 6.10 — Performance Pass

Review:

- app startup
- Inbox loading
- capture responsiveness
- AI loading state
- large image handling

Capture should feel fast even when AI processing itself requires time.

---

# 24. Phase 7 — MVP Validation Build

## Goal

Prepare a build suitable for real usability testing.

The focus is product validation, not public commercial release.

---

## Task 7.1 — Build Representative Test Cases

Prepare realistic but non-sensitive capture examples.

Examples:

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

## Task 7.2 — Instrument Product Metrics

Where appropriate, track privacy-safe events such as:

```text
capture_started
capture_submitted
extraction_completed
review_edited
reminder_confirmed
notification_opened
reminder_completed
```

Do not collect Reminder Card content in analytics.

---

## Task 7.3 — Measure Capture Friction

Important metrics may include:

```text
Time to Confirm
Number of interactions
Capture abandonment rate
```

---

## Task 7.4 — Measure Review Effort

Possible metrics:

```text
Fields edited per capture
% confirmed without editing
Clarification rate
Review duration
```

---

## Task 7.5 — Usability Test Core Flows

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

without assistance.

---

# 25. MVP Definition of Done

WillDo MVP is not complete merely because all screens exist.

The MVP is complete when the following workflow works reliably on a real mobile device:

```text
User encounters something they need to deal with later

↓

User captures it using:
Text
Image
or Voice

↓

WillDo identifies a Future Action

↓

User reviews and corrects the result

↓

User confirms

↓

Reminder Card is saved locally

↓

Native notification is scheduled

↓

The notification returns the Future Action to attention

↓

User opens the card

↓

User resolves and marks it Done
```

---

# 26. Functional Definition of Done

Before calling the MVP complete:

- [ ] Text Capture works
- [ ] Image Capture works
- [ ] Voice Capture works
- [ ] Real AI extraction works
- [ ] AI structured output is validated
- [ ] AI uncertainty can be handled
- [ ] Review allows correction
- [ ] Reminder Cards persist locally
- [ ] Inbox works after app restart
- [ ] Cards can be edited
- [ ] Cards can be completed
- [ ] Cards can be deleted
- [ ] Local notifications work
- [ ] Notification timing can be edited
- [ ] Notification opens relevant context where practical
- [ ] AI failure has manual fallback
- [ ] permission denial has graceful fallback
- [ ] original image is not retained by default
- [ ] raw voice recording is not retained by default
- [ ] no private AI secret exists in mobile client
- [ ] important error states are handled
- [ ] core flows work on a physical device

---

# 27. Product Definition of Done

Before treating the MVP as validated, users should demonstrate that they:

- understand what WillDo is for
- recognise Capture Moments in their own lives
- can capture something without learning a task-management system
- understand AI output
- can correct AI mistakes
- perceive the Review step as lightweight
- trust the confirmed reminder
- understand the Inbox
- notice the reminder when it returns
- can resolve the Future Action

---

# 28. Out-of-Scope During MVP Development

Do not implement unless the scope is explicitly changed:

```text
Authentication
Cloud database
Multi-device sync
Email scanning
Bank integration
Kanban
Projects
Habit tracking
Family sharing
Team features
Advanced analytics dashboard
Document storage vault
Original screenshot archive
General AI chatbot
Automatic actions without review
```

---

# 29. Codex Working Method

Codex should not be asked to:

```text
Build Phase 1
```

as a single task.

Each task should be small enough to:

```text
Understand
Implement
Test
Review
Commit
```

independently.

---

# 30. Codex Task Template

Use the following structure when assigning implementation work.

```markdown
## Task

[Describe one specific implementation task.]

## Context

Read:

- AGENTS.md
- docs/PRODUCT_BRIEF.md
- docs/MVP_SCOPE.md
- docs/DATA_SCHEMA.md
- docs/AI_EXTRACTION_SPEC.md
- docs/PRIVACY_RULES.md
- docs/UX_FLOWS.md
- docs/BUILD_PLAN.md

Also read any relevant existing source files before making changes.

## Goal

[Describe the user or technical outcome.]

## Requirements

- requirement 1
- requirement 2
- requirement 3

## Non-goals

Do not:

- add unrelated features
- refactor unrelated code
- change the data model without explicit reason
- introduce new dependencies unless necessary

## Acceptance Criteria

- criterion 1
- criterion 2
- criterion 3

## Verification

Before completing:

- run TypeScript checks
- run linting where configured
- verify relevant flow manually
- report any remaining limitations

## Output

Summarise:

1. what changed
2. which files changed
3. how it was verified
4. any assumptions or unresolved issues
```

---

# 31. Codex Change Rules

Codex should not independently change:

```text
Product positioning
MVP scope
Data schema
Privacy rules
Core UX flow
```

because an implementation is easier another way.

If implementation reveals a conflict with documentation:

```text
Stop
↓
Explain the conflict
↓
Propose options
↓
Update the decision intentionally
```

Do not silently change product behaviour.

---

# 32. Dependency Rule

Before adding a dependency, determine:

```text
What problem does it solve?
Is the platform already capable of solving it?
Is it actively required now?
What future maintenance does it introduce?
```

Avoid accumulating libraries simply because they make a small task slightly faster.

Dependencies that materially affect architecture should be documented.

---

# 33. Refactoring Rule

Refactoring is appropriate when it:

- removes duplication
- improves maintainability
- restores architectural boundaries
- reduces bugs
- supports the current development phase

Avoid large speculative refactors for hypothetical future features.

Cloud readiness should come from clean interfaces rather than building cloud infrastructure early.

---

# 34. Testing Strategy

Testing should grow with the project.

## Early Phases

Prioritise:

```text
Type correctness
Repository behaviour
Manual user-flow verification
```

## AI Phase

Add tests for:

```text
Output validation
Known extraction examples
Error handling
Date parsing assumptions
```

## Notification Phase

Test on a real device.

Simulators alone are not sufficient to validate real reminder behaviour.

## Validation Phase

Prioritise:

```text
Real user behaviour
Capture friction
Review effort
Reminder effectiveness
```

---

# 35. Physical Device Rule

Because WillDo depends on:

- camera
- microphone
- native notifications
- mobile interaction

major milestones should eventually be tested on a physical mobile device.

Do not rely exclusively on browser previews or simulators.

---

# 36. Development Checkpoint After Each Phase

After each phase, review:

### Product

```text
Does this still support Future Action Capture?
```

### UX

```text
Did we add unnecessary interaction?
```

### AI

```text
Is AI actually reducing user effort?
```

### Privacy

```text
Are we collecting or retaining new information?
```

### Architecture

```text
Did implementation bypass service/repository boundaries?
```

### Scope

```text
Did we accidentally add task-management features?
```

Record major changes in:

```text
docs/DECISIONS.md
```

---

# 37. Approximate Project Sequence

The intended project sequence is:

```text
Project definition
✓

Core documentation
✓

Phase 0
Project foundation

↓

Phase 1
Text + Mock AI

↓

Early interaction test

↓

Phase 2
Real AI

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

Cloud / accounts decision

↓

Public launch preparation
```

The plan may change based on product evidence.

---

# 38. Validation Before Expansion

Do not automatically proceed from MVP to:

```text
Login
Cloud
Subscription
Public App Store release
```

without first evaluating the core product.

Questions to answer include:

1. Are Capture Moments frequent enough?
2. Does WillDo reduce capture friction?
3. Does AI reduce manual structuring?
4. Is Review lightweight enough?
5. Are Image and Voice genuinely useful?
6. Do users notice WillDo reminders?
7. Do users return to the Inbox?
8. Does WillDo feel different from a normal reminder app?
9. Which parts of the product create repeated value?
10. What would users miss if WillDo disappeared?

---

# 39. Post-MVP Direction

If the core concept validates successfully, likely next-stage work may include:

```text
Authentication
Cloud persistence
Multi-device sync
Share Sheet
Widgets
Home-screen quick capture
Advanced notification actions
Calendar integration refinement
Optional source retention
```

These should be prioritised based on validated user needs.

---

# 40. Build North Star

At every stage of development, ask:

> Are we making it easier to capture something now and trust that it will come back when it matters?

The development plan exists to build and validate that experience before expanding WillDo into a larger product.