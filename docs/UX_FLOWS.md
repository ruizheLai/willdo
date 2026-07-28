# WillDo — UX Flows

> Status: Draft v0.1  
> Purpose: This document defines the primary user flows and interaction principles for the WillDo MVP.
>
> It describes how users move through Future Action Capture, AI extraction, review, confirmation, reminders, and resolution.
>
> This document defines interaction logic rather than final visual design.

---

# 1. Core UX Goal

WillDo should reduce the effort required to move from:

> "I need to deal with this later."

to:

> "It's captured. I won't forget."

The core interaction should feel closer to dropping something into an inbox than manually creating a task.

The user should not need to decide:

- which app the information belongs in
- how to structure the reminder
- which category to use
- which fields to fill
- what information matters

before capturing it.

The intended mental model is:

```text
Drop it in.
WillDo figures out the structure.
I quickly check it.
Done.
```

---

# 2. Core Product Loop

The primary WillDo lifecycle is:

```text
Capture
↓
Understand
↓
Review
↓
Confirm
↓
Remind
↓
Resolve
```

Each stage has a different responsibility.

## Capture

The user gives WillDo something they cannot deal with right now.

## Understand

AI identifies the likely Future Action and relevant information.

## Review

The user checks what WillDo understood.

## Confirm

The user accepts the Future Action and reminder.

## Remind

WillDo brings the Future Action back to the user's attention.

## Resolve

The user handles the action and marks it complete.

---

# 3. UX Principles

## 3.1 Capture Is the Primary Action

The most prominent action in WillDo should always support starting a capture.

The user should not have to navigate through:

```text
Projects
→ Lists
→ Categories
→ Create Task
```

before entering information.

The intended interaction is:

```text
Open WillDo
→ Capture
```

---

## 3.2 Capture Before Structure

Users should provide information before being asked to organise it.

Avoid capture flows that begin with:

```text
Choose category
Choose task type
Choose due date
Choose reminder type
Enter title
```

Prefer:

```text
Tell WillDo what happened
↓
WillDo proposes structure
```

---

## 3.3 Review Should Be Faster Than Manual Entry

The review screen should not recreate a traditional task form.

AI should have already done most of the work.

The review experience should focus attention on the fields that matter most:

```text
What?
When?
Remind me when?
What do I need to do?
```

Secondary details should not dominate the experience.

---

## 3.4 Progressive Disclosure

Show only what is needed at the current moment.

For example:

Primary review fields:

- title
- target date/time
- reminder timing
- next action

Secondary information:

- category
- location
- amount
- contact/provider
- context

Secondary information may be shown below the primary information or behind an optional expansion.

---

## 3.5 AI Should Not Create Unnecessary Conversation

WillDo is not a chatbot.

Avoid:

```text
AI:
Hi! How can I assist you today?

User:
I need to remember...

AI:
Of course! I'd be happy to help...
```

Prefer:

```text
User capture
↓
Processing
↓
Structured result
```

Conversation should appear only when clarification is genuinely needed.

---

## 3.6 Preserve User Control

Users must be able to:

- edit AI output
- reject AI suggestions
- change reminder timing
- cancel before saving
- manually enter missing information
- delete saved cards

AI accelerates the workflow but does not remove user control.

---

# 4. Primary App Structure

The MVP should keep navigation simple.

Recommended high-level structure:

```text
Inbox
Capture
Completed / History
```

The exact navigation pattern may change after design exploration.

The MVP does not need:

```text
Projects
Folders
Workspaces
Boards
Goals
Teams
```

---

# 5. Recommended Home / Inbox Structure

The Inbox is the primary home screen after initial onboarding.

Its purpose is to answer:

> What requires my attention?

Possible structure:

```text
WillDo

[ Capture something... ]

Today
────────────────
Return headphones
Due today

Tomorrow
────────────────
Dentist appointment
3:30 PM

Coming up
────────────────
Cancel gym membership
12 Aug
```

The capture entry point should remain highly visible.

---

# 6. First Launch Flow

The onboarding should explain the product with minimal friction.

Recommended flow:

```text
Launch WillDo
↓
Short value explanation
↓
Example Capture Moment
↓
Enter Inbox
```

Avoid requesting all permissions immediately.

---

## 6.1 Suggested First Screen

Possible message:

```text
Things you can't deal with now?
Drop them here.

WillDo turns screenshots, text, and voice into reminders you can review and confirm.
```

Primary action:

```text
Get started
```

---

## 6.2 Optional Example

A short example may demonstrate:

```text
"Return headphones before August 20"

↓ WillDo

Return headphones
Deadline: Aug 20
Remind: Aug 18
```

This should explain the core product model faster than a feature list.

---

## 6.3 Permissions During Onboarding

Do not request:

- notifications
- camera
- photo library
- microphone
- calendar

all at once.

Permissions should be requested contextually when the user first uses the relevant feature.

---

# 7. Capture Entry Flow

The user should be able to start a capture from the Inbox with one obvious action.

Possible interaction:

```text
[ + Capture ]
```

or a persistent capture input.

Opening Capture presents:

```text
Text
Photo
Voice
```

The exact visual treatment is TBD.

The selection should require minimal decision-making.

---

# 8. Text Capture Flow

## 8.1 Happy Path

```text
Inbox
↓
Tap Capture
↓
Text input
↓
Enter or paste information
↓
Submit
↓
AI processing
↓
Review
↓
Confirm
↓
Reminder Card saved
↓
Inbox
```

Example input:

```text
Need to cancel my gym membership before August 12.
```

AI result:

```text
Cancel gym membership

Deadline
12 Aug

Remind me
10 Aug, 9:00 AM

Next action
Cancel membership before renewal
```

Primary action:

```text
Confirm
```

Secondary action:

```text
Edit
```

---

## 8.2 Text Capture Screen

The interface should support:

- typing
- pasting
- multiline text
- quick submission

Placeholder example:

```text
What do you need to deal with later?
```

Avoid requiring a title before submission.

---

## 8.3 Empty Submission

If the user submits without content:

```text
Add something first.
```

Do not create an extraction request.

---

# 9. Image Capture Flow

## 9.1 Entry

```text
Inbox
↓
Capture
↓
Photo
```

The user may choose:

```text
Take Photo
Choose Existing Image
```

depending on platform capability and MVP implementation.

---

## 9.2 Permission Flow

If photo-library permission is needed:

```text
User chooses Photo
↓
System permission request
```

The permission request should happen only after the user chooses the feature.

If denied:

```text
WillDo needs photo access to use an existing image.

[Open Settings]
[Use Text Instead]
```

The rest of the app should remain usable.

---

## 9.3 Happy Path

```text
Select screenshot/photo
↓
Preview selected image
↓
Submit
↓
AI processing
↓
Review extracted Future Action
↓
Confirm
↓
Original image discarded
↓
Reminder Card saved
```

The user should understand that the persistent result is the Future Action, not the screenshot.

---

## 9.4 Image Preview

Before processing, provide:

```text
Selected image
[Use this image]
[Choose another]
```

Possible privacy note:

```text
Only the reminder details you confirm will be saved.
```

Any privacy wording must accurately reflect implementation.

---

## 9.5 Multiple Relevant Dates

Example screenshot contains:

```text
Purchased: August 1
Return by: August 30
```

WillDo should not silently choose.

Flow:

```text
Image
↓
AI detects ambiguity
↓
Review / Clarification
```

Possible UX:

```text
Which date matters?

○ Purchase date — Aug 1
● Return deadline — Aug 30

[Continue]
```

Prefer selectable answers over open-ended conversation where possible.

---

# 10. Voice Capture Flow

## 10.1 Entry

```text
Inbox
↓
Capture
↓
Voice
```

---

## 10.2 Microphone Permission

Ask only when voice capture is first used.

If denied:

```text
Microphone access is needed for voice capture.

[Open Settings]
[Type Instead]
```

---

## 10.3 Happy Path

```text
Start recording
↓
User speaks
↓
Stop recording
↓
Speech-to-text
↓
AI extraction
↓
Review
↓
Confirm
↓
Reminder Card saved
```

Example:

```text
Remind me to contact the property manager next Wednesday if they haven't replied.
```

---

## 10.4 Voice Capture State

The interface should clearly communicate:

```text
Recording
```

and provide:

```text
Stop
Cancel
```

The app should never appear to record without an obvious active state.

---

## 10.5 Transcript Review

The MVP may show the transcript before or alongside extraction if useful.

However, avoid introducing an unnecessary extra confirmation step.

Preferred:

```text
Voice
↓
Transcript
↓
AI extraction
↓
Reminder review
```

with transcript editing available when recognition is clearly wrong.

The goal is not:

```text
Record
→ confirm transcript
→ confirm AI
→ confirm reminder
```

unless testing demonstrates that this level of review is required.

---

# 11. Processing State

AI extraction may require noticeable processing time.

The processing screen should communicate progress without pretending certainty.

Example:

```text
Finding what matters...
```

or:

```text
Turning this into a reminder...
```

Avoid anthropomorphic messages such as:

```text
I'm thinking really hard!
```

---

## 11.1 Long Processing

If extraction takes longer than expected, the app should remain responsive.

Possible state:

```text
Still working on it...
```

Provide:

```text
Cancel
```

where technically practical.

---

# 12. Review Flow

Review is the most important trust interaction in the product.

The user should answer:

```text
Did WillDo understand this correctly?
```

without having to rebuild the reminder manually.

---

## 12.1 Recommended Information Hierarchy

### Primary

```text
Title
Target date/time
Reminder time
Next action
```

### Secondary

```text
Category
Location
Amount
Contact/provider
Context
```

---

## 12.2 Example Review

```text
Ready to remind you

Cancel gym membership

Deadline
12 Aug

Reminder
10 Aug · 9:00 AM

Next action
Cancel membership before renewal


More details
Gym · Deadline


[Confirm]
```

Fields should be directly editable where appropriate.

---

## 12.3 Editing

Tapping a field should allow focused editing.

Example:

```text
Deadline
12 Aug
```

Tap:

```text
[Date picker]
```

Avoid sending the user to a large generic edit form unless necessary.

---

## 12.4 User-Changed Fields

After the user edits an AI-extracted value, their value becomes authoritative.

Example:

```text
AI:
Aug 12

User edits:
Aug 13
```

WillDo should use:

```text
Aug 13
```

without attempting to override the change.

---

# 13. Clarification Flow

Clarification should occur only when uncertainty prevents a useful reminder.

Example:

```text
Alex may know Thursday or Friday.
Message him if he hasn't replied.
```

Possible UI:

```text
When should WillDo bring this back?

[Thursday]
[Friday]
[Choose another date]
```

This is preferable to:

```text
AI: Can you clarify what you mean?
```

---

## 13.1 Clarification Principle

Prefer:

```text
Specific question
+
Simple choices
```

over:

```text
Open conversation
```

---

## 13.2 Missing Reminder Timing

Example input:

```text
Remember to cancel my membership.
```

WillDo can understand the action but not the timing.

Possible review:

```text
Cancel membership

When should WillDo remind you?

[Today]
[Tomorrow]
[Choose date]
```

AI extraction can still provide value without complete information.

---

# 14. No Future Action Detected

WillDo should not force all input into a reminder.

Example:

```text
Great coffee shop on Swanston Street.
```

Possible result:

```text
I'm not sure what you want to do with this.

What should WillDo remind you about?

[Add action]
[Cancel]
```

The product should not silently become a general Notes app.

---

# 15. Multiple Future Actions

MVP assumes:

```text
One Capture
→ One Primary Reminder Card
```

Example:

```text
Friday go to Costco, buy milk, and return the headphones.
```

Possible UI:

```text
I found more than one thing to do.

What do you want to capture?

○ Costco trip
○ Return headphones

[Continue]
```

Automatic multi-card creation is not required for MVP.

---

# 16. Extraction Failure

Possible causes:

- network failure
- model failure
- unreadable image
- unsupported format
- invalid AI response
- speech transcription failure

Expected UX:

```text
Couldn't understand this capture.

[Try Again]
[Enter Manually]
```

Do not discard the user's input immediately after failure if it can safely be retained temporarily for retry.

---

# 17. Manual Fallback

AI should never be the only way to create a Reminder Card.

If AI fails, users should be able to provide the minimum required information manually.

Possible minimum:

```text
What?
When?
Remind me when?
```

Manual fallback should remain lightweight.

WillDo should not expose every possible schema field as a required form.

---

# 18. Confirmation Flow

When the user confirms:

```text
Review
↓
Confirm
↓
Create ReminderCard
↓
Persist locally
↓
Schedule notification
↓
Return to Inbox
```

If notification scheduling fails but the card saves successfully:

```text
Reminder saved

Notification couldn't be scheduled.

[Try Again]
```

The card itself should not disappear because notification scheduling failed.

---

# 19. Post-Confirmation Feedback

Confirmation should feel immediate.

Possible feedback:

```text
Done — I'll bring this back on Aug 10.
```

Then return to:

```text
Inbox
```

The product should avoid unnecessary success screens that slow capture.

---

# 20. Inbox Flow

The Inbox should focus on future attention rather than organisation.

Possible grouping:

```text
Needs attention
Today
Tomorrow
Coming up
Later
```

Exact grouping is TBD and should be tested.

Avoid complex organisational layers.

---

## 20.1 Inbox Card

A compact card may show:

```text
Return headphones
Aug 20

Remind Aug 18
```

or:

```text
Dentist appointment
Tomorrow · 3:30 PM
120 Collins Street
```

The card should answer:

```text
What?
When?
```

without requiring the user to open it.

---

## 20.2 Visual Priority

Items requiring immediate attention should be easier to notice.

Priority should be derived primarily from time and status rather than user-created priority systems.

The MVP does not require:

```text
High Priority
Medium Priority
Low Priority
```

unless research later demonstrates a need.

---

# 21. Reminder Notification Flow

The reminder lifecycle is:

```text
Confirmed ReminderCard
↓
Local notification scheduled
↓
Scheduled time arrives
↓
Native notification appears
↓
User taps notification
↓
Relevant ReminderCard opens
```

---

## 21.1 Notification Content

Notification content should be concise.

Example:

```text
WillDo

Return headphones
Return deadline is in 2 days.
```

For privacy-sensitive items, a more limited presentation may later be needed.

Notification privacy behaviour is defined in `PRIVACY_RULES.md`.

---

## 21.2 Opening a Notification

Tapping the notification should ideally open:

```text
Reminder Card Detail
```

not simply the generic Inbox.

This helps preserve context.

---

# 22. Notification Permission Flow

Notification permission should not be requested immediately on first launch.

Preferred:

```text
User creates first reminder
↓
Before scheduling
↓
Explain value
↓
Request notification permission
```

Possible pre-permission message:

```text
Want WillDo to bring this back at the right time?

Enable notifications so your reminders can reach you.
```

Then:

```text
[Enable Notifications]
```

followed by the OS permission prompt.

---

## 22.1 Notification Permission Denied

If denied:

```text
Reminder saved

Notifications are off, so WillDo can't alert you outside the app.

[Open Settings]
```

The card should still be saved.

---

# 23. Reminder Card Detail Flow

Opening a card should show enough context to act.

Example:

```text
Return headphones

Deadline
Aug 20

Reminder
Aug 18 · 9:00 AM

Next action
Return headphones before the return window closes.

Store
Example Store


[Mark Done]
```

Secondary actions:

```text
Edit
Delete
```

P1 may include:

```text
Add to Calendar
```

when appropriate.

---

# 24. Mark as Done

Flow:

```text
Reminder Card
↓
Mark Done
↓
status = done
↓
Cancel future notification if still scheduled
↓
Move out of active Inbox
```

Confirmation should normally be immediate.

Possible feedback:

```text
Done
```

Avoid a modal such as:

```text
Are you sure you want to mark this task complete?
```

for a reversible low-risk action.

---

# 25. Completed Items

Completed items should remain accessible during MVP testing.

Possible structure:

```text
Completed
```

Users may:

- view completed cards
- restore them to active if necessary
- delete them

The final retention behaviour remains a product question.

---

# 26. Edit Existing Card

Flow:

```text
Card Detail
↓
Edit
↓
Change fields
↓
Save
```

If reminder timing changes:

```text
Cancel old native notification
↓
Schedule new notification
```

Editing a Reminder Card should not re-run AI extraction unless explicitly requested.

User-confirmed data is authoritative.

---

# 27. Delete Flow

Deleting a card removes it from the user's WillDo experience.

Flow:

```text
Card Detail
↓
Delete
↓
Confirmation
↓
Cancel scheduled notification
↓
Delete card
↓
Return to Inbox
```

Because deletion is destructive, confirmation is appropriate.

Example:

```text
Delete this reminder?

This can't be undone.

[Cancel]
[Delete]
```

---

# 28. Calendar Integration — P1

Calendar integration should only appear when a card represents something calendar-like.

Example:

```text
Dentist appointment
Aug 18 · 3:30 PM
```

Possible action:

```text
Add to Calendar
```

Not every card should show this action.

Example:

```text
Return headphones before Aug 20
```

may be better represented as a WillDo reminder than a calendar event.

---

## 28.1 Calendar Flow

```text
Card
↓
Add to Calendar
↓
Preview event details
↓
Calendar permission if required
↓
Create event
↓
Success feedback
```

Users should review exported information before creation.

---

# 29. Follow-Up Suggestion — P1

Example capture:

```text
The property manager said the plumber should contact me this week.
```

WillDo may suggest:

```text
Follow up Friday if you haven't heard anything.
```

The user should explicitly confirm this.

Possible review:

```text
Suggested follow-up

Friday · 9:00 AM

[Use suggestion]
[Change]
[Skip]
```

Do not silently create conditional future actions.

---

# 30. Search — P1

Search should support finding saved Future Actions.

Example:

```text
Search reminders
```

Possible queries:

```text
headphones
dentist
landlord
gym
```

Initial search may operate over:

- title
- nextAction
- contact/provider
- context

Advanced semantic AI search is not required for MVP.

---

# 31. Empty Inbox

Empty states should reinforce the Capture-first model.

Example:

```text
Nothing waiting for you.

Got something you can't deal with right now?

[Capture it]
```

Avoid:

```text
You have no tasks.
```

because WillDo is not positioned as a traditional task manager.

---

# 32. First Capture Empty State

Before the user has ever captured something:

```text
Things disappear when they're left in screenshots, messages, and memory.

Drop something here and WillDo will help turn it into a reminder.
```

Then show:

```text
[Capture something]
```

Potential examples may be offered underneath.

---

# 33. Loading and Error Principles

Every asynchronous operation should clearly expose one of:

```text
idle
loading
success
error
```

The interface should never leave the user unsure whether:

```text
something is still processing
something was saved
something failed
```

---

# 34. Offline Behaviour

Because the MVP is local-first, existing Reminder Cards should remain available without internet access.

Operations that do not require AI should continue to work offline where practical:

- view Inbox
- view card
- edit card
- mark done
- delete card
- manually create information

AI extraction may require connectivity.

If unavailable:

```text
You're offline.

You can save this manually or try AI when you're connected.

[Enter Manually]
[Try Later]
```

The exact offline capture behaviour may evolve later.

---

# 35. App Relaunch Behaviour

When WillDo is reopened:

```text
Launch
↓
Load local Reminder Cards
↓
Show Inbox
```

The app should not require onboarding again.

If a capture was interrupted before confirmation, MVP may discard it unless restoring drafts becomes clearly valuable.

Persisted abandoned drafts are not required for v0.1.

---

# 36. Deep Link / Notification Navigation

Where possible, each Reminder Card should support navigation by ID.

Conceptually:

```text
willdo://reminder/{id}
```

or the routing equivalent provided by Expo Router.

This allows:

- notification tap
- future widgets
- future Share Sheet integrations
- future external links

to open the relevant card.

The exact URI format is an implementation detail.

---

# 37. Future Share Sheet Flow — P2

Share Sheet integration strongly supports the Capture-first vision but is not required for the earliest MVP.

Ideal future flow:

```text
User sees information in another app
↓
Share
↓
WillDo
↓
Content received
↓
AI extraction
↓
Review
↓
Confirm
```

Example:

```text
Event page in browser
↓
Share to WillDo
↓
Registration closes Friday
↓
Review
↓
Confirmed reminder
```

This would reduce the need to:

```text
Screenshot
→ open WillDo
→ upload screenshot
```

and should be explored after the core capture workflow is validated.

---

# 38. Future Home-Screen Quick Capture — P2

Possible future entry points:

```text
Long-press app icon
→ Text Capture
→ Photo Capture
→ Voice Capture
```

or a widget.

These should be evaluated based on whether they materially reduce Capture Moment friction.

---

# 39. Key UX States

The MVP should eventually account for:

```text
First launch
Empty Inbox
Active Inbox
Capture
Processing
Review
Clarification
Confirmation
Extraction failure
Permission denied
Offline
Notification received
Card detail
Editing
Completed
Deletion
```

Design should not focus only on the happy path.

---

# 40. Core Flow 1 — Text Capture

```text
Inbox

↓

Capture

↓

Text

↓

"Cancel gym membership before Aug 12"

↓

AI Processing

↓

Review

Title
Cancel gym membership

Deadline
Aug 12

Reminder
Aug 10 · 9:00 AM

Next action
Cancel before renewal

↓

Confirm

↓

Save Card

↓

Schedule Notification

↓

Inbox
```

---

# 41. Core Flow 2 — Image Capture

```text
Inbox

↓

Capture

↓

Photo

↓

Choose Screenshot

↓

Preview

↓

AI Processing

↓

Extract:

Dentist appointment
Aug 18 · 3:30 PM
120 Collins Street

↓

Review

↓

Confirm

↓

Discard Original Image

↓

Save Card

↓

Schedule Notification

↓

Inbox
```

---

# 42. Core Flow 3 — Voice Capture

```text
Inbox

↓

Capture

↓

Voice

↓

Record

"Contact Sam next Wednesday if he hasn't replied."

↓

Speech to Text

↓

AI Processing

↓

Review

Follow up with Sam
Next Wednesday

↓

Confirm

↓

Save Card

↓

Schedule Notification
```

---

# 43. Core Flow 4 — Ambiguous Capture

```text
Capture

↓

"Alex said Thursday or Friday"

↓

AI Processing

↓

Ambiguity detected

↓

When should you follow up?

[Thursday]
[Friday]
[Choose another date]

↓

User selects Friday

↓

Review completed Future Action

↓

Confirm
```

---

# 44. Core Flow 5 — Reminder Return

```text
Reminder scheduled

↓

Time arrives

↓

Native Notification

"Return headphones
Deadline in 2 days"

↓

User taps

↓

Reminder Card Detail

↓

User takes action

↓

Mark Done

↓

Notification cancelled if necessary

↓

Card moves to Completed
```

This flow is critical because it closes the Future Action Capture loop.

---

# 45. Core Flow 6 — AI Failure

```text
Capture

↓

AI Processing

↓

Extraction fails

↓

Couldn't understand this capture.

[Try Again]
[Enter Manually]

↓

Manual input

What?
When?
Remind when?

↓

Confirm

↓

Save
```

AI failure should degrade gracefully rather than block the product.

---

# 46. Capture Performance Goal

WillDo should eventually measure the effort required to move from Capture Moment to confirmed Future Action.

Potential UX metrics:

```text
Time to Confirm
Number of taps
Number of edited AI fields
Clarification rate
Capture abandonment rate
```

The exact targets should be established after baseline testing.

The principle is:

> The capture workflow should feel meaningfully easier than manually creating the equivalent reminder.

---

# 47. Review Effort

Review Effort is a key AI + UX metric.

Possible measures:

```text
% of captures confirmed without editing
average fields edited per capture
% requiring clarification
time spent on review
```

A strong AI extraction should reduce review effort without removing user control.

---

# 48. Reminder Effectiveness

WillDo should eventually evaluate whether captured items successfully return to attention.

Possible signals:

```text
notification delivered
notification opened
card completed
card completed after notification
reminder rescheduled
```

These signals should not require collecting the private contents of Reminder Cards.

---

# 49. Accessibility Principles

The native MVP should consider:

- readable text sizing
- sufficient touch target size
- screen reader labels
- meaningful button labels
- colour-independent status communication
- reduced motion where appropriate
- keyboard behaviour for text entry
- clear permission/error messaging

Accessibility should be considered during component development rather than added only after visual polish.

---

# 50. UX Anti-Patterns

WillDo should avoid the following unless research proves they are necessary.

## Large Task Forms

Do not make users complete:

```text
Title
Description
Project
Priority
Tags
Start date
Deadline
Reminder
Repeat
Location
Assignee
```

for ordinary capture.

---

## Forced Categorisation Before Capture

Do not ask:

```text
What kind of reminder is this?
```

before receiving the information.

---

## AI Chat as Default UI

Do not require users to have a conversation to create every reminder.

---

## Automatic AI Actions Without Review

Do not silently schedule reminders from AI extraction.

---

## Dashboard Complexity

Do not introduce:

- productivity scores
- completion charts
- project dashboards
- Kanban boards

into the core MVP.

---

## Excessive Confirmation

Do not require confirmation for every harmless action.

Review should exist where it protects accuracy or user control.

---

# 51. UX Decision Rule

Before adding a new interaction, ask:

1. Does it reduce Capture Moment friction?
2. Does it help the user understand the Future Action?
3. Does it reduce AI uncertainty?
4. Does it help bring the action back at the right time?
5. Does it help the user resolve the action?
6. Does it add unnecessary management work?

If an interaction increases user effort without meaningfully improving one of these stages:

```text
Capture
Understand
Review
Remind
Resolve
```

it should be reconsidered.

---

# 52. Primary UX Hypothesis

WillDo should feel faster than creating a traditional reminder because the user provides information in the form they already have.

Traditional flow:

```text
See information
↓
Interpret information
↓
Open reminder app
↓
Create title
↓
Choose date
↓
Choose time
↓
Choose reminder
↓
Add supporting information
↓
Save
```

WillDo:

```text
See information
↓
Drop into WillDo
↓
Review
↓
Confirm
```

The MVP should validate whether this reduction in cognitive and interaction effort is meaningful to real users.

---

# 53. North Star UX Question

For every WillDo interaction, ask:

> Does this help the user move from "I'll deal with it later" to "I know I won't forget" with less effort?

If not, the interaction is probably outside WillDo's core experience.