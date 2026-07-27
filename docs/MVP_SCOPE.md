# WillDo — MVP Scope

> Status: Draft v0.1
>
> Purpose: This document defines what WillDo will and will not include in the initial MVP.
> It should be used to prevent scope creep during product design and development.
>
> Features outside this document should not be implemented unless the scope is explicitly updated.

---

## 1. MVP Goal

The goal of the WillDo MVP is to validate whether Future Action Capture can provide a faster and lower-effort alternative to manually creating reminders from everyday information.

The MVP should allow a user to:

1. encounter something they need to deal with later
2. capture it quickly
3. let AI identify the relevant future action
4. review and correct the interpretation
5. confirm the reminder
6. receive a reminder at the appropriate time
7. return to WillDo and understand what needs to be done

The MVP is not intended to replace a complete task manager, calendar, or life-admin system.

---

## 2. Core MVP Loop

The complete MVP loop is:

Capture  
→ Extract  
→ Review  
→ Confirm  
→ Remind  
→ Resolve

Each major feature should support this loop.

If a proposed feature mainly improves general task management rather than this loop, it should not automatically be included in the MVP.

---

# 3. P0 — Required for MVP

P0 features define the minimum product that should be considered a functional WillDo MVP.

---

## 3.1 Capture

Users must be able to quickly create a new capture from the main product experience.

The MVP should ultimately support:

### Text Capture

Users can:

- type free-form text
- paste text copied from another app
- submit incomplete or unstructured information

Example:

> Need to cancel my gym membership before August 12.

### Image Capture

Users can:

- select an existing screenshot or photo
- take a photo where appropriate
- submit the image for AI interpretation

Example inputs:

- appointment screenshots
- event posters
- receipts
- bills
- chat screenshots
- return information

### Voice Capture

Users can:

- speak a quick thought or instruction
- convert speech into text
- send the resulting content through the same extraction workflow

Example:

> The property manager said to contact them again next Wednesday if I haven't heard anything.

The three capture methods should feed into the same underlying Future Action Capture workflow.

---

## 3.2 AI Extraction

WillDo should convert unstructured input into a proposed future action.

AI may extract:

- title
- relevant date
- relevant time
- category
- location
- amount
- person or organisation
- next action
- suggested reminder time
- follow-up date

Not every field is required.

AI must not invent unsupported information.

When information is uncertain or missing, the interface should allow the user to resolve it.

---

## 3.3 Review Before Confirm

AI output must never immediately become a confirmed reminder.

After extraction, users should see a lightweight review experience.

Primary information should include:

- title
- date
- time
- reminder timing
- next action

Additional information may include:

- category
- location
- amount
- contact or organisation
- follow-up date

Users must be able to:

- edit extracted information
- remove unnecessary information
- correct AI mistakes
- confirm the future action
- cancel without saving

The review step should be fast enough that AI still reduces work compared with manually creating a reminder.

---

## 3.4 Reminder Cards

Confirmed captures become Reminder Cards.

A Reminder Card should communicate:

- what needs attention
- when it matters
- what the user needs to do
- current status

Cards should not become complex project-management objects.

The card exists to preserve enough context for the user to understand the future action when they return later.

---

## 3.5 Inbox

Users should have a clear place to see captured future actions.

The Inbox should support:

- upcoming items
- items requiring attention
- completed items
- empty state

Users should be able to quickly understand:

- what needs action
- what is coming soon
- what has already been handled

The Inbox should remain lightweight and should not become a Kanban board or project dashboard.

---

## 3.6 Card Detail and Basic Management

Users must be able to:

- view a Reminder Card
- edit its information
- change its reminder time
- mark it as done
- delete it

Basic management exists to support the capture-remind lifecycle.

Advanced task-management features are outside MVP scope.

---

## 3.7 Local Notifications

The native MVP should support local notifications for confirmed reminders.

Users should be able to:

- choose or confirm when they want to be reminded
- receive a native notification
- open the relevant Reminder Card from the notification where practical

The MVP does not require a cloud notification service.

Notifications should be generated locally on the device where possible.

---

## 3.8 Local-First Persistence

The MVP should work without an account.

Confirmed cards should be stored locally on the user's device.

The implementation must remain cloud-ready.

The UI must not depend directly on a specific local storage implementation.

Persistence should be accessed through a repository or service abstraction so that cloud storage can replace or extend local storage later.

---

# 4. P1 — Important After the Core Loop Works

P1 features improve the product but should not block validation of the core interaction.

---

## 4.1 Calendar Integration

Allow users to send suitable confirmed actions to their device calendar.

Not every Reminder Card should automatically become a calendar event.

Calendar integration should be offered when the information represents a time-specific event such as:

- appointment
- event
- inspection
- booking

---

## 4.2 Follow-Up Suggestions

AI may suggest follow-up reminders when the captured information implies that action depends on another person or event.

Example:

> Property manager said they would respond this week.

Possible suggestion:

> Remind me Friday if there is still no response.

The user must confirm the suggestion.

---

## 4.3 Search and Lightweight Filtering

Users may search saved Reminder Cards or filter by useful states such as:

- upcoming
- completed
- category

The MVP does not need advanced task organisation.

---

## 4.4 Improved AI Uncertainty Handling

The interface may communicate situations where AI is uncertain.

Examples:

- multiple possible dates detected
- unclear deadline
- ambiguous next action

Instead of guessing, WillDo should ask the user for minimal clarification.

---

## 4.5 Faster Capture Entry Points

Explore native capabilities that reduce the number of steps required to start a capture.

Possible future directions include:

- home-screen quick actions
- system share workflows
- widgets

These should only be implemented after the core capture experience is validated.

---

# 5. P2 — Post-MVP

These capabilities are intentionally outside the initial MVP.

They may be explored only after the core Future Action Capture concept has been validated.

---

## Account and Cloud Features

- user authentication
- cloud database
- multi-device sync
- cloud backup
- account recovery

---

## Advanced System Integration

- advanced Share Sheet integration
- widgets
- notification actions
- background processing
- cross-device reminder sync

---

## External Account Integrations

- email integration
- bank integration
- automatic inbox scanning
- automatic financial transaction scanning

---

## Collaboration

- family sharing
- shared reminders
- household management
- teams
- assignment of tasks to other people

---

## Advanced Task Management

- projects
- Kanban boards
- priorities
- productivity scoring
- habit tracking
- goal tracking
- complex recurring task systems

---

# 6. Explicit Non-Goals

WillDo should not become:

- a general-purpose to-do app
- a project management app
- a productivity dashboard
- a calendar replacement
- a notes replacement
- a document storage vault
- a family organiser
- a financial-management platform
- an email assistant
- a general AI assistant

The MVP should resist adding features simply because established task-management apps include them.

WillDo's differentiated value is Future Action Capture.

---

# 7. MVP Data Principles

The MVP should store only information necessary to support Future Action Capture.

Original images or voice recordings should not be retained by default after extraction unless a future product decision explicitly requires them.

Structured Reminder Card data should be separated from temporary capture inputs.

Users must be able to delete saved Reminder Cards.

More detailed requirements are defined in `PRIVACY_RULES.md`.

---

# 8. MVP Technical Constraints

The MVP will be developed as a native mobile application using:

- React Native
- Expo
- TypeScript

The MVP is:

- local-first
- account-free
- cloud-ready

Implementation details belong in technical documentation, but the following architecture rules apply to MVP development:

- UI components must not directly depend on storage implementation.
- Persistence should use a repository or service abstraction.
- AI extraction should expose a stable structured result contract.
- Mock extraction and real extraction should return compatible data shapes.
- Device integrations should be isolated from core product logic where practical.

---

# 9. Development Phases

The MVP should be built incrementally.

---

## Phase 1 — Core Interaction Prototype

Build:

Text Capture  
→ Mock Extraction  
→ Review  
→ Confirm  
→ Local Card  
→ Inbox

Purpose:

Validate the interaction and data model before introducing external AI or native complexity.

---

## Phase 2 — Real Text AI

Replace mock extraction with real AI extraction.

Validate:

- extraction accuracy
- response structure
- uncertainty
- review effort

---

## Phase 3 — Image Capture

Add:

- screenshot/photo selection
- image interpretation
- the same review workflow

Validate whether image input meaningfully improves Future Action Capture.

---

## Phase 4 — Voice Capture

Add voice as another fast capture method.

Voice should feed into the same extraction and review system.

---

## Phase 5 — Native Reminder Loop

Add:

- local notification permissions
- reminder scheduling
- reminder delivery
- returning to the associated card

This completes the core MVP loop:

Capture → Remind → Resolve

---

## Phase 6 — Integration and Polish

Consider:

- calendar integration
- follow-up suggestions
- search/filter
- onboarding
- empty states
- error handling
- interaction polish
- accessibility
- performance

Only features that improve the validated experience should be included.

---

# 10. MVP Success Criteria

The MVP should allow a test user to complete the following without assistance:

### Capture

The user can capture an unstructured future action.

### Understand

The user can understand what AI extracted.

### Correct

The user can easily correct incorrect or missing information.

### Confirm

The user can turn the capture into a confirmed reminder.

### Retrieve

The user can later locate the Reminder Card.

### Remind

The user receives an appropriate reminder.

### Resolve

The user can understand the required action and mark it as complete.

---

# 11. Product Validation Questions

The MVP should help answer:

1. Do users recognise Future Action Capture moments in their own lives?
2. Is WillDo faster or easier than their existing workaround?
3. Does AI reduce the effort required to create a useful reminder?
4. Is the review step lightweight enough?
5. Which input method provides the most value: text, image, or voice?
6. Do users trust AI-extracted reminder information?
7. Are WillDo reminders noticeable enough to support follow-through?
8. Do users return to the Inbox after capturing something?
9. Does the product feel meaningfully different from a traditional reminder app?
10. Which management features, if any, are actually necessary after capture?

---

# 12. Scope Rule

When considering a new feature, ask:

> Does this materially reduce the effort of capturing, understanding, remembering, or resolving a future action?

If the answer is no, the feature should not be part of the MVP without a deliberate scope change.