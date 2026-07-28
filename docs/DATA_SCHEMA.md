# WillDo — Data Schema

> Status: Draft v0.1  
> Purpose: This document defines the core data models used by WillDo.  
> It acts as the shared contract between capture, AI extraction, review, local persistence, reminders, and future cloud storage.
>
> The schema should remain intentionally small during the MVP.  
> New fields should only be added when they support a validated product need.

---

## 1. Data Model Principles

WillDo separates the user journey into three data stages:

```text
Capture Input
→ AI Extraction Result
→ Confirmed Reminder Card
```

These stages should not be treated as the same object.

### Capture Input

Temporary raw information supplied by the user.

Examples:

- typed text
- pasted text
- screenshot
- photo
- voice transcript

Raw image and voice data should not be retained by default after the capture workflow is completed.

### Extraction Result

A temporary AI interpretation of the capture.

It may contain:

- extracted fields
- suggestions
- uncertainty
- missing information
- clarification prompts

The user can edit this information before confirmation.

### Reminder Card

The confirmed, persistent representation of a Future Action.

Only confirmed Reminder Cards should become part of the user's Inbox and reminder system.

---

## 2. Core Types

### 2.1 Capture Source Type

Describes how the user captured the information.

```ts
export type CaptureSourceType =
  | "text"
  | "image"
  | "voice";
```

This describes the capture method only.

It should not affect how the resulting Reminder Card behaves.

---

### 2.2 Reminder Category

Categories are lightweight labels used primarily for presentation and filtering.

They should not become a complex organisational system.

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

#### Category intent

| Category | Example |
| --- | --- |
| `appointment` | Dentist appointment |
| `deadline` | Event registration closes Friday |
| `bill` | Electricity bill due August 12 |
| `return` | Headphones return window ends August 20 |
| `warranty` | Laptop warranty expires next year |
| `follow_up` | Contact property manager again next Wednesday |
| `general` | Call insurance tomorrow |

Categories should normally be suggested by AI rather than manually chosen during capture.

The category must remain editable.

---

### 2.3 Temporal Kind

Different Future Actions use dates differently.

An appointment is not the same as a deadline, even if both contain a date.

```ts
export type TemporalKind =
  | "event"
  | "deadline"
  | "follow_up"
  | "reference"
  | "none";
```

Examples:

```text
Dentist at 3 PM
→ event

Return before August 12
→ deadline

Contact landlord again next Wednesday
→ follow_up

Warranty began August 2026
→ reference
```

`reference` represents a date that provides useful context but is not itself necessarily the action time.

---

### 2.4 Time Precision

AI may know a date without knowing an exact time.

WillDo must not invent precision that does not exist in the source.

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

Sometime next month
→ approximate
```

---

### 2.5 Reminder Card Status

```ts
export type ReminderCardStatus =
  | "active"
  | "done"
  | "archived";
```

#### Active

The Future Action is still relevant.

#### Done

The user has completed or resolved the action.

#### Archived

The card is retained but no longer part of the active Inbox.

Draft state should normally exist during the review flow rather than as a persisted Reminder Card.

---

## 3. Capture Input

Capture Input represents temporary user-provided information before AI processing.

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

### 3.1 Capture Input Rules

At least one content field must exist according to `sourceType`.

#### Text

```ts
{
  sourceType: "text",
  text: "Cancel gym membership before August 12"
}
```

#### Image

```ts
{
  sourceType: "image",
  imageUri: "temporary-device-uri"
}
```

#### Voice

```ts
{
  sourceType: "voice",
  voiceTranscript:
    "Contact the property manager next Wednesday if they have not replied"
}
```

Raw capture input is temporary.

The application should not depend on retaining:

- original screenshots
- photos
- audio recordings

after the user confirms or abandons the capture.

---

## 4. Reminder Card Draft

AI extraction should produce a draft representation before confirmation.

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

The draft does not contain persistent identifiers or lifecycle metadata.

Those are created only after confirmation.

---

## 5. Money Amount

Financial information is optional and should only be stored when relevant to the Future Action.

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

WillDo should not attempt to become an expense tracker or financial-management system.

Amount exists only when it helps the user understand the Future Action.

---

## 6. Extraction Result

The AI response should include both the proposed card and information about uncertainty.

```ts
export type ExtractionResult = {
  draft: ReminderCardDraft;

  confidence: AIConfidence;

  uncertainFields: ExtractionUncertainty[];

  clarificationQuestion?: string | null;
};
```

### 6.1 AI Confidence

```ts
export type AIConfidence =
  | "high"
  | "medium"
  | "low";
```

Overall confidence is primarily useful during:

- development
- testing
- review UX
- AI evaluation

It should not necessarily be shown prominently to users.

---

### 6.2 Field-Level Uncertainty

```ts
export type ExtractionUncertainty = {
  field: ReminderDraftField;

  reason: string;

  alternatives?: string[];
};
```

Possible fields:

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

Example:

```ts
{
  field: "targetAt",
  reason: "Two dates were detected in the screenshot.",
  alternatives: [
    "2026-08-12",
    "2026-08-15"
  ]
}
```

WillDo should prefer uncertainty over guessing.

---

## 7. Confirmed Reminder Card

A Reminder Card is created after the user reviews and confirms the extracted information.

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

## 8. Core Reminder Card Fields

### `id`

Unique identifier for the card.

Use a UUID-compatible identifier so the model can migrate to cloud storage later without changing ID semantics.

---

### `userId`

```ts
userId?: string | null;
```

The local-first MVP does not require an account.

For the MVP:

```ts
userId = null
```

This field exists so future cloud-backed cards can be associated with authenticated users.

The application must not assume that `userId` exists.

---

### `title`

Short human-readable description of the Future Action.

Good:

```text
Cancel gym membership
```

Bad:

```text
Gym membership information screenshot from July
```

The title should describe what requires attention.

---

### `category`

A lightweight classification.

Category should support:

- visual recognition
- simple filtering
- AI understanding

It should not determine complex application behaviour.

---

### `sourceType`

Records how the Future Action originally entered WillDo.

Examples:

```text
text
image
voice
```

This may later help evaluate which capture methods provide the most value.

---

### `targetAt`

The primary date or date-time associated with the Future Action.

Examples:

```text
Dentist appointment:
2026-08-18T15:30:00+10:00

Return deadline:
2026-08-20

Follow up:
2026-08-14
```

The meaning of `targetAt` is determined by `temporalKind`.

---

### `timePrecision`

Indicates how confidently the source defines time.

This allows WillDo to distinguish:

```text
August 12
```

from:

```text
August 12 at 3:30 PM
```

without inventing an arbitrary time.

---

### `timezone`

Where an exact time exists, timezone should be preserved.

Example:

```text
Australia/Melbourne
```

The user's current device timezone may be used when the source does not specify a timezone, but the application should clearly separate:

- extracted timezone
- device-assumed timezone

if this becomes important later.

For the MVP, the current device timezone may be used as the default.

---

### `nextAction`

A concise description of what the user should do.

Example:

```text
Cancel the membership before renewal.
```

or:

```text
Attend dentist appointment.
```

The title and next action may be similar.

This is acceptable.

The purpose of `nextAction` is to preserve enough context for the reminder to be actionable later.

---

### `context`

Optional short supporting context.

Example:

```text
Property manager said the plumber would contact me this week.
```

This should remain concise.

WillDo is not a notes app.

---

## 9. Reminder Schedule

Reminder timing should be separated conceptually from the date of the Future Action.

Example:

```text
Return deadline:
August 20

Reminder:
August 18 at 9:00 AM
```

These are not the same thing.

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

### 9.1 Reminder Rules

For the MVP, each Reminder Card supports:

**one primary reminder**

This keeps the interaction and notification model simple.

Future versions may support multiple reminders.

Example future scenario:

```text
7 days before
1 day before
2 hours before
```

This is intentionally outside the initial schema requirement.

---

### 9.2 `nativeNotificationId`

The operating system may return an identifier when a notification is scheduled.

This identifier exists only to support native notification management.

It must not become the identity of the reminder itself.

Example:

```text
ReminderSchedule.id
→ WillDo business identifier

nativeNotificationId
→ device notification identifier
```

This separation is important for future cloud sync.

---

## 10. Dates and Time Format

All persistent timestamps should use ISO 8601 strings.

Example:

```text
2026-08-18T15:30:00+10:00
```

System timestamps should include:

```text
createdAt
updatedAt
completedAt
archivedAt
deletedAt
```

Whenever the product only knows a calendar date rather than a time, the schema must preserve that distinction.

Do not automatically convert:

```text
August 12
```

into:

```text
August 12 at 9:00 AM
```

AI extraction and reminder scheduling are separate decisions.

---

## 11. Deletion Model

The schema includes:

```ts
deletedAt?: string | null;
```

This allows the storage layer to support soft deletion later.

For the local MVP, implementation may permanently remove deleted cards if that is simpler.

UI code should not depend on whether deletion is implemented as:

- hard delete
- soft delete

This behaviour belongs to the repository layer.

---

## 12. Persisted vs Temporary Data

### Persisted

The MVP may persist:

- confirmed Reminder Cards
- Reminder Schedules
- card status
- timestamps

### Temporary

The MVP should treat the following as temporary by default:

- uploaded images
- captured photos
- raw audio
- intermediate AI prompts
- AI raw responses
- abandoned draft captures

Temporary data should be discarded when it is no longer required.

---

## 13. Data That Should Not Be Required

The MVP must not require:

- email address
- bank account information
- financial account credentials
- government ID numbers
- passport information
- medical records
- contact-list access
- complete original documents

Users may still accidentally include sensitive information in captures.

Privacy handling is defined separately in `PRIVACY_RULES.md`.

---

## 14. Example — Text Capture

### Input

```text
Need to cancel my gym membership before August 12.
```

### Extraction Result

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

### Confirmed Card

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
    "createdAt": "2026-07-27T09:00:00+10:00",
    "updatedAt": "2026-07-27T09:00:00+10:00"
  },
  "status": "active",
  "createdAt": "2026-07-27T09:00:00+10:00",
  "updatedAt": "2026-07-27T09:00:00+10:00",
  "completedAt": null,
  "archivedAt": null,
  "deletedAt": null
}
```

---

## 15. Example — Image Capture

A user shares an appointment screenshot containing:

```text
Appointment confirmed
Tuesday 18 August
3:30 PM
120 Collins Street
```

The original screenshot is temporary.

The confirmed card may contain:

```json
{
  "title": "Appointment",
  "category": "appointment",
  "sourceType": "image",
  "temporalKind": "event",
  "targetAt": "2026-08-18T15:30:00+10:00",
  "timePrecision": "exact",
  "timezone": "Australia/Melbourne",
  "nextAction": "Attend appointment",
  "location": "120 Collins Street",
  "amount": null,
  "contactOrProvider": null
}
```

The original image does not need to remain stored after confirmation.

---

## 16. Example — Ambiguous Capture

### Input

```text
Alex said they might know by Thursday or Friday.
Message them again if I haven't heard back.
```

WillDo should not arbitrarily choose Thursday.

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
      "reason": "The input contains two possible follow-up dates.",
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

This is preferable to silently guessing.

---

## 17. MVP Schema Assumptions

The following are deliberate simplifications for MVP v0.1.

### One Capture → One Primary Future Action

The MVP assumes that one capture normally creates one Reminder Card.

If AI detects multiple actions, the MVP may:

- ask the user which action matters, or
- select a primary candidate for review

Automatic creation of multiple Reminder Cards from a single capture is outside the initial MVP.

This assumption should be tested.

---

### One Card → One Primary Reminder

The MVP supports one primary scheduled reminder per card.

Multiple reminder schedules may be introduced later if testing demonstrates a clear need.

---

### Category Is Lightweight

Categories are not intended to form:

- folders
- projects
- nested structures
- workflows

---

### Raw Input Is Not the Product Record

The Reminder Card is the persistent product object.

Screenshots, audio, and raw capture information should not become a personal document archive.

---

## 18. Repository Contract

UI components must not directly read from or write to device storage.

All persistent Reminder Card operations should go through a repository interface.

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

The initial implementation may use local device storage.

A future implementation may use:

```text
Supabase
or another cloud persistence layer
```

without changing the core UI contract.

---

## 19. Schema Change Rule

Before adding a new persistent field, ask:

1. Does the user need this information when the Future Action returns later?
2. Does it support capture, understanding, reminding, or resolving?
3. Does it need to be persisted, or can it remain temporary?
4. Is it a real product requirement or merely something AI is capable of extracting?

Do not persist information simply because AI can detect it.

The schema should represent the user's Future Action, not the full contents of the original information.