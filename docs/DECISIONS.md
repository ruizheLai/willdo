# WillDo — Product & Technical Decisions

> Status: Living Document  
> Last major update: 2026-07  
> Purpose: This document records important product, UX, AI, privacy, architecture, and technical decisions made during the development of WillDo.
>
> It exists to preserve the reasoning behind the product and prevent future development from drifting away from intentional decisions.
>
> This document should be updated whenever a decision materially affects:
>
> - product positioning
> - MVP scope
> - UX architecture
> - AI behaviour
> - data architecture
> - privacy
> - technical implementation

---

# 1. Decision Format

Major decisions should record:

```text
Decision
Why
Impact
Future consideration
```

A decision does not mean it can never change.

When a decision changes, preserve the reasoning behind the previous choice and record why the new direction is better.

Working assumptions should remain clearly separated from decisions.

---

# 2. Focus on Future Action Capture

## Decision

WillDo focuses on:

> **Future Action Capture (FAC)**

rather than general task management or broad life administration.

FAC describes the moment when a user encounters something they cannot or do not want to deal with now but needs to remember and act on later.

## Why

The original product exploration focused more broadly on life administration.

Competitor research showed that broad life-admin products often expand into:

- documents
- finance
- email
- household management
- task management
- dashboards

Traditional reminder and task apps already manage reminders well once users know what they want to create.

The stronger opportunity is the step before that:

```text
Unstructured everyday information
→ Future Action
```

Users currently reduce Capture Moment friction through behaviours such as:

- screenshots
- self-messages
- quick notes
- saved messages
- open browser tabs
- relying on memory

These methods make capture easy but follow-through unreliable.

## Impact

WillDo should prioritise:

```text
Capture
→ Understand
→ Review
→ Remind
→ Resolve
```

over:

```text
Projects
Boards
Priorities
Productivity systems
Complex organisation
```

---

# 3. Use WillDo as the Working Product Name

## Decision

The working product name is:

```text
WillDo
```

The underlying product framework remains:

```text
FAC — Future Action Capture
```

## Why

WillDo reflects the user's mindset:

> I cannot deal with this now, but I will do it later.

It is:

- short
- consumer-friendly
- action-oriented
- less technical than FAC
- broader than the word "Reminder"

## Impact

Product and development documentation should use:

```text
WillDo
```

as the working brand.

## Future Consideration

Before public release, review:

- trademark conflicts
- App Store availability
- domain availability
- user interpretation
- brand distinctiveness

---

# 4. WillDo Is Not a Generic To-Do App

## Decision

WillDo will not attempt to become a complete productivity or task-management system.

## Why

Traditional task applications already provide mature functionality for:

- task lists
- projects
- priorities
- recurring tasks
- labels
- goals
- team workflows

WillDo's differentiated value occurs earlier:

> Reducing the effort required to turn something encountered now into something that will return later.

## Impact

The MVP should not introduce:

- Kanban boards
- projects
- goals
- productivity scoring
- complex priority systems
- habit tracking
- team task assignment

Basic management exists only to support the Future Action lifecycle.

---

# 5. Capture Before Structure

## Decision

Users should provide information before being asked to organise it.

Preferred flow:

```text
Provide information
↓
AI proposes structure
↓
User reviews
```

Not:

```text
Choose category
↓
Enter title
↓
Choose date
↓
Choose reminder
↓
Add metadata
```

## Why

Capture Moments often occur when users:

- are busy
- are switching contexts
- do not want to organise information
- already have the information in another format
- have not fully decided what action is required

Asking for structure before capture recreates the friction WillDo is intended to remove.

## Impact

Primary capture methods are:

```text
Text
Image
Voice
```

without requiring metadata first.

---

# 6. AI Is an Enabling Layer, Not the Product Identity

## Decision

WillDo uses AI to reduce user work, but the default experience should not feel like an AI chatbot.

## Why

AI creates value by transforming:

```text
Interpret manually
→ structure manually
→ create reminder
```

into:

```text
Capture
→ AI structures
→ review
```

The product should be valuable because the workflow becomes easier, not merely because AI exists.

## Impact

AI should primarily:

- identify the Future Action
- extract relevant information
- interpret temporal information
- propose a next action
- suggest reminder timing
- identify uncertainty

The default interaction remains structured rather than conversational.

---

# 7. AI Proposes, User Decides

## Decision

AI output must not automatically become:

- a confirmed Reminder Card
- a scheduled notification
- an irreversible product action

The user must review and confirm the result.

## Why

AI may:

- misunderstand dates
- select the wrong date from an image
- misunderstand user intent
- infer unsupported actions
- misread transcription

Reminder accuracy directly affects trust.

## Impact

The required flow is:

```text
Capture
→ Extraction
→ Review
→ Confirm
→ Persist
→ Schedule
```

User-edited values become authoritative.

---

# 8. Prefer Uncertainty Over Guessing

## Decision

When important information is ambiguous, WillDo should expose uncertainty or ask a lightweight clarification question rather than silently guessing.

## Example

Input:

```text
Alex said they might know by Thursday or Friday.
Message them if I haven't heard anything.
```

Prefer:

```text
When should you follow up?

[Thursday]
[Friday]
[Another date]
```

instead of selecting Thursday without evidence.

## Impact

The AI contract includes:

- confidence
- uncertain fields
- clarification question

Clarification should normally use structured choices rather than extended chat.

---

# 9. Build Native Mobile From the Beginning

## Decision

WillDo is built as a native mobile application using:

```text
React Native
Expo
TypeScript
```

rather than starting as a mobile web application.

## Why

The long-term product depends heavily on mobile-native capabilities:

- local notifications
- camera
- photo selection
- microphone
- calendar access
- deep links
- future Share Sheet integration
- future widgets
- quick capture

The Capture Moment is primarily a mobile interaction.

## Impact

The product architecture should assume a native mobile environment from the beginning.

---

# 10. Use Expo as the React Native Framework

## Decision

WillDo uses:

```text
Expo
```

as the primary React Native application framework and native tooling ecosystem.

## Why

WillDo requires multiple native capabilities that are well aligned with the Expo ecosystem.

Using Expo reduces infrastructure and native configuration work during early development while preserving access to native capabilities when required.

The project should prioritise product validation over unnecessary platform setup.

## Impact

Current implementation direction includes Expo modules for:

```text
Routing
Local database
Image capture
Voice
Notifications
Calendar
```

Expo does not remove the need for architectural boundaries between application logic and native infrastructure.

---

# 11. Use Expo Router

## Decision

WillDo uses:

```text
Expo Router
```

as its routing system.

## Why

The application naturally contains routeable destinations such as:

```text
Inbox
Capture
Review
Reminder Detail
Completed
```

Future native interactions may need to open a specific Reminder Card directly.

Examples:

```text
Notification
→ /reminders/[id]

Future Share Sheet
→ Capture flow

Future widget
→ Reminder detail
```

A routing model designed around deep-linkable screens supports this direction.

## Impact

Do not introduce a second competing navigation architecture without a concrete reason.

Routes should preserve the ability to navigate to individual Reminder Cards by ID.

---

# 12. Use Expo Development Builds

## Decision

WillDo should move to:

```text
Expo Development Build
```

early in development rather than depending on Expo Go as the long-term development environment.

## Why

Later WillDo phases depend on increasingly native functionality and configuration.

Development Builds provide a development environment closer to the actual native application while still preserving Expo tooling.

## Impact

Expo Go may be used for early smoke testing, but:

```text
Development Build
```

is the primary direction for real feature development.

Native capabilities should eventually be verified on physical devices.

---

# 13. Develop Incrementally

## Decision

Native architecture does not justify building all native functionality immediately.

Development remains phased.

## Sequence

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

## Why

The core interaction and data architecture should be validated before layering on device integrations and infrastructure.

## Impact

Codex tasks should remain small and phase-specific.

Do not implement later-phase functionality merely because it is technically convenient.

---

# 14. Local-First MVP

## Decision

The initial MVP stores confirmed Future Actions locally and does not require an account.

## Why

This reduces:

- authentication work
- backend complexity
- infrastructure work
- onboarding friction
- early privacy exposure

It allows development to focus on validating the product itself.

## Impact

The initial MVP does not require:

- login
- cloud Reminder Card database
- account recovery
- multi-device sync

Existing Reminder Cards should remain available without internet access where practical.

---

# 15. Local-First Must Remain Cloud-Ready

## Decision

Local-first storage must be hidden behind a repository abstraction.

## Architecture

```text
UI
↓
CardRepository
↓
Persistence implementation
```

## Why

Future product validation may justify:

```text
Authentication
Cloud storage
Cross-device sync
```

The UI should not need major redesign when persistence changes.

## Impact

React Native components must not directly depend on:

- SQLite
- future Supabase tables
- storage-specific row formats

---

# 16. Use expo-sqlite for Reminder Card Persistence

## Decision

The initial structured local database uses:

```text
expo-sqlite
```

through:

```text
SQLiteCardRepository
```

## Why

Reminder Cards are structured records that will likely need:

- filtering
- sorting
- status queries
- date queries
- Completed views
- search
- updates
- deletion
- future migrations

SQLite is better suited to this data model than treating the entire product database as key-value JSON storage.

## Impact

The intended architecture is:

```text
Application
↓
CardRepository
↓
SQLiteCardRepository
↓
expo-sqlite
```

UI code should not contain SQL.

---

# 17. AsyncStorage Is Not the Main Reminder Database

## Decision

Do not use:

```text
AsyncStorage
```

as the primary Reminder Card persistence layer.

## Why

AsyncStorage is suitable for simple key-value state and preferences, but Reminder Cards represent structured domain data.

Using a relational local database provides a better foundation for the expected Inbox and lifecycle behaviour.

## Impact

AsyncStorage may still be used later for small values such as:

- local preferences
- simple onboarding state
- non-domain settings

when appropriate.

---

# 18. Do Not Introduce an ORM Initially

## Decision

The initial SQLite implementation will not use an ORM.

Start with:

```text
expo-sqlite
+
SQLiteCardRepository
```

## Why

The initial domain is small.

Likely early database needs include only a limited number of structured entities, such as:

```text
Reminder Cards
Reminder schedules
```

Introducing an ORM before database complexity exists adds:

- abstraction
- dependencies
- migration concepts
- agent complexity

without clear current value.

## Impact

Do not automatically introduce:

```text
Drizzle
Prisma
Realm
WatermelonDB
```

during the initial MVP.

## Future Consideration

Reconsider an ORM if:

- schema complexity increases significantly
- migrations become difficult
- database query duplication becomes substantial
- the persistence layer becomes difficult to maintain

---

# 19. Separate Capture, Extraction, and Persistent Data

## Decision

WillDo distinguishes:

```text
CaptureInput
ExtractionResult
ReminderCard
```

## Why

A screenshot, voice recording, or pasted message is not itself the long-term product record.

The long-term object is the confirmed Future Action.

Separating the stages improves:

- privacy
- AI experimentation
- architecture
- Review UX
- future cloud migration

## Impact

Temporary raw inputs should not automatically become persistent Reminder Card fields.

---

# 20. Original Images Are Temporary by Default

## Decision

Images used for Capture are not retained permanently by default.

## Why

WillDo exists to preserve:

```text
Future Action
```

not:

```text
Screenshot archive
```

Retaining every source image would increase:

- privacy risk
- storage requirements
- product complexity

and move WillDo toward a document-management product.

## Impact

Reminder Cards do not require a persistent original-image field.

Persistent source retention would require a new deliberate product decision.

---

# 21. Voice Is Capture, Not Recording Storage

## Decision

Voice input should produce:

```text
Transcript
→ Extraction
→ Future Action
```

Raw audio is not retained by default.

## Why

WillDo is not a voice-note or recording archive.

## Impact

The intended lifecycle is:

```text
Record
→ Transcribe
→ Extract
→ Review
→ Confirm
→ Discard unnecessary raw audio
```

---

# 22. Text, Image, and Voice Share One Core Model

## Decision

Text, image, and voice are input methods rather than separate reminder systems.

All should eventually produce:

```text
ExtractionResult
```

and enter:

```text
Review
→ Confirm
→ ReminderCard
```

## Why

The Future Action is the product object.

The format used to capture it should not fragment the architecture.

## Impact

Do not build:

```text
Text Reminder
Image Reminder
Voice Reminder
```

as separate domain models.

---

# 23. Multimodal Capture Is Part of the Complete MVP

## Decision

The complete MVP should support:

- text
- image
- voice

although development is incremental.

## Why

Different Capture Moments naturally produce different input types.

Examples:

```text
Screenshot
→ Image

Quick thought
→ Voice

Message or idea
→ Text
```

Testing only text would validate AI-assisted reminder creation but not the broader Capture-first product hypothesis.

---

# 24. Use expo-image-picker for Initial Image Capture

## Decision

The initial image capture/selection direction is:

```text
expo-image-picker
```

## Why

The MVP needs straightforward user-initiated access to:

- existing screenshots
- photos
- camera capture where appropriate

A standard Expo-supported path reduces unnecessary native implementation complexity.

## Impact

Image functionality should remain behind the Capture workflow rather than becoming a document-management subsystem.

## Future Consideration

This implementation may change if later requirements such as advanced camera control or Share Extensions require a different native approach.

---

# 25. Use expo-audio for Initial Voice Capture

## Decision

The initial voice recording direction is:

```text
expo-audio
```

## Why

Voice is intended as a lightweight capture entry point rather than a full audio product.

The application needs:

```text
Record
→ obtain audio
→ transcribe
→ discard unnecessary raw audio
```

rather than advanced audio production capabilities.

## Impact

Voice-specific implementation should remain separated from the Future Action extraction contract.

---

# 26. Native Notifications Are Required for the Complete MVP

## Decision

WillDo's complete MVP must support native local notifications.

Initial implementation direction:

```text
expo-notifications
```

behind:

```text
NotificationService
```

## Why

The promise is not:

> I saved something.

It is:

> It will come back when it matters.

Without reminder delivery, only the Capture half of the product is validated.

## Impact

The complete loop includes:

```text
Capture
→ Confirm
→ Persist
→ Schedule
→ Notification
→ Return to Card
→ Resolve
```

---

# 27. Notification Permission Is Contextual

## Decision

Do not request notification permission during initial onboarding by default.

Request it when the user understands why notifications matter.

## Why

Permissions requested without context:

- increase friction
- are easier to deny
- reduce trust
- provide unclear value

## Impact

The same contextual principle applies to:

- camera
- photo library
- microphone
- calendar

---

# 28. Notification Privacy Is Part of Product Privacy

## Decision

Do not assume that all Reminder Card information should appear on the lock screen.

## Why

Future Actions may contain:

- medical context
- bills
- landlord issues
- personal relationships
- private appointments

## Impact

Notification architecture should allow future variation such as:

```text
Full detail
Title only
Private reminder
```

Do not tightly couple Notification content to full Reminder Card contents.

---

# 29. One Capture Produces One Primary Future Action in MVP

## Decision

MVP v0.1 assumes:

```text
One Capture
→ One Primary Reminder Card
```

## Why

A Capture may contain multiple actions, but automatic splitting introduces additional:

- AI complexity
- Review complexity
- UX complexity

before the frequency of this need is known.

## Impact

When multiple actions are detected, WillDo may:

- identify a primary action
- ask the user which action matters

Automatic multi-card extraction is outside the initial MVP.

---

# 30. One Card Has One Primary Reminder in MVP

## Decision

MVP v0.1 supports one primary Reminder Schedule per Reminder Card.

## Why

Multiple reminders such as:

```text
7 days before
1 day before
2 hours before
```

add scheduling and UX complexity.

They are not necessary to validate the primary FAC hypothesis.

## Impact

The initial schema uses one primary reminder.

## Future Consideration

User testing should determine whether multiple reminders provide enough value to justify the complexity.

---

# 31. Target Time and Reminder Time Are Different

## Decision

Keep:

```text
targetAt
```

separate from:

```text
ReminderSchedule.scheduledAt
```

## Example

```text
Return deadline:
August 20

Reminder:
August 18
```

## Why

One describes when the underlying Future Action matters.

The other describes when WillDo returns it to attention.

## Impact

Do not merge these concepts into a generic single `date` field.

---

# 32. Preserve Date Precision

## Decision

WillDo must distinguish between:

```text
August 20
```

and:

```text
August 20 at 3:30 PM
```

## Why

A date-only source does not contain an exact time.

Product defaults must not be presented as extracted facts.

## Impact

The schema includes explicit time precision.

Reminder suggestion logic remains separate from source temporal information.

---

# 33. Categories Remain Lightweight

## Decision

Categories such as:

```text
appointment
deadline
bill
return
warranty
follow_up
```

are lightweight labels rather than separate product systems.

## Why

Separate category systems would quickly expand WillDo toward:

- bill management
- finance
- warranty management
- appointment management
- household management

## Impact

All categories remain based on the same core Reminder Card model.

---

# 34. Persist Minimum Useful Information

## Decision

WillDo stores only information that remains useful when the Future Action returns later.

## Why

AI may detect:

- addresses
- account numbers
- unrelated people
- phone numbers
- full document content

but these do not necessarily help the Future Action.

## Impact

Data minimisation applies across:

- AI extraction
- schema
- SQLite
- future cloud storage

The ability to extract a field does not justify persisting it.

---

# 35. Use ExtractionService as the AI Boundary

## Decision

Application UI accesses AI extraction through:

```ts
ExtractionService
```

rather than directly depending on an AI provider.

## Why

Phase 1 begins with:

```text
MockExtractionService
```

and later changes to real AI.

The AI provider may also change later because of:

- accuracy
- cost
- latency
- privacy
- multimodal quality

## Impact

The UI should depend only on:

```text
ExtractionResult
```

rather than provider-specific APIs.

---

# 36. Use Supabase Edge Functions as the Initial Real-AI Backend

## Decision

The initial Phase 2 real AI architecture is:

```text
WillDo App
↓
ExtractionService
↓
Supabase Edge Function
↓
AI Provider
```

## Why

A production mobile application must not embed reusable private AI provider credentials.

WillDo needs a small protected server-side boundary for:

- credentials
- model calls
- extraction API
- future backend-side validation or control

Supabase Edge Functions provide an initial server-side path without requiring a complete custom backend platform.

## Impact

Phase 2 introduces Supabase for AI infrastructure.

This decision does **not** automatically introduce:

```text
Supabase Auth
Supabase Postgres Reminder Cards
Cloud sync
Multi-device sync
```

into the MVP.

---

# 37. Supabase Is Not Yet the Product Database

## Decision

Using Supabase Edge Functions does not mean Reminder Cards migrate to Supabase during the initial MVP.

## Why

The local-first MVP should validate product value before adding account and sync complexity.

AI backend infrastructure and user-data persistence solve different problems.

## Impact

During initial MVP:

```text
Reminder Cards
→ SQLite

AI requests
→ Supabase Edge Function
```

These should remain separate concerns.

---

# 38. AI Secrets Must Not Live in the Mobile Client

## Decision

Reusable AI provider credentials must never be embedded directly in the distributed application.

## Why

Client-side application bundles cannot safely protect reusable private API credentials.

## Impact

Real model calls should go through protected server-side infrastructure.

Mock AI requires no backend.

---

# 39. Use Zod for Runtime Validation at External Boundaries

## Decision

WillDo uses:

```text
Zod
```

to validate important untrusted runtime data.

## Why

TypeScript provides compile-time type safety but does not guarantee that external runtime data actually matches those types.

Important boundaries include:

```text
AI response
Backend response
Future migrated data
External payload
```

AI is especially important because structured model output must not be trusted merely because the application expects a particular TypeScript interface.

## Impact

Preferred flow:

```text
External response
↓
Zod validation
↓
Trusted domain structure
↓
Application
```

Avoid:

```ts
response as ExtractionResult
```

as the primary validation strategy.

---

# 40. Zod Does Not Replace TypeScript Domain Types

## Decision

Use Zod selectively at runtime boundaries rather than around every internal application object.

## Why

Adding runtime schemas everywhere would introduce unnecessary duplication and complexity.

## Impact

Use:

```text
TypeScript
```

for internal type contracts.

Use:

```text
Zod
```

where runtime data cannot already be trusted.

---

# 41. Do Not Introduce Global State Management Initially

## Decision

The initial MVP does not use:

```text
Redux
Zustand
MobX
```

or another global state-management system by default.

## Why

Early WillDo flows are relatively small:

```text
Inbox
Capture
Review
Card Detail
```

Introducing global state infrastructure before an actual state problem exists creates unnecessary architecture.

## Impact

Start with:

```text
React local state
Custom hooks
Repository / Service boundaries
```

## Future Consideration

Introduce a dedicated state solution if actual application complexity demonstrates a need.

---

# 42. Do Not Introduce TanStack Query Initially

## Decision

The local-first MVP does not introduce TanStack Query simply as standard application infrastructure.

## Why

Initial Reminder Card data is local SQLite data rather than server-owned query state.

Adding a server-state library before cloud data exists adds complexity without solving a current problem.

## Future Consideration

Reconsider if WillDo later introduces substantial cloud-backed server state.

---

# 43. No Email or Bank Integration in MVP

## Decision

The MVP does not connect to:

- email accounts
- bank accounts

## Why

These integrations increase:

- privacy risk
- trust requirements
- technical complexity
- infrastructure

and move WillDo toward broad automated life-admin products.

## Impact

Capture remains explicitly initiated by the user.

---

# 44. No Automatic Device Scanning

## Decision

WillDo will not automatically:

- scan screenshots
- inspect the photo library
- monitor clipboard content
- monitor other applications
- continuously listen through the microphone

## Why

The product should feel:

```text
user-controlled
```

rather than:

```text
always watching
```

## Impact

Capture begins through explicit user action.

---

# 45. Calendar Integration Is Complementary

## Decision

Calendar integration is a supporting P1 feature rather than the core data model.

Initial implementation direction:

```text
expo-calendar
```

## Why

Calendars already manage events well.

WillDo solves the problem that happens before users know or decide that something belongs in a calendar.

## Impact

Appropriate Future Actions may offer:

```text
Add to Calendar
```

Not every Reminder Card should become an event.

---

# 46. Share Sheet Is Strategically Important but Post-Core

## Decision

Share Sheet capture is considered strategically aligned with the long-term product, but it is not a dependency of the first core MVP implementation.

## Why

The ideal future flow may be:

```text
See something
→ Share
→ WillDo
→ Understand
→ Review
→ Confirm
```

This could reduce Capture friction significantly.

However, it introduces additional system-level native complexity.

## Impact

The core product must work independently through:

```text
Text
Image
Voice
```

before advanced external entry points become required.

---

# 47. AI Failure Must Have a Manual Fallback

## Decision

Users must be able to create a Future Action when AI fails.

## Why

Failure may occur because of:

- connectivity
- backend errors
- model errors
- invalid structured output
- image problems
- transcription errors

The user's ability to remember something should not depend completely on AI availability.

## Impact

Failure states should support:

```text
Try Again
Enter Manually
```

Manual entry should remain lightweight.

---

# 48. Review Effort Is a Key AI Product Metric

## Decision

AI success should be measured partly by how much work remains after extraction.

Potential measures:

```text
Time to Confirm
Fields edited
Clarification rate
Confirmed without edits
Review duration
```

## Why

The purpose of AI is to reduce cognitive and interaction work.

Extracting more information is not automatically more valuable.

## Impact

AI optimisation should focus on:

> minimum reliable user effort

rather than:

> maximum extracted information

---

# 49. Reminder Effectiveness Matters Beyond Delivery

## Decision

The Future Action loop ends at resolution rather than notification delivery.

## Why

WillDo promises:

```text
Capture
→ Remember
→ Resolve
```

not simply:

```text
Capture
→ Store
```

## Impact

Future testing may evaluate:

```text
notification delivered
notification opened
card viewed
reminder completed
reminder rescheduled
```

without collecting private Reminder Card content.

---

# 50. Current Working Assumptions

The following are hypotheses rather than permanent decisions.

They should remain easy to test and revise.

---

## Assumption A — Initial Target Users

Initial users are expected to be:

> Young adults managing many small personal responsibilities across multiple apps and contexts.

The audience may later become narrower.

---

## Assumption B — Future Action Capture Is Frequent

WillDo assumes users encounter Future Action Capture moments frequently enough for a dedicated tool to provide repeated value.

This needs validation.

---

## Assumption C — Text, Image, and Voice All Add Value

The complete MVP currently includes all three capture methods.

Testing may show that some methods are substantially more valuable than others.

---

## Assumption D — One Capture → One Future Action Is Enough Initially

Real captures may contain multiple actions.

The frequency and importance of this behaviour need validation before increasing extraction and Review complexity.

---

## Assumption E — One Reminder per Card Is Enough Initially

Users may eventually want multiple reminder timings.

MVP begins with one to preserve simplicity.

---

## Assumption F — Inbox Is the Right Management Model

WillDo currently uses an Inbox-like model:

> Things waiting for future attention.

Testing may reveal a better structure such as:

- timeline
- Today / Upcoming
- attention-based grouping

---

## Assumption G — Native Notifications Are Visible Enough

A key original motivation is that existing reminder channels may be easy to ignore.

The MVP assumes:

```text
Native notification
+
WillDo Inbox
```

can provide enough visibility.

This needs testing.

Possible future solutions include:

- badges
- widgets
- repeated reminders
- notification actions
- stronger Inbox surfaces

---

## Assumption H — Users Accept Lightweight Review

The product assumes users are willing to spend a small amount of effort checking AI output in exchange for:

- accuracy
- control
- lower capture effort

If Review feels like rebuilding the reminder manually, the core product value weakens.

---

## Assumption I — AI Meaningfully Reduces Capture Friction

The central hypothesis is that:

```text
Capture
→ Review
→ Confirm
```

will feel meaningfully easier than manually interpreting and structuring an equivalent reminder.

This must be validated through real use rather than assumed.

---

## Assumption J — Original Source Retention Is Usually Unnecessary

The current privacy direction assumes that structured Future Action information normally provides enough context later.

Testing may reveal that users sometimes need the original screenshot or source information to understand the reminder.

If so, optional source retention may need to be explored deliberately.

---

# 51. Technical Decision Rule

Before introducing new architecture or infrastructure, ask:

1. What current problem does this solve?
2. Is the problem present in the current development phase?
3. Can an existing Expo or platform capability solve it?
4. Does it introduce a new abstraction?
5. Does it introduce a new dependency?
6. Does it make the MVP harder to understand or maintain?
7. Is it being added for a real requirement or hypothetical future scale?

Prefer the smallest architecture that protects the boundaries WillDo actually needs.

---

# 52. Product Decision Rule

Before changing the product direction, ask:

1. What user problem does the change solve?
2. Does it strengthen Future Action Capture?
3. Does it reduce Capture Moment friction?
4. Does it reduce cognitive effort?
5. Does it improve reminder effectiveness?
6. Does it introduce task-management complexity?
7. Does it introduce new privacy risk?
8. Is the change supported by user evidence?

WillDo should evolve through validated needs rather than feature accumulation.

---

# 53. Current Architecture Summary

The current intended MVP architecture is:

```text
React Native
+
Expo
+
TypeScript

↓

Expo Router

↓

UI / Feature Logic

↓

Domain boundaries

├── CardRepository
├── ExtractionService
└── NotificationService

↓

Implementations

├── SQLiteCardRepository
│   ↓
│   expo-sqlite
│
├── MockExtractionService
│
├── RealAIExtractionService
│   ↓
│   Supabase Edge Function
│   ↓
│   AI Provider
│
└── ExpoNotificationService
    ↓
    expo-notifications
```

Additional native directions:

```text
Image
→ expo-image-picker

Voice
→ expo-audio

Calendar
→ expo-calendar
```

External runtime data is validated where appropriate using:

```text
Zod
```

The initial MVP does not include:

```text
Auth
Cloud Reminder Card database
Cross-device sync
ORM
Global state library
```

unless later evidence changes the decision.

---

# 54. Current North Star

The current product North Star remains:

> Does this make it easier for users to capture something they need to deal with later and trust that it will return at the right time?

A technical or product feature should not be added merely because it is common, powerful, or easy for an AI coding agent to generate.

WillDo should optimise for the smallest reliable Future Action Capture experience.