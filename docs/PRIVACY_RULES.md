# WillDo — Privacy Rules

> Status: Draft v0.1  
> Purpose: This document defines the privacy principles and product rules for WillDo MVP development.
>
> These rules are product and engineering constraints, not a complete legal or regulatory compliance specification.
>
> Any future public commercial release should receive a separate privacy, security, and legal review.

---

## 1. Privacy Position

WillDo handles everyday information that may contain personal or sensitive details.

Users may capture:

- screenshots
- messages
- appointments
- receipts
- bills
- return information
- warranty information
- household matters
- voice notes
- personal reminders

Even when the intended Future Action is simple, the original capture may contain more information than WillDo needs.

For example, a bill screenshot may contain:

```text
Name
Home address
Account number
Bill amount
Due date
Payment details
```

WillDo may only need:

```text
Bill title
Provider
Amount
Due date
Next action
```

The product should therefore follow the principle:

> Process the minimum information required to create a useful Future Action.

---

# 2. Core Privacy Principles

WillDo follows six MVP privacy principles.

## 2.1 User-Initiated Capture

WillDo should only process information the user intentionally chooses to capture.

The MVP must not:

- automatically scan the user's device
- automatically scan screenshots
- automatically scan photos
- monitor clipboard content in the background
- connect to email accounts
- connect to bank accounts
- continuously listen through the microphone
- automatically inspect other applications

The user decides what enters WillDo.

---

## 2.2 Data Minimisation

WillDo should extract and retain only information necessary to support:

```text
Capture
→ Understand
→ Review
→ Remind
→ Resolve
```

Information should not be retained merely because AI can detect it.

Examples of unnecessary information may include:

- account numbers
- card numbers
- full residential addresses unrelated to the action
- unrelated names
- signatures
- government identifiers
- complete document text
- unrelated conversation history

---

## 2.3 Temporary Raw Inputs

Images, photos, audio, and other raw capture material should be treated as temporary by default.

The long-term product record is:

```text
ReminderCard
```

not:

```text
Original screenshot
Original document
Original audio recording
```

Raw inputs should be discarded when they are no longer required for the capture workflow.

---

## 2.4 User Review Before Persistence

AI interpretation must not automatically become a saved Future Action.

The user should review the extracted result before confirmation.

This provides control over:

- what information is saved
- whether AI interpreted the capture correctly
- whether unnecessary information should be removed
- whether the reminder should exist at all

---

## 2.5 User Control

Users must be able to:

- inspect saved Reminder Cards
- edit saved information
- mark actions as complete
- delete saved information
- cancel a capture before saving

Future cloud versions should also support:

- account deletion
- data export
- cloud-data deletion

---

## 2.6 Privacy Should Not Depend on User Expertise

Users should not need to understand AI infrastructure or data storage architecture to protect themselves.

Privacy-protective behaviour should be the default.

Do not rely on users to manually configure privacy settings before the product becomes reasonably safe to use.

---

# 3. MVP Data Boundaries

The MVP is designed as:

```text
Local-first
Account-free
User-initiated
Minimal persistence
```

The MVP should not require:

- account creation
- email address
- phone number
- contact access
- bank access
- email access
- location history
- background device access

---

# 4. Data Classification

For MVP development, WillDo should think about captured information using three broad sensitivity levels.

## 4.1 Low Sensitivity

Examples:

- general reminders
- event information
- shopping reminders
- public event posters
- simple personal notes

Example:

```text
Book restaurant before Friday.
```

These are appropriate WillDo use cases.

---

## 4.2 Moderate Sensitivity

Examples:

- receipts
- bills
- appointment confirmations
- landlord conversations
- household information
- purchase information
- subscription information

Example:

```text
Electricity bill due August 12.
```

These are supported use cases, but extraction should minimise unnecessary personal information.

---

## 4.3 High Sensitivity

Examples may include:

- passports
- driver's licences
- government IDs
- bank statements
- payment card details
- medical reports
- prescriptions
- detailed health information
- tax documents
- legal case documents
- passwords
- authentication codes

These are not intended MVP use cases.

WillDo should not be positioned as a storage or processing tool for highly sensitive documents.

---

# 5. Sensitive Capture Warning

The capture experience should communicate that users should avoid submitting highly sensitive information.

The warning should be clear without making the product feel dangerous.

Example:

```text
Avoid uploading IDs, bank details, passwords, or sensitive medical documents.
```

The message should appear where relevant, particularly around image capture.

It does not need to interrupt every capture.

---

# 6. Image Privacy

Images are particularly risky because they may contain information unrelated to the user's intended Future Action.

Examples:

- names
- addresses
- phone numbers
- profile photos
- account numbers
- unrelated messages
- notification previews
- other visible apps or browser content

---

## 6.1 Image Lifecycle

The intended MVP flow is:

```text
User selects image
↓
Image is temporarily available for extraction
↓
AI processes relevant information
↓
User reviews structured result
↓
User confirms
↓
ReminderCard is saved
↓
Original image is discarded
```

The application should not depend on retaining the original image.

---

## 6.2 No Default Image Archive

WillDo should not evolve into a screenshot or document archive by accident.

Confirmed Reminder Cards should not include a persistent original-image field by default.

If future research demonstrates that retaining source images provides meaningful value, this should be treated as a deliberate product and privacy decision.

---

## 6.3 Image Cropping or Redaction

A future version may allow users to:

- crop an image before processing
- select the relevant region
- redact sensitive information

These capabilities are not required for initial MVP validation but may reduce unnecessary data exposure.

---

# 7. Voice Privacy

Voice is a capture method, not a recording-storage feature.

The intended flow is:

```text
User starts voice capture
↓
Audio is captured intentionally
↓
Speech is converted to text
↓
Future Action extraction occurs
↓
Structured result is reviewed
↓
Raw audio is discarded
```

WillDo should not retain voice recordings by default.

The persisted Future Action may contain the resulting structured text fields.

---

## 7.1 Microphone Access

Microphone access must only occur after explicit user action.

The app must not:

- continuously listen
- start recording in the background without clear user intent
- use microphone access for unrelated functionality

The user should receive normal operating-system permission controls.

---

# 8. Text Privacy

Typed and pasted text may also contain sensitive information.

The app should not assume text input is automatically safe.

The same principles apply:

- minimise extraction
- avoid unnecessary persistence
- treat captured content as temporary until confirmation
- persist only Future Action information that the user confirms

---

# 9. AI Processing

Real AI extraction may require sending capture information to an external AI service.

This represents a different privacy condition from local device storage and must be treated explicitly.

The product must not create the impression that all processing happens locally if capture data is transmitted to an external service.

---

## 9.1 AI Data Flow

A future production flow may resemble:

```text
User device
↓
WillDo extraction service
↓
AI provider
↓
Structured ExtractionResult
↓
User device
↓
Review
```

The final implementation may differ, but users should not be misled about where processing occurs.

---

## 9.2 Minimum AI Payload

Only information required for extraction should be sent to the AI service.

Do not send unrelated application data such as:

- complete Reminder Card history
- unrelated captures
- device information unrelated to extraction
- analytics identifiers
- other user records

unless there is a validated product requirement.

---

## 9.3 AI Provider Policy

Before production release, the chosen AI provider should be reviewed for:

- data retention
- training-data usage
- regional processing
- security
- deletion policies
- applicable contractual terms

These considerations are implementation-dependent and should not be assumed in MVP documentation.

---

## 9.4 API Credentials

AI provider credentials must never be embedded directly in distributed client code.

A native mobile application cannot safely treat a bundled API key as secret.

Production AI requests should therefore use a protected server-side service or equivalent secure architecture.

Example:

```text
WillDo App
↓
WillDo Backend / Edge Function
↓
AI Provider
```

The client should not contain a reusable private provider key.

---

# 10. AI Extraction Privacy

The extraction system should identify the Future Action, not reconstruct the entire document.

Example input:

```text
Electricity Provider
John Example
123 Example Street
Account: 123456789
Amount due: $86.40
Due August 12
```

Useful structured output:

```json
{
  "title": "Pay electricity bill",
  "amount": {
    "value": 86.40,
    "currency": "AUD"
  },
  "targetAt": "2026-08-12",
  "contactOrProvider": "Electricity Provider",
  "nextAction": "Pay electricity bill"
}
```

Information that should normally not be persisted:

```text
John Example
123 Example Street
123456789
```

---

# 11. Extracted Information vs Original Information

WillDo should distinguish between:

### Raw Input

What the user submitted.

### Extracted Fact

Information supported by the capture and relevant to the Future Action.

### AI Suggestion

Information proposed by WillDo that was not explicitly present in the capture.

Example:

```text
Original:
Return deadline August 20

Extracted fact:
Deadline = August 20

AI suggestion:
Remind August 18
```

This distinction is important for both trust and privacy.

AI-generated suggestions must not be presented as original source information.

---

# 12. Local Storage

The first MVP stores confirmed Reminder Cards locally.

Local storage is intended to reduce:

- account requirements
- infrastructure complexity
- unnecessary cloud persistence
- early privacy risk

However, local storage should not be described as inherently secure.

Device security, backup behaviour, operating-system storage, and physical access may still affect privacy.

---

## 12.1 Storage Abstraction

UI components must not directly depend on the local persistence implementation.

All Reminder Card storage should go through a repository or service abstraction.

This allows future migration to:

```text
Cloud storage
Encrypted storage
Database-backed accounts
Cross-device sync
```

without redesigning the product model.

---

## 12.2 Sensitive Secret Storage

If WillDo later needs to store:

- authentication tokens
- session secrets
- credentials

they must not be stored as ordinary Reminder Card data.

Secure device storage mechanisms should be used.

---

# 13. Logging

Application logs must not include raw user capture content by default.

Do not log:

- screenshot contents
- full voice transcripts
- complete Reminder Cards
- raw AI prompts containing user content
- raw AI responses containing personal information

Example of bad logging:

```ts
console.log("AI request:", captureInput);
```

Example of safer development logging:

```ts
console.log("Extraction request started", {
  sourceType: input.sourceType,
  requestId
});
```

Development debugging that temporarily exposes user content should not remain in production code.

---

# 14. Analytics

Analytics should measure product behaviour without collecting the contents of Future Actions.

Useful analytics may include:

```text
capture_started
capture_source_selected
extraction_completed
review_edited
reminder_confirmed
reminder_completed
```

Analytics should not include values such as:

```text
title
nextAction
context
location
amount
voiceTranscript
originalText
```

unless a separate deliberate privacy decision is made.

---

## 14.1 Example

Acceptable:

```json
{
  "event": "capture_completed",
  "sourceType": "image"
}
```

Avoid:

```json
{
  "event": "capture_completed",
  "text": "My medical appointment is..."
}
```

---

# 15. Crash Reporting

Crash-reporting tools may automatically collect:

- application state
- breadcrumbs
- network metadata
- error context

Before enabling production crash reporting, verify that user capture data is not unintentionally included.

Error messages should not contain raw captured content when avoidable.

---

# 16. Notifications and Lock-Screen Privacy

Notifications can expose information even when the app itself is closed.

This is especially important because WillDo's core value depends on reminders.

A Reminder Card may contain information that users do not want visible on a lock screen.

Example:

```text
Pay overdue medical bill — $480
```

may reveal more than necessary.

---

## 16.1 Notification Principle

A notification should contain enough information to help the user recognise the Future Action, but no more than necessary.

---

## 16.2 Possible Notification Levels

Future testing may explore:

### Detailed

```text
Return headphones before tomorrow.
```

### Reduced Detail

```text
You have something due tomorrow.
```

### Private

```text
WillDo reminder
Tap to view
```

The MVP may initially use a reasonable default, but notification privacy should be considered before real-world testing.

---

## 16.3 Notification Settings

A future version may allow users to choose notification preview behaviour.

Example:

```text
Show full reminder
Show title only
Hide reminder details
```

This is not required for the earliest implementation, but the notification architecture should avoid assuming that full card content must always appear.

---

# 17. Calendar Integration

Calendar integration may expose Reminder Card information to another system.

The app should not automatically sync every capture to a calendar.

Calendar integration should require user action.

Before calendar creation, users should be able to understand:

- title
- date
- time
- location
- other exported information

Only necessary fields should be transferred.

---

# 18. Device Permissions

WillDo should request permissions only when a related feature is used.

Examples:

### Camera

Request when the user chooses to capture a photo.

### Photo Library

Request when the user chooses an existing image.

### Microphone

Request when the user chooses voice capture.

### Notifications

Request when reminder functionality becomes relevant.

### Calendar

Request when the user explicitly chooses calendar integration.

Avoid asking for multiple permissions during initial onboarding.

Permission requests should be contextual.

---

# 19. Permission Denial

The product should continue working where possible when users decline optional permissions.

Examples:

If microphone access is denied:

```text
Text capture
Image capture
```

should still work.

If calendar access is denied:

```text
WillDo reminders
```

should still work.

If notification access is denied, the product should explain the limitation and allow users to change the permission later.

---

# 20. Deletion

Users must be able to delete Reminder Cards.

For the local MVP, deletion may permanently remove local card data.

If future versions use soft deletion or cloud backup, the user-facing meaning of deletion must remain clear.

---

## 20.1 Raw Capture Deletion

Temporary capture content should be discarded when:

- the user cancels the capture
- the capture workflow finishes
- the temporary data is no longer required for extraction

The application should not accumulate abandoned raw captures indefinitely.

---

# 21. Completed Cards

Marking a card as `done` does not necessarily mean the information should remain forever.

Future research should determine whether completed items should:

- remain indefinitely
- automatically archive
- automatically expire
- allow bulk deletion

The MVP may retain completed cards locally for basic history.

This is a working assumption rather than a permanent retention policy.

---

# 22. Cloud Migration

The local-first architecture is designed to support a future authenticated cloud version.

Adding cloud persistence creates additional privacy requirements.

Before cloud migration, define:

- account identity
- encryption approach
- access control
- user data deletion
- backup behaviour
- retention policy
- cross-device sync behaviour
- server logging
- breach-response procedures
- third-party processor responsibilities

Cloud storage must not simply replace local storage without reviewing these implications.

---

# 23. Share Sheet and External Capture

A future WillDo version may accept content through the operating system Share Sheet.

This supports the capture-first product direction but expands the range of information users may submit.

Shared content should be treated exactly like in-app capture:

```text
Receive
→ Process temporarily
→ Extract minimum useful information
→ Review
→ Confirm
→ Discard unnecessary raw input
```

Receiving shared content should not imply permanent storage.

---

# 24. Clipboard

WillDo should not continuously inspect or store clipboard contents.

If future capture allows:

```text
Paste from clipboard
```

it should happen after explicit user action.

Clipboard contents must not be collected for analytics or background processing.

---

# 25. High-Sensitivity Content

WillDo cannot guarantee that users will never submit sensitive information.

If obviously high-sensitivity content is detected, future versions may:

- warn the user
- recommend removing unnecessary details
- minimise extraction
- avoid retaining raw source content

The MVP does not need a complex document-classification safety system.

The product should instead rely primarily on:

- user-initiated capture
- clear scope
- data minimisation
- temporary raw input
- review before saving

---

# 26. Privacy and Product Design

Privacy measures should not create unnecessary capture friction.

For example, WillDo should not require users to complete a long privacy workflow every time they submit an ordinary reminder.

The goal is:

```text
Low friction
+
Reasonable user control
+
Minimal data processing
```

Privacy design should be proportional to risk.

---

# 27. Privacy UX Principles

Privacy communication should be:

- clear
- specific
- contextual
- brief
- actionable

Avoid vague messages such as:

```text
Your privacy is important to us.
```

Prefer:

```text
This image is used to create your reminder and is not saved after confirmation.
```

provided this accurately reflects the implementation.

Never promise privacy behaviour that the implementation does not actually provide.

---

# 28. MVP Privacy UX Examples

## Image Capture

Possible supporting text:

```text
We'll use this image to find the important reminder details.
The original image won't be kept after you confirm.
```

This wording may only be used once implementation matches the claim.

---

## Voice Capture

Possible supporting text:

```text
Your voice is converted into text for this capture.
The recording isn't kept after processing.
```

Again, this must match actual implementation.

---

## AI Review

Possible supporting text:

```text
Only the details you confirm will be saved.
```

---

# 29. Development Data

During development and testing, developers should prefer:

- synthetic screenshots
- mock bills
- fake receipts
- fictional names
- test appointments
- generated conversations

Real personal documents should not be required to develop basic features.

---

# 30. User Research Data

User research is separate from application data.

Research participants should not be asked to provide real sensitive documents unless there is a clear research need and appropriate handling process.

For usability testing, prefer realistic mock captures.

Examples:

- fake appointment confirmation
- fake bill
- fake return receipt
- fictional landlord conversation

Research notes and recordings should not be stored inside the public application repository.

---

# 31. Privacy Testing Checklist

Before testing a new feature, ask:

### Capture

- Does the feature collect more information than necessary?
- Is capture initiated by the user?

### Processing

- Where does the data go?
- Does an external service receive it?
- Is all transmitted information necessary?

### Persistence

- What is saved?
- Why is it saved?
- How long is it required?

### Visibility

- Could information appear unexpectedly in:
  - notifications
  - logs
  - analytics
  - crash reports
  - calendar entries?

### Control

- Can the user review the result?
- Can the user edit it?
- Can the user delete it?

---

# 32. Privacy Red Flags

Development should stop and require an explicit product decision if a proposed feature introduces:

- automatic email scanning
- bank connections
- continuous background monitoring
- persistent storage of all screenshots
- persistent storage of raw voice recordings
- upload of unrelated Reminder Card history to AI
- analytics containing user-written content
- full reminder details appearing in public or shared contexts by default
- hidden data collection
- new sensitive-data categories
- AI provider credentials embedded in the mobile app

These changes materially alter the privacy model of WillDo.

---

# 33. Privacy Success Principle

WillDo should not aim to know as much as possible about the user.

It should aim to know:

> The minimum required to make sure the right thing does not get forgotten.

The product's intelligence should come from understanding individual capture moments, not accumulating unnecessary personal data.

---

# 34. Privacy Change Rule

Before adding any new data collection, storage, or integration, ask:

1. Is this necessary for Future Action Capture?
2. Can the same user value be achieved with less data?
3. Does the data need to leave the device?
4. Does the raw input need to be retained?
5. Does the user clearly understand what is happening?
6. Can the user review and delete the resulting information?
7. Does this change increase the sensitivity of data WillDo handles?
8. Does it introduce a new third-party processor or permission?
9. Would this still feel reasonable if the captured information were personally sensitive?

If the answer reveals unnecessary privacy risk, the feature should be redesigned before implementation.