# WillDo

WillDo is a **capture-first AI mobile app** for things you cannot deal with now but do not want to forget later.

Instead of asking users to manually create and structure reminders, WillDo helps turn everyday information into reviewable Future Actions.

```text
Capture
→ Understand
→ Review
→ Confirm
→ Remind
→ Resolve
```

---

## Product Idea

People often encounter information that matters later but is inconvenient to deal with immediately.

Examples:

- a screenshot with a return deadline
- an appointment confirmation
- a message that needs a follow-up next week
- a subscription that should be cancelled before renewal
- an event registration deadline
- a quick thought that should come back tomorrow

Common workarounds include:

- screenshots
- self-messages
- quick notes
- saved messages
- browser tabs
- relying on memory

These methods make capture easy, but follow-through unreliable.

WillDo focuses on the moment between:

> "I need to deal with this later."

and:

> "It's captured, and I trust that it will come back when it matters."

This product concept is referred to internally as:

**Future Action Capture (FAC)**.

---

## Product Principle

WillDo follows a simple rule:

> **Capture before structure.**

Users should be able to provide information in the form they already have.

For example:

```text
"Need to cancel my gym membership before August 12."
```

WillDo may turn that into:

```text
Cancel gym membership

Deadline
Aug 12

Reminder
Aug 10 · 9:00 AM

Next action
Cancel before renewal
```

The user reviews and confirms the result before anything is saved or scheduled.

---

## Core Capture Methods

The complete MVP is designed around three primary capture methods:

```text
Text
Image
Voice
```

All three should eventually enter the same core workflow:

```text
CaptureInput
↓
AI Extraction
↓
Review
↓
ReminderCard
```

The input format should not create separate reminder systems.

---

## What WillDo Is Not

WillDo is not intended to become:

- a generic to-do app
- a Kanban board
- a project-management system
- a habit tracker
- a productivity dashboard
- a notes app
- a calendar replacement
- a document vault
- an email assistant
- a finance platform
- a general-purpose AI assistant

The product should remain focused on reducing the effort required to capture and return to Future Actions.

---

## MVP

The complete MVP should support:

- text capture
- image capture
- voice capture
- AI-powered structured extraction
- review and editing before confirmation
- Reminder Cards
- lightweight Inbox
- local persistence
- native local notifications
- card editing
- mark as done
- deletion
- graceful AI failure handling
- privacy-conscious raw-input handling

The MVP is:

```text
Native mobile
Local-first
Account-free
Cloud-ready
```

Authentication, cloud sync, and multi-device support are intentionally deferred until the core product is validated.

---

## Tech Stack

### App

- React Native
- Expo
- TypeScript
- Expo Router

### Local Persistence

- `expo-sqlite`
- `CardRepository` abstraction

Reminder Cards are stored locally during the initial MVP.

The UI should not depend directly on SQLite so the persistence layer can evolve later.

### Runtime Validation

- Zod

Zod is used at untrusted runtime boundaries such as AI or backend responses.

TypeScript remains the primary internal type system.

### AI

The application uses an `ExtractionService` abstraction.

Development begins with:

```text
MockExtractionService
```

and later moves to:

```text
WillDo App
↓
Supabase Edge Function
↓
AI Provider
```

Reusable private AI provider credentials must not be embedded in the mobile client.

### Native Capabilities

Current implementation direction:

```text
Image
→ expo-image-picker

Voice
→ expo-audio

Notifications
→ expo-notifications

Calendar
→ expo-calendar
```

Advanced system-level capture such as Share Sheet integration may be explored after the core MVP.

---

## Architecture

The intended application structure separates product logic from infrastructure.

```text
UI / Feature Logic
↓
Domain Interfaces
├── CardRepository
├── ExtractionService
└── NotificationService
↓
Infrastructure
├── SQLiteCardRepository
├── MockExtractionService
├── RealAIExtractionService
└── ExpoNotificationService
```

This allows implementation details to change without tightly coupling them to the UI.

---

## Data Flow

WillDo separates temporary capture data from confirmed product data.

```text
CaptureInput
↓
ExtractionResult
↓
User Review
↓
ReminderCard
```

### CaptureInput

Temporary user-provided content such as:

- text
- screenshot
- photo
- voice transcript

### ExtractionResult

AI-proposed structured information.

It may include:

- title
- date/time
- deadline
- location
- amount
- provider/contact
- next action
- reminder suggestion
- uncertainty

### ReminderCard

The confirmed Future Action stored by WillDo.

Raw screenshots and voice recordings are not persistent product data by default.

---

## AI Principle

AI should remove work rather than add another interaction layer.

The default experience should be:

```text
Capture
→ Processing
→ Structured result
→ Review
```

not a chatbot conversation.

AI output is always a proposal.

The user remains authoritative.

If important information is ambiguous, WillDo should prefer clarification over guessing.

---

## Privacy Direction

WillDo follows a user-controlled, data-minimising model.

Core principles include:

- capture is user-initiated
- original images are temporary by default
- raw voice recordings are temporary by default
- only useful Future Action information should be persisted
- no automatic screenshot scanning
- no email scanning
- no bank integration
- no continuous microphone listening
- no background clipboard monitoring
- AI output must be reviewed before persistence
- users can delete saved Reminder Cards

The initial MVP is local-first, but real AI processing may still require transmitting selected capture data to a protected external service.

See:

```text
docs/PRIVACY_RULES.md
```

for detailed rules.

---

## Development Plan

Development is incremental.

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

The first major product milestone is:

```text
Inbox
→ Text Capture
→ Mock Extraction
→ Review
→ Confirm
→ SQLite
→ Inbox
```

The complete core loop later becomes:

```text
Capture
→ AI
→ Review
→ Confirm
→ Save
→ Notify
→ Return
→ Resolve
```

See:

```text
docs/BUILD_PLAN.md
```

for implementation tasks and acceptance criteria.

---

## Documentation

The `/docs` directory is the product and development source of truth.

```text
docs/
├── PRODUCT_BRIEF.md
├── MVP_SCOPE.md
├── DATA_SCHEMA.md
├── AI_EXTRACTION_SPEC.md
├── PRIVACY_RULES.md
├── UX_FLOWS.md
├── DECISIONS.md
└── BUILD_PLAN.md
```

### `PRODUCT_BRIEF.md`

Defines what WillDo is, the problem it addresses, product principles, and core hypotheses.

### `MVP_SCOPE.md`

Defines what belongs in the MVP and what is intentionally excluded.

### `DATA_SCHEMA.md`

Defines core product data structures such as:

```text
CaptureInput
ExtractionResult
ReminderCard
ReminderSchedule
```

### `AI_EXTRACTION_SPEC.md`

Defines how AI should interpret captures, structure output, handle ambiguity, and avoid unsupported assumptions.

### `PRIVACY_RULES.md`

Defines privacy and data-handling constraints.

### `UX_FLOWS.md`

Defines the intended interaction flows from Capture through Resolve.

### `DECISIONS.md`

Records important product and technical decisions and the reasoning behind them.

### `BUILD_PLAN.md`

Defines implementation phases, development tasks, acceptance criteria, and MVP completion requirements.

---

## Agent Development

This repository is designed to be developed with coding agents such as Codex.

Agents must follow:

```text
AGENTS.md
```

before making implementation changes.

Key rules include:

- read relevant documentation first
- do not silently expand MVP scope
- do not change product behaviour for implementation convenience
- preserve repository and service boundaries
- validate external data
- avoid unnecessary dependencies
- verify work before declaring completion

---

## Current Development Direction

Current architecture:

```text
React Native
+
Expo
+
TypeScript
+
Expo Router

↓

Local-first application

↓

expo-sqlite

↓

CardRepository

↓

Mock AI first

↓

Real AI through protected backend

↓

Native notifications

↓

MVP validation
```

The project should optimise for:

> building the smallest reliable Future Action Capture experience

rather than maximising feature count or architectural complexity.

---

## North Star

> Does WillDo make it easier to capture something now and trust that it will come back when it matters?

Every major product and technical decision should support that question.