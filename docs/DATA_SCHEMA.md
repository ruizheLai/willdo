# WillDo — Data Schema

> Status: Draft v0.2  
> Purpose: This document defines the core domain data models used by WillDo.
>
> It acts as the shared contract between Capture, AI extraction, Review, persistence, reminders, and future cloud infrastructure.
>
> This document defines the **product domain model**, not the physical SQLite database schema.
>
> The schema should remain intentionally small during the MVP. New persistent fields should only be added when they support a validated product need.

---

# 1. Data Model Principles

WillDo separates the Future Action lifecycle into three primary data stages:

```text
CaptureInput
↓
ExtractionResult
↓
ReminderCard
```

These are intentionally different objects.

They should not be merged for implementation convenience.

---

## 1.1 CaptureInput

Temporary raw information supplied by the user.

Examples:

- typed text
- pasted text
- screenshot
- photo
- voice transcript

Raw images and audio should not become persistent product data by default.

---

## 1.2 ExtractionResult

A temporary AI interpretation of the Capture.

It may contain:

- proposed structured fields
- suggestions
- uncertainty
- missing information
- clarification requirements

The user may edit this information before confirmation.

---

## 1.3 ReminderCard

The confirmed persistent representation of a Future Action.

Only after user confirmation should information enter the long-term Reminder Card model.

The product record is:

```text
Future Action
```

not:

```text
Original screenshot
Original voice recording
Raw AI response
```

---

# 2. Domain Model vs Persistence Model

The types in this document represent WillDo's domain model.

They should remain independent of the underlying persistence technology.

Current MVP architecture:

```text
Application
↓
CardRepository
↓
SQLiteCardRepository
↓
expo-sqlite
```

This means:

```text
ReminderCard
```

is not necessarily identical to:

```text
SQLite row
```

The repository implementation is responsible for translating between them where necessary.

---

## 2.1 Domain Model

Describes what the product means.

Examples:

```text
ReminderCard
ReminderSchedule
CaptureInput
ExtractionResult
```

---

## 2.2 Persistence Model

Describes how the data is physically stored.

Examples may include:

```text
SQLite tables
columns
foreign keys
serialized fields
indexes
database migrations
```

Persistence details should not leak into UI components or redefine product meaning.

---

# 3. Capture Source Type

Describes how the user originally captured the information.

```ts
export type CaptureSourceType =
  | "text"
  | "image"
  | "voice";
```

This describes the Capture method only.

It should not create separate Future Action models.

All Capture types eventually enter the same:

```text
Extraction
→ Review
→ ReminderCard
```

flow.

---

# 4. Reminder Category

Categories are lightweight labels used primarily for:

- presentation
- AI interpretation
- future simple filtering

They should not become separate management systems.

```ts
export type ReminderCategory =
  | "appointment"
  | "deadline"
  | "bill"
  | "return"
  | "warranty"
  | "follow_up"
  | "general";
```

---

## 4.1 Category Intent

| Category | Example |
| --- | --- |
| `appointment` | Dentist appointment |
| `deadline` | Event registration closes Friday |
| `bill` | Electricity bill due August 12 |
| `return` | Headphones return window ends August 20 |
| `warranty` | Laptop warranty expires next year |
| `follow_up` | Contact property manager again next Wednesday |
| `general` | Call insurance tomorrow |

Categories should normally be suggested by AI rather than manually required before Capture.

Categories remain editable.

---

## 4.2 Category Rule

Categories must not create separate domain systems such as:

```text
Bill
Warranty
Return
Appointment
```

with completely different product architectures.

They remain variations of:

```text
ReminderCard
```

unless later user evidence justifies a different design.

---

# 5. Temporal Kind

Different Future Actions use dates differently.

An appointment time and a deadline may contain similar timestamps but represent different meanings.

```ts
export type TemporalKind =
  | "event"
  | "deadline"
  | "follow_up"
  | "reference"
  | "none";
```

---

## `event`

Something happens at the target time.

Example:

```text
Dentist appointment at 3:30 PM
```

---

## `deadline`

Something should happen by the target time.

Example:

```text
Return headphones before August 20
```

---

## `follow_up`

The user should revisit something around the target time.

Example:

```text
Contact landlord again next Wednesday
```

---

## `reference`

A date provides useful supporting context but is not necessarily itself the action time.

Example:

```text
Purchased laptop on August 12
```

when the Future Action concerns warranty information.

---

## `none`

No meaningful target time is known.

---

# 6. Time Precision

WillDo must preserve the difference between an exact time and incomplete temporal information.

```ts
export type TimePrecision =
  | "exact"
  | "date_only"
  | "approximate"
  | "unknown";
```

Examples:

```text
Friday at 3:30 PM
→ exact

August 12
→ date_only

Around the end of August
→ approximate

No useful date detected
→ unknown
```

WillDo must not invent precision that does not exist in the source.

---

# 7. Reminder Card Status

```ts
export type ReminderCardStatus =
  | "active"
  | "done"
  | "archived";
```

---

## `active`

The Future Action still requires attention.

---

## `done`

The user has resolved or completed the Future Action.

---

## `archived`

The card is retained but is no longer part of the active Inbox.

---

## Draft State

`draft` should not normally be a persisted Reminder Card status.

Draft behaviour belongs to:

```text
Capture
Extraction
Review
```

before confirmation.

---

# 8. CaptureInput

`CaptureInput` represents temporary user-provided information before structured extraction.

```ts
export type CaptureInput = {
  id: string;

  sourceType: CaptureSourceType;

  text?: string | null;

  imageUri?: string | null;

  voiceTranscript?: string | null;

  createdAt: string;
};
```

---

# 9. CaptureInput Rules

At least one relevant content field should exist according to `sourceType`.

---

## Text

```ts
{
  sourceType: "text",
  text: "Cancel gym membership before August 12"
}
```

---

## Image

```ts
{
  sourceType: "image",
  imageUri: "temporary-device-uri"
}
```

`imageUri` represents temporary access to the Capture source.

It should not be assumed to remain valid permanently.

---

## Voice

```ts
{
  sourceType: "voice",
  voiceTranscript:
    "Contact the property manager next Wednesday if they haven't replied"
}
```

Raw audio itself is not required in the domain model.

---

# 10. Capture Lifecycle

Raw Capture content should normally be temporary.

The application should not rely on retaining:

- original screenshots
- photos
- raw audio recordings
- intermediate model prompts
- raw model responses

after they are no longer required.

Conceptually:

```text
Capture
↓
Extract
↓
Review
↓
Confirm or Cancel
↓
Discard unnecessary raw input
```

---

# 11. Money Amount

Financial information is optional.

It should only be stored when it contributes to the Future Action.

```ts
export type MoneyAmount = {
  value: number;
  currency: string;
};
```

Example:

```ts
{
  value: 86.40,
  currency: "AUD"
}
```

WillDo should not evolve into an expense tracker simply because amounts can be extracted.

---

# 12. ReminderCardDraft

AI extraction produces a temporary proposed Future Action.

```ts
export type ReminderCardDraft = {
  title: string;

  category: ReminderCategory;

  temporalKind: TemporalKind;

  targetAt?: string | null;

  timePrecision: TimePrecision;

  timezone?: string | null;

  nextAction?: string | null;

  location?: string | null;

  amount?: MoneyAmount | null;

  contactOrProvider?: string | null;

  context?: string | null;

  suggestedReminderAt?: string | null;
};
```

A draft does not contain persistent lifecycle metadata such as:

```text
id
status
createdAt
completedAt
```

Those belong to the confirmed Reminder Card.

---

# 13. Extracted Facts vs Suggestions

The domain should preserve the conceptual difference between:

```text
information supported by the source
```

and:

```text
information suggested by WillDo
```

Example:

```text
Source:
Return deadline August 20

Extracted:
targetAt = August 20

Suggested:
remind August 18
```

Therefore:

```text
targetAt
```

and:

```text
suggestedReminderAt
```

represent different concepts.

---

# 14. AI Confidence

```ts
export type AIConfidence =
  | "high"
  | "medium"
  | "low";
```

Overall confidence may be useful for:

- development
- evaluation
- uncertainty UX
- AI quality measurement

It does not necessarily need to be prominently visible to users.

---

# 15. Reminder Draft Field

Fields that may be identified as uncertain:

```ts
export type ReminderDraftField =
  | "title"
  | "category"
  | "temporalKind"
  | "targetAt"
  | "timezone"
  | "nextAction"
  | "location"
  | "amount"
  | "contactOrProvider";
```

---

# 16. Extraction Uncertainty

```ts
export type ExtractionUncertainty = {
  field: ReminderDraftField;

  reason: string;

  alternatives?: string[];
};
```

Example:

```ts
{
  field: "targetAt",
  reason: "Two relevant dates were detected.",
  alternatives: [
    "2026-08-12",
    "2026-08-15"
  ]
}
```

WillDo should prefer uncertainty over unsupported guessing.

---

# 17. ExtractionResult

The AI extraction layer returns:

```ts
export type ExtractionResult = {
  draft: ReminderCardDraft;

  confidence: AIConfidence;

  uncertainFields: ExtractionUncertainty[];

  clarificationQuestion?: string | null;
};
```

`ExtractionResult` is temporary.

It should normally exist only during:

```text
Extraction
→ Review
```

until the user either:

```text
Confirms
or
Cancels
```

---

# 18. Runtime Validation

TypeScript types describe the application's expected data shape but do not guarantee that external runtime data is valid.

External data should be validated before becoming a trusted domain object.

The current validation direction uses:

```text
Zod
```

for important trust boundaries.

Example:

```text
AI / Backend Response
↓
Zod validation
↓
ExtractionResult
↓
Review
```

---

## 18.1 Zod Is Not the Domain Model

The canonical product concepts remain the domain types defined in this document.

Zod schemas are runtime validation tools.

Do not allow:

```text
validation implementation
```

to redefine:

```text
product semantics
```

without an explicit schema decision.

---

## 18.2 Initial Validation Priority

The highest-priority runtime validation target is:

```text
ExtractionResult
```

because AI and backend responses are external data.

Future validation may also apply to:

- migrated persisted data
- cloud API responses
- imported data

where necessary.

---

# 19. Confirmed ReminderCard

After Review and confirmation, WillDo creates a persistent Reminder Card.

```ts
export type ReminderCard = {
  id: string;

  userId?: string | null;

  title: string;

  category: ReminderCategory;

  sourceType: CaptureSourceType;

  temporalKind: TemporalKind;

  targetAt?: string | null;

  timePrecision: TimePrecision;

  timezone?: string | null;

  nextAction?: string | null;

  location?: string | null;

  amount?: MoneyAmount | null;

  contactOrProvider?: string | null;

  context?: string | null;

  reminder: ReminderSchedule | null;

  status: ReminderCardStatus;

  createdAt: string;

  updatedAt: string;

  completedAt?: string | null;

  archivedAt?: string | null;

  deletedAt?: string | null;
};
```

---

# 20. ReminderCard Field Semantics

## `id`

Unique WillDo identifier for the Reminder Card.

Use UUID-compatible identifiers.

The ID should remain valid independently of:

- SQLite row identifiers
- native notification IDs
- future cloud database IDs where possible

---

## `userId`

```ts
userId?: string | null;
```

The local-first MVP does not require authentication.

For the initial MVP:

```text
userId = null
```

The application must not assume an authenticated user exists.

This field preserves future compatibility without requiring account infrastructure today.

---

## `title`

A short human-readable description of the Future Action.

Good:

```text
Cancel gym membership
```

Bad:

```text
Gym screenshot from July
```

The title should describe what requires future attention.

---

## `category`

A lightweight label.

It may support:

- recognition
- simple filtering
- AI interpretation

It should not determine large independent workflows.

---

## `sourceType`

Records how the Future Action originally entered WillDo:

```text
text
image
voice
```

This may later help evaluate which Capture methods create the most value.

---

## `temporalKind`

Describes what `targetAt` means.

Example:

```text
event
deadline
follow_up
reference
none
```

---

## `targetAt`

The primary temporal point associated with the Future Action.

Examples:

```text
Dentist appointment:
2026-08-18T15:30:00+10:00

Return deadline:
2026-08-20

Follow-up:
2026-08-14
```

The meaning of this value depends on:

```text
temporalKind
```

It is not the notification time.

---

## `timePrecision`

Describes the precision of the known target time.

This prevents the system from treating:

```text
August 12
```

as equivalent to:

```text
August 12 at 9:00 AM
```

---

## `timezone`

Where exact time matters, timezone should be preserved.

Example:

```text
Australia/Melbourne
```

The device timezone may be used as a working default when the source does not provide one.

Any later distinction between:

```text
extracted timezone
device-assumed timezone
```

should be added only if product needs justify it.

---

## `nextAction`

A concise description of what the user should do.

Example:

```text
Cancel the membership before renewal.
```

or:

```text
Attend dentist appointment.
```

The title and `nextAction` may sometimes be similar.

That is acceptable.

---

## `location`

Optional location relevant to resolving the Future Action.

Do not persist unrelated addresses simply because they appeared in a captured document.

---

## `amount`

Optional money value relevant to the Future Action.

Do not use it as the basis for building a general finance model.

---

## `contactOrProvider`

Optional person or organisation useful to the Future Action.

Examples:

```text
Alex
Property manager
City Dental Melbourne
Energy provider
```

---

## `context`

Optional short supporting information that helps the user understand the Future Action later.

Example:

```text
Property manager said the plumber should contact me this week.
```

Context should remain concise.

WillDo is not a notes archive.

---

# 21. ReminderSchedule

The time when a Future Action matters is separate from the time when WillDo reminds the user.

Example:

```text
Deadline
August 20

Reminder
August 18 at 9:00 AM
```

The reminder model is:

```ts
export type ReminderSchedule = {
  id: string;

  scheduledAt: string;

  status:
    | "scheduled"
    | "delivered"
    | "cancelled";

  nativeNotificationId?: string | null;

  createdAt: string;

  updatedAt: string;
};
```

---

# 22. Reminder Identity

The WillDo Reminder Schedule ID and native notification ID are different concepts.

```text
ReminderSchedule.id
→ WillDo domain identifier

nativeNotificationId
→ operating-system notification binding
```

The application must not treat the native notification ID as the business identity of the reminder.

This distinction supports:

- cancelling notifications
- rescheduling
- future infrastructure changes
- future sync

---

# 23. One Primary Reminder in MVP

For MVP v0.1:

```text
One Reminder Card
→ One Primary Reminder Schedule
```

This keeps the initial interaction and notification model simple.

Future versions may support:

```text
7 days before
1 day before
2 hours before
```

but multiple reminders are not required for the initial MVP.

---

# 24. Date and Time Format

Persistent timestamps should use ISO 8601-compatible strings.

Exact date-time example:

```text
2026-08-18T15:30:00+10:00
```

Date-only example:

```text
2026-08-20
```

System timestamps include:

```text
createdAt
updatedAt
completedAt
archivedAt
deletedAt
```

Do not automatically convert:

```text
August 12
```

into:

```text
August 12 at 9:00 AM
```

inside the domain model merely because notifications require a time.

Reminder scheduling is a separate decision.

---

# 25. Persisted Data

The MVP should persist confirmed product state such as:

- Reminder Cards
- Reminder Schedules
- Reminder Card status
- lifecycle timestamps

The current local persistence technology is:

```text
expo-sqlite
```

through:

```text
SQLiteCardRepository
```

---

# 26. Temporary Data

The following should remain temporary by default:

- uploaded images
- captured photos
- raw audio recordings
- abandoned Capture inputs
- AI prompts containing Capture content
- raw AI responses
- intermediate extraction processing data

Temporary data should be discarded when no longer required.

---

# 27. Deletion Model

The domain includes:

```ts
deletedAt?: string | null;
```

to allow future persistence implementations to support soft deletion.

However, the initial local MVP may use hard deletion if that is simpler.

UI code must not depend on whether persistence internally uses:

```text
Hard delete
or
Soft delete
```

That behaviour belongs behind the repository boundary.

---

# 28. SQLite Persistence

The initial local persistence implementation uses:

```text
expo-sqlite
```

This is an infrastructure decision rather than a change to the Reminder Card domain model.

Conceptually:

```text
ReminderCard
↓
CardRepository
↓
SQLiteCardRepository
↓
SQLite representation
```

---

# 29. SQLiteCardRepository

`SQLiteCardRepository` implements:

```ts
CardRepository
```

and is responsible for:

- CRUD operations
- converting database rows into domain objects
- converting domain objects into persistent representations
- hiding SQL from UI and feature code
- future local schema migration handling where required

Feature components should not know:

```text
table names
column names
SQL syntax
database migration details
```

---

# 30. SQLite Schema Guidance

The physical SQLite schema should support the domain model without becoming the source of product meaning.

An implementation may use tables such as:

```text
reminder_cards
reminder_schedules
```

but exact database structure belongs to implementation.

Do not update the product domain simply because a database representation is inconvenient.

Prefer adapting the persistence layer.

---

# 31. SQLite Query Needs

The initial database should support expected product operations such as:

```text
List active cards
Get card by ID
Create card
Update card
Mark card done
Delete card
List completed cards
Sort cards by relevant date
```

Later requirements may include:

```text
Search
Date-based Inbox grouping
Filtering
```

The database should not be over-designed for hypothetical future analytics or enterprise workloads.

---

# 32. SQLite Indexing

Indexes should be added when they support demonstrated query needs.

Potential future candidates may include:

```text
status
targetAt
createdAt
```

Do not create extensive indexing before query patterns exist.

---

# 33. Database Migrations

The database schema will evolve during development.

Local schema changes should eventually use a predictable migration process.

Migration code belongs to the persistence layer.

The domain model should not contain migration-specific fields purely to support database implementation.

Before changing persisted structure, consider:

- existing local data
- development builds already installed on devices
- backwards compatibility
- safe migration
- whether resetting development data is acceptable at the current phase

MVP development may allow simpler resets early, but this should not become the permanent migration strategy.

---

# 34. No ORM Initially

The initial persistence architecture is:

```text
expo-sqlite
+
SQLiteCardRepository
```

without an ORM.

Do not introduce an ORM merely to mirror the domain model automatically.

If later database complexity justifies one, that should become an explicit technical decision.

---

# 35. CardRepository Contract

UI components must not directly access SQLite.

Persistent Reminder Card operations should use:

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

---

# 36. Repository Responsibilities

`CardRepository` defines what the application needs from persistence.

It should not expose:

```text
SQL
SQLite transactions
Supabase queries
database rows
storage keys
```

to UI components.

Current:

```text
CardRepository
↓
SQLiteCardRepository
```

Future possibilities:

```text
CardRepository
↓
Cloud repository
```

or:

```text
CardRepository
↓
Local + sync implementation
```

without requiring the core product UI to be rewritten.

---

# 37. Account and Cloud Readiness

The initial MVP does not require:

```text
Authentication
Cloud Reminder Card storage
Cross-device sync
```

`userId` remains optional.

Do not introduce cloud-specific data concepts into the domain until the product actually needs them.

Cloud readiness should come primarily from:

- stable IDs
- repository boundaries
- predictable timestamps
- clean domain models

rather than building cloud infrastructure prematurely.

---

# 38. Sensitive Data Boundaries

The Reminder Card model should not require:

- email address
- bank credentials
- account numbers
- payment card numbers
- passwords
- government ID numbers
- passport information
- complete medical records
- complete original documents

Users may accidentally include sensitive information in Capture inputs.

The system should minimise what enters persistent Reminder Card data.

See:

```text
docs/PRIVACY_RULES.md
```

for detailed handling rules.

---

# 39. Example — Text Capture

## Input

```text
Need to cancel my gym membership before August 12.
```

## ExtractionResult

```json
{
  "draft": {
    "title": "Cancel gym membership",
    "category": "deadline",
    "temporalKind": "deadline",
    "targetAt": "2026-08-12",
    "timePrecision": "date_only",
    "timezone": "Australia/Melbourne",
    "nextAction": "Cancel gym membership before renewal",
    "location": null,
    "amount": null,
    "contactOrProvider": "Gym",
    "context": null,
    "suggestedReminderAt": "2026-08-10T09:00:00+10:00"
  },
  "confidence": "high",
  "uncertainFields": [],
  "clarificationQuestion": null
}
```

---

## Confirmed ReminderCard

```json
{
  "id": "card_uuid",
  "userId": null,
  "title": "Cancel gym membership",
  "category": "deadline",
  "sourceType": "text",
  "temporalKind": "deadline",
  "targetAt": "2026-08-12",
  "timePrecision": "date_only",
  "timezone": "Australia/Melbourne",
  "nextAction": "Cancel gym membership before renewal",
  "location": null,
  "amount": null,
  "contactOrProvider": "Gym",
  "context": null,
  "reminder": {
    "id": "reminder_uuid",
    "scheduledAt": "2026-08-10T09:00:00+10:00",
    "status": "scheduled",
    "nativeNotificationId": null,
    "createdAt": "2026-07-28T14:30:00+10:00",
    "updatedAt": "2026-07-28T14:30:00+10:00"
  },
  "status": "active",
  "createdAt": "2026-07-28T14:30:00+10:00",
  "updatedAt": "2026-07-28T14:30:00+10:00",
  "completedAt": null,
  "archivedAt": null,
  "deletedAt": null
}
```

---

# 40. Example — Image Capture

A screenshot contains:

```text
City Dental Melbourne

Appointment confirmed

Tuesday 18 August
3:30 PM

120 Collins Street
```

The original screenshot is temporary.

The extracted domain data may become:

```json
{
  "title": "Attend dentist appointment",
  "category": "appointment",
  "sourceType": "image",
  "temporalKind": "event",
  "targetAt": "2026-08-18T15:30:00+10:00",
  "timePrecision": "exact",
  "timezone": "Australia/Melbourne",
  "nextAction": "Attend dentist appointment",
  "location": "120 Collins Street",
  "amount": null,
  "contactOrProvider": "City Dental Melbourne"
}
```

The original screenshot does not need to remain stored after confirmation.

---

# 41. Example — Ambiguous Capture

## Input

```text
Alex said they might know by Thursday or Friday.
Message them again if I haven't heard back.
```

Possible extraction:

```json
{
  "draft": {
    "title": "Follow up with Alex",
    "category": "follow_up",
    "temporalKind": "follow_up",
    "targetAt": null,
    "timePrecision": "unknown",
    "timezone": "Australia/Melbourne",
    "nextAction": "Message Alex if there has been no response",
    "location": null,
    "amount": null,
    "contactOrProvider": "Alex",
    "context": "Alex expects to know by Thursday or Friday",
    "suggestedReminderAt": null
  },
  "confidence": "medium",
  "uncertainFields": [
    {
      "field": "targetAt",
      "reason": "Two possible follow-up dates were provided.",
      "alternatives": [
        "Thursday",
        "Friday"
      ]
    }
  ],
  "clarificationQuestion":
    "Would you like to follow up on Thursday or Friday?"
}
```

The application should resolve important ambiguity during Review rather than silently turning uncertain information into persistent data.

---

# 42. MVP Schema Assumptions

The following are deliberate simplifications for MVP v0.1.

---

## One Capture → One Primary Future Action

The MVP assumes one Capture normally produces one Reminder Card.

If multiple actions are detected, WillDo may:

- identify one primary candidate
- ask which Future Action the user wants to capture

Automatic creation of multiple cards is outside the initial MVP.

---

## One Card → One Primary Reminder

The MVP supports one primary Reminder Schedule per card.

Multiple reminder schedules may be introduced later if user evidence justifies them.

---

## Category Is Lightweight

Categories are not intended to create:

- folders
- projects
- nested structures
- workflow engines
- independent life-admin systems

---

## Raw Input Is Not Product History

Screenshots and raw audio should not become a permanent personal archive by default.

The confirmed Future Action is the primary persistent object.

---

# 43. Schema Change Rule

Before adding a persistent domain field, ask:

1. Does the user need this information when the Future Action returns later?
2. Does it support Capture, understanding, reminding, or resolving?
3. Does it actually need persistence?
4. Is this a product requirement or merely something AI can extract?
5. Does it introduce additional privacy risk?
6. Can the need be solved without changing the domain model?

Do not modify the domain schema simply because:

- a model can extract another field
- a SQLite table would be easier to design differently
- a future feature might hypothetically use the information

The domain should represent the Future Action, not every technical capability of the system.

---

# 44. Persistence Change Rule

Before changing the SQLite persistence schema, ask:

1. Does the domain model actually need to change?
2. Can the persistence representation change without affecting product semantics?
3. Does existing local data require migration?
4. Does the change leak infrastructure details into feature code?
5. Is the change required in the current development phase?

Prefer changing:

```text
SQLite implementation
```

without changing:

```text
product domain
```

when the product meaning itself has not changed.

---

# 45. Data North Star

WillDo should persist:

> The minimum reliable information required for a Future Action to make sense when it returns later.

The goal is not to build the richest possible data model.

The goal is to represent enough information to support:

```text
Capture
→ Understand
→ Review
→ Remind
→ Resolve
```

without accumulating unnecessary complexity or personal data.