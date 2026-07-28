# WillDo — Product & Technical Decisions

> Status: Living Document  
> Purpose: This document records important product, UX, AI, privacy, and technical decisions made during the development of WillDo.
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

Each major decision should record:

```text
Decision
Why
Impact
Future consideration
```

A decision does not mean it can never change.

If a decision changes, do not delete the original reasoning.

Instead, record the new decision and explain why it changed.

---

# 2. Product Positioning

## 2026-07 — Focus on Future Action Capture

### Decision

WillDo will focus on **Future Action Capture (FAC)** rather than general task management or life administration.

FAC describes the moment when a user encounters something they cannot deal with now but need to remember and act on later.

### Why

The original exploration focused broadly on life-admin management.

Competitor research showed that:

- general task management is already crowded
- life-admin platforms often expand into documents, finance, email, household management, and dashboards
- existing reminder apps are already strong at managing reminders once users know what reminder they want

The stronger opportunity is the step before traditional reminder management:

```text
Unstructured information
→ Future Action
```

Users frequently rely on temporary behaviours such as:

- screenshots
- self-messages
- saved messages
- quick notes
- leaving tabs open

because these methods require less effort at the Capture Moment.

WillDo should reduce the gap between temporary capture and future action.

### Impact

The product should prioritise:

```text
Capture
→ Understand
→ Review
→ Remind
```

over:

```text
Projects
Priorities
Boards
Task organisation
Productivity systems
```

Any proposed feature should be evaluated against FAC before being added.

---

# 3. Product Name

## 2026-07 — Use WillDo as the Working Product Name

### Decision

The working product name is:

```text
WillDo
```

### Why

The name reflects the core user mindset:

> I cannot deal with this now, but I will do it later.

It is:

- short
- consumer-friendly
- related to future action
- less technical than "FAC"
- broader than "Reminder"

### Impact

Internal product documentation and development may use:

```text
WillDo
```

The conceptual framework remains:

```text
FAC — Future Action Capture
```

### Future Consideration

The final public name is not locked.

Before commercial release, naming should be reviewed for:

- App Store availability
- domain availability
- trademark conflicts
- user interpretation

---

# 4. WillDo Is Not a Generic To-Do App

## 2026-07 — Do Not Compete on Task Management

### Decision

WillDo will not attempt to become a complete to-do or productivity application.

### Why

Products such as traditional reminder and task-management apps already provide mature functionality for:

- task lists
- priorities
- projects
- recurring tasks
- tags
- productivity systems

WillDo's differentiated value is reducing the effort required to create the Future Action in the first place.

### Impact

The MVP should not introduce:

- Kanban boards
- projects
- goals
- productivity scoring
- complex priorities
- habit tracking
- team task assignment

Basic card management exists only to support the Future Action lifecycle.

---

# 5. Capture Before Structure

## 2026-07 — Users Provide Information Before Organising It

### Decision

WillDo should not ask users to structure information before capture.

The preferred interaction is:

```text
Provide information
↓
AI proposes structure
↓
User reviews
```

rather than:

```text
Choose category
↓
Enter title
↓
Choose date
↓
Choose reminder
↓
Add details
```

### Why

The Capture Moment often occurs when users:

- are busy
- do not want to think about organisation
- have information in an existing format
- have not yet decided exactly what reminder they need

Structuring before capture recreates the friction WillDo is intended to remove.

### Impact

The primary entry points should accept:

- text
- images
- voice

without requiring metadata first.

---

# 6. AI Is an Enabling Layer, Not the Product Identity

## 2026-07 — Product-First, AI-Assisted

### Decision

WillDo will use AI extensively, but AI itself should not become the primary product experience.

The product should feel like a fast Capture tool rather than an AI chatbot.

### Why

The value of AI is not simply:

```text
AI classification
AI summarisation
AI generation
```

The value is that AI changes the workflow from:

```text
Interpret information manually
→ create structured reminder
```

to:

```text
Capture
→ AI structures
→ review
```

### Impact

AI should primarily:

- identify Future Actions
- extract relevant information
- interpret dates
- propose next actions
- suggest reminder timing
- identify uncertainty

The default UI should not be conversational.

Chat-like interaction should only appear when clarification is necessary.

---

# 7. AI Output Requires User Confirmation

## 2026-07 — AI Proposes, User Decides

### Decision

AI-extracted information must not automatically become a confirmed Reminder Card or scheduled notification.

The user must review and confirm the result.

### Why

AI may:

- misunderstand dates
- misinterpret actions
- select the wrong information from an image
- infer something the user did not intend

Reminder accuracy directly affects user trust.

### Impact

The core flow is:

```text
Capture
→ Extraction
→ Review
→ Confirm
→ Persist
→ Schedule
```

not:

```text
Capture
→ AI automatically schedules reminder
```

User-edited values become authoritative and should not be overwritten by AI.

---

# 8. AI Should Express Uncertainty Instead of Guessing

## 2026-07 — Prefer Clarification Over Hallucination

### Decision

When important information is ambiguous, WillDo should expose uncertainty or ask a short clarification question rather than silently guessing.

### Example

Input:

```text
Alex said they might know by Thursday or Friday.
Message them if I haven't heard anything.
```

WillDo should not arbitrarily choose Thursday.

Instead:

```text
When should you follow up?

[Thursday]
[Friday]
[Choose another date]
```

### Impact

The AI schema includes:

- overall confidence
- uncertain fields
- clarification question

Clarification UX should favour quick choices over open-ended conversation.

---

# 9. Native Mobile From the Beginning

## 2026-07 — Build WillDo as a Native Mobile App

### Decision

WillDo will be developed as a native mobile application using:

```text
React Native
Expo
TypeScript
```

rather than beginning as a Next.js mobile web app.

### Why

Future Action Capture primarily happens in mobile contexts.

Important future product capabilities include:

- native notifications
- camera access
- image selection
- microphone access
- calendar integration
- Share Sheet
- home-screen shortcuts
- widgets

A native foundation better supports the long-term Capture-first interaction model and avoids later rebuilding the entire UI layer.

### Impact

The MVP architecture and UI should assume a native mobile environment from the beginning.

---

# 10. Incremental Development Despite Native Architecture

## 2026-07 — Native Does Not Mean Build Everything Immediately

### Decision

Although WillDo starts as a native app, development will remain incremental.

### Development sequence

```text
Phase 1
Text → Mock AI → Review → Card → Inbox

Phase 2
Real text AI

Phase 3
Image capture

Phase 4
Voice capture

Phase 5
Native notifications

Phase 6
Integration and polish
```

### Why

Native system capabilities add complexity.

The core interaction and data model should be validated before adding multiple integrations.

### Impact

Codex tasks should remain small and phase-specific.

Native architecture should not be used as justification for building all device capabilities at once.

---

# 11. Local-First MVP

## 2026-07 — No Account Required for Initial MVP

### Decision

The initial MVP will store confirmed Reminder Cards locally and will not require account creation.

### Why

This reduces:

- implementation complexity
- authentication work
- database work
- privacy risk
- onboarding friction

It allows development to focus on validating the core product loop.

### Impact

The initial MVP does not require:

- login
- cloud database
- account recovery
- multi-device sync

Existing Reminder Cards should remain usable offline where practical.

---

# 12. Local-First but Cloud-Ready

## 2026-07 — Storage Must Be Abstracted

### Decision

Local-first does not mean the app architecture may depend directly on local storage.

All persistent Reminder Card operations must use a repository abstraction.

### Required pattern

```ts
export interface CardRepository {
  listCards(): Promise<ReminderCard[]>;

  getCard(
    id: string
  ): Promise<ReminderCard | null>;

  createCard(
    card: ReminderCard
  ): Promise<ReminderCard>;

  updateCard(
    id: string,
    updates: Partial<ReminderCard>
  ): Promise<ReminderCard>;

  deleteCard(
    id: string
  ): Promise<void>;
}
```

### Why

A future version may introduce:

```text
Authentication
Cloud storage
Supabase
Cross-device sync
```

without requiring the UI layer to be rewritten.

### Impact

React Native components must not directly read from or write to the underlying storage mechanism.

---

# 13. Capture, Extraction, and Reminder Cards Are Separate Objects

## 2026-07 — Separate Temporary Inputs From Persistent Product Data

### Decision

WillDo will distinguish between:

```text
CaptureInput
ExtractionResult
ReminderCard
```

### Why

A screenshot, voice recording, or pasted text is not the product record.

The persistent product object is the confirmed Future Action.

Separating these objects improves:

- architecture
- privacy
- AI experimentation
- UI clarity
- future cloud migration

### Impact

Raw inputs should not automatically become persistent Reminder Card fields.

---

# 14. Original Images Are Temporary by Default

## 2026-07 — WillDo Is Not a Screenshot Archive

### Decision

Images used for capture should not be retained by default after the capture workflow is completed.

### Why

The product exists to preserve:

```text
Future Action
```

not:

```text
Original document
```

Persisting every screenshot would:

- increase privacy risk
- increase storage complexity
- move WillDo toward screenshot management
- move WillDo toward document-vault competitors

### Impact

The core Reminder Card schema does not require an original image field.

Future image retention would require an explicit product and privacy decision.

---

# 15. Voice Is Capture, Not Audio Storage

## 2026-07 — Do Not Retain Voice Recordings by Default

### Decision

Voice capture should ultimately produce text and structured Future Action information.

Raw audio should not be treated as long-term product data.

### Why

WillDo is not:

- a voice-notes application
- a recording archive

### Impact

The intended flow is:

```text
Record
→ Transcribe
→ Extract
→ Review
→ Discard raw audio
```

---

# 16. Text, Image, and Voice Share the Same Core Flow

## 2026-07 — Capture Methods Are Inputs, Not Separate Products

### Decision

Text, image, and voice capture should all eventually produce the same:

```text
ExtractionResult
```

and enter the same:

```text
Review → Confirm → Reminder Card
```

flow.

### Why

The user's Future Action is the core product object.

The input format should not create separate product architectures.

### Impact

Avoid implementing:

```text
Text Reminder System
Image Reminder System
Voice Reminder System
```

as independent workflows.

---

# 17. Multimodal Capture Is Required for the Final MVP

## 2026-07 — Text Alone Is Not Sufficient Product Validation

### Decision

The final MVP should support:

- text
- image
- voice

although they will be implemented sequentially.

### Why

WillDo is positioned around Capture Moment convenience.

Different real-life moments naturally involve different input formats.

Examples:

```text
Screenshot
→ image

Quick thought while walking
→ voice

Copied message
→ text
```

Testing only text would validate AI-assisted reminder creation, but not the broader Capture-first hypothesis.

### Impact

Text is Phase 1.

Image and voice remain required parts of the complete MVP.

---

# 18. Native Notifications Are Required for the MVP

## 2026-07 — Capture Must Eventually Return to Attention

### Decision

The complete MVP must support native local notifications.

### Why

WillDo's promise is not merely:

> I saved it.

It is:

> I know it will come back when it matters.

Without reminder delivery, the product only validates capture and storage.

### Impact

The complete loop must include:

```text
Capture
→ Confirm
→ Schedule
→ Notification
→ Card
→ Resolve
```

Cloud push infrastructure is not required for the local-first MVP.

---

# 19. Notification Permission Should Be Contextual

## 2026-07 — Do Not Ask for All Permissions During Onboarding

### Decision

Notification permission should be requested when the user creates a reminder and understands why the permission is useful.

The same contextual principle applies to:

- camera
- photos
- microphone
- calendar

### Why

Requesting multiple permissions immediately:

- increases onboarding friction
- provides little context
- may reduce trust
- encourages denial

### Impact

Permissions should be requested at the moment the related capability is used.

---

# 20. Notification Privacy Is Part of Product Privacy

## 2026-07 — Do Not Assume Full Card Content Belongs on the Lock Screen

### Decision

Notification design must consider that reminder content may be personally sensitive.

### Why

A Reminder Card may contain:

```text
medical appointment
bill amount
landlord issue
personal follow-up
```

Displaying complete content on a lock screen could expose information unintentionally.

### Impact

Notification architecture should allow future variation between:

```text
Full detail
Title only
Private notification
```

The initial implementation may use a reasonable default, but should not hard-code assumptions that full card data must always appear.

---

# 21. One Capture → One Primary Future Action for MVP

## 2026-07 — Simplify Multi-Action Inputs

### Decision

MVP v0.1 assumes that one capture produces one primary Reminder Card.

### Why

A single input may contain multiple actions:

```text
Friday go to Costco, buy milk, and return headphones.
```

Automatically splitting one capture into multiple cards introduces:

- extraction complexity
- review complexity
- UI complexity

before real user need has been validated.

### Impact

If multiple actions are detected, WillDo may:

- identify one primary action
- ask the user which action matters

Automatic multi-card creation is outside the initial MVP.

### Future Consideration

User research should determine how frequently multi-action captures actually occur.

---

# 22. One Card → One Primary Reminder for MVP

## 2026-07 — Start With One Reminder Schedule

### Decision

Each Reminder Card supports one primary scheduled reminder in MVP v0.1.

### Why

Multiple reminders such as:

```text
1 week before
1 day before
2 hours before
```

introduce additional UI and notification complexity.

The primary hypothesis does not require this functionality.

### Impact

The schema supports one primary:

```text
ReminderSchedule
```

per card.

Multiple reminders may be introduced after testing.

---

# 23. Target Time and Reminder Time Are Different

## 2026-07 — Separate `targetAt` From `scheduledAt`

### Decision

The date when something matters must remain separate from the date when WillDo reminds the user.

### Example

```text
Return deadline:
August 20

Reminder:
August 18
```

### Why

These represent different concepts.

Combining them into one date field would create ambiguity across:

- deadlines
- appointments
- follow-ups
- reminder scheduling

### Impact

The data model uses:

```text
targetAt
```

for the Future Action and:

```text
ReminderSchedule.scheduledAt
```

for notification timing.

---

# 24. Preserve Date Precision

## 2026-07 — Do Not Invent Exact Times

### Decision

WillDo should distinguish between:

```text
August 12
```

and:

```text
August 12 at 3:30 PM
```

### Why

A source that specifies a date does not automatically specify a time.

AI or system defaults should not be represented as source facts.

### Impact

The schema includes:

```text
TimePrecision
```

with values such as:

```text
exact
date_only
approximate
unknown
```

Product defaults may still suggest a notification time separately.

---

# 25. Categories Remain Lightweight

## 2026-07 — Do Not Build Separate Systems Around Each Category

### Decision

Categories such as:

```text
appointment
bill
return
warranty
follow_up
```

are labels, not independent product modules.

### Why

Creating different domain systems would quickly expand WillDo into:

- bill management
- warranty management
- appointment management
- finance management
- household management

### Impact

All categories remain based on the same core:

```text
ReminderCard
```

structure.

Category-specific experiences may be explored later only if research demonstrates strong value.

---

# 26. WillDo Should Persist Minimum Useful Information

## 2026-07 — Do Not Store Everything AI Can Extract

### Decision

Persistent Reminder Cards should contain only information useful when the Future Action returns later.

### Why

AI may detect:

- addresses
- account numbers
- phone numbers
- names
- unrelated document details

but the user may only need:

```text
What
When
Next action
Reminder
```

### Impact

Data minimisation applies to:

- AI extraction
- schema design
- local storage
- future cloud storage

The ability to extract a field does not justify persisting it.

---

# 27. No Email or Bank Integration in the MVP

## 2026-07 — Capture Must Remain User-Controlled

### Decision

WillDo will not connect to:

- email accounts
- bank accounts

during the MVP.

### Why

These integrations:

- increase privacy risk
- increase user trust requirements
- increase technical complexity
- move the product toward competitors such as automated life-admin platforms
- reduce the focus on user-initiated Capture Moments

### Impact

Information enters WillDo only through explicit user capture.

---

# 28. No Automatic Device Scanning

## 2026-07 — WillDo Does Not Monitor the User

### Decision

The MVP will not automatically:

- inspect screenshots
- scan photo libraries
- read clipboard contents
- monitor apps
- listen in the background

### Why

WillDo should be:

```text
user-controlled
```

rather than:

```text
always watching
```

### Impact

Capture begins only after explicit user action.

---

# 29. Calendar Integration Is Complementary

## 2026-07 — Do Not Replace the System Calendar

### Decision

Calendar integration may be added for appropriate Future Actions, but WillDo will not attempt to become a calendar replacement.

### Why

Calendar applications already perform event management well.

WillDo's value occurs primarily before the calendar becomes useful.

### Impact

P1 may offer:

```text
Add to Calendar
```

for appropriate cards such as:

- appointments
- events
- inspections
- bookings

Not every Reminder Card should become a calendar event.

---

# 30. Share Sheet Is Strategically Important but Post-Core MVP

## 2026-07 — Validate Capture Before Building Advanced Entry Points

### Decision

Native Share Sheet integration is highly aligned with WillDo's long-term Capture-first vision but is not required for the first core implementation.

### Why

An ideal future flow is:

```text
See something
→ Share
→ WillDo
→ Review
→ Confirm
```

However, Share extensions introduce additional native complexity.

The core Capture model should work before advanced system entry points are implemented.

### Impact

Share Sheet integration remains an important post-core direction rather than an initial development blocker.

---

# 31. AI Provider Must Be Abstracted

## 2026-07 — UI Must Not Depend on a Specific Model

### Decision

AI extraction should be accessed through an abstraction such as:

```ts
export interface ExtractionService {
  extract(
    input: CaptureInput,
    context: ExtractionContext
  ): Promise<ExtractionResult>;
}
```

### Why

The project will initially use:

```text
MockExtractionService
```

and later use a real AI model.

Future model choice may also change based on:

- accuracy
- cost
- latency
- privacy
- multimodal performance

### Impact

The UI should not know which AI provider performs extraction.

Mock and production extraction must return compatible data structures.

---

# 32. AI Secrets Must Not Live in the Mobile Client

## 2026-07 — Production AI Calls Require Protected Infrastructure

### Decision

Reusable private AI API credentials must not be embedded directly in the distributed mobile app.

### Why

A native application bundle cannot safely protect a reusable provider secret.

### Impact

Production architecture should eventually resemble:

```text
WillDo App
↓
WillDo Backend / Edge Function
↓
AI Provider
```

Phase 1 mock development does not require backend infrastructure.

---

# 33. AI Failure Must Have a Manual Fallback

## 2026-07 — WillDo Must Remain Usable Without Successful AI Extraction

### Decision

Users must be able to create a Future Action manually when AI extraction fails.

### Why

AI may fail due to:

- network issues
- model errors
- unreadable images
- transcription errors
- invalid structured output

The user's ability to remember something should not depend entirely on AI availability.

### Impact

Failure states should provide:

```text
Try Again
Enter Manually
```

Manual creation should remain lightweight.

---

# 34. Review Effort Is a Key AI Product Metric

## 2026-07 — Measure User Work, Not Extraction Volume

### Decision

One of the primary AI success measures will be how much work remains after extraction.

Potential metrics include:

```text
Time to Confirm
Fields edited per capture
Clarification rate
% confirmed without editing
Capture abandonment
```

### Why

The purpose of AI is to reduce cognitive and interaction effort.

Extracting more information does not necessarily create more value.

### Impact

AI optimisation should prioritise:

> minimum reliable user effort

rather than:

> maximum extracted fields

---

# 35. Reminder Effectiveness Matters Beyond Notification Delivery

## 2026-07 — The Product Loop Ends at Resolution

### Decision

WillDo should evaluate whether reminders successfully return Future Actions to attention, not simply whether notifications were scheduled.

### Future signals may include:

```text
notification delivered
notification opened
card viewed
card completed
reminder rescheduled
```

### Why

The complete promise is:

```text
Capture
→ Remember
→ Resolve
```

not merely:

```text
Capture
→ Store
```

### Impact

Future testing should observe behaviour after notification delivery.

---

# 36. Current Working Assumptions

The following are not permanent product decisions.

They should be tested.

---

## Assumption A — Primary Users

Initial target users are:

```text
young adults managing many small personal responsibilities
```

This may later narrow.

---

## Assumption B — Capture Frequency

WillDo is expected to support relatively frequent Capture Moments rather than only occasional major life-admin events.

This assumption supports the Capture-first direction.

---

## Assumption C — Text, Image, and Voice All Provide Value

All three capture methods are currently considered valuable.

Research may reveal that one or two methods provide substantially more value than the others.

---

## Assumption D — One Primary Reminder Is Enough for MVP

Some users may eventually require multiple reminder timings.

This should be tested before increasing complexity.

---

## Assumption E — Inbox Is the Correct Management Model

The current concept uses an Inbox-like home experience because it supports the idea:

> Things waiting for future attention.

The final organisation model should be validated through usability testing.

---

## Assumption F — Local Notification Visibility Is Sufficient

The initial native MVP will use system notifications.

Research should determine whether reminder visibility requires additional mechanisms such as:

- persistent app inbox
- badges
- widgets
- notification actions
- repeated reminders

---

# 37. Decision Rule

Before changing the product direction, ask:

1. What user problem does the change solve?
2. Does it strengthen Future Action Capture?
3. Does it reduce Capture Moment friction?
4. Does it reduce cognitive effort?
5. Does it improve reminder effectiveness?
6. Does it introduce unnecessary task-management complexity?
7. Does it introduce new privacy risk?
8. Has user research demonstrated the need?

WillDo should evolve through validated user needs rather than feature accumulation.

---

# 38. Current North Star

The current product North Star remains:

> Does this make it easier for users to capture something they need to deal with later and trust that it will return at the right time?

If a feature mainly makes WillDo a more complex task manager without improving this journey, it should not be added by default.