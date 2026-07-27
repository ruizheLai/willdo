# WillDo — Product Brief

> Status: Draft v0.1
>
> Purpose: This document defines the current product direction for WillDo.
> It serves as a source of truth for product design and MVP development.
> Assumptions in this document may change after user research and testing.

---

## 1. Product Summary

WillDo is a capture-first AI product for things users cannot deal with now but do not want to forget later.

Users can quickly capture information through text, images, or voice. AI helps identify what matters, such as what needs to be done, when it matters, and what the next action may be.

The extracted information is presented as an editable reminder card. Nothing is confirmed until the user reviews it.

WillDo focuses on **Future Action Capture (FAC)** rather than traditional task management.

---

## 2. Core Problem

People regularly encounter small pieces of information that require future action:

- an appointment they need to attend
- something they need to cancel before a deadline
- an item they may need to return
- a bill that needs attention later
- someone they need to follow up with
- an event or registration deadline
- a warranty or renewal date
- a small task they cannot deal with right now

The problem is often not that users do not know how to create a reminder.

The problem is that, at the moment they encounter the information, they:

- do not have time to process it
- do not want to manually structure it
- may not yet know exactly what reminder they need
- want to save it quickly before it disappears

As a result, people use temporary workarounds such as:

- taking screenshots
- sending messages to themselves
- saving messages or posts
- writing quick notes
- leaving browser tabs open
- relying on memory

These methods are fast at capture, but weak at follow-through.

Important information becomes scattered, unstructured, and easy to forget.

---

## 3. Core Insight

Traditional reminder and calendar tools assume that the user has already translated information into an action.

For example, the user is expected to know:

- what the task is
- when it is due
- whether it should be a reminder or calendar event
- what information should be kept
- when they should be reminded

In many real-life situations, users have not done this thinking yet.

WillDo aims to reduce the gap between:

**"I need to deal with this later."**

and

**"This is already captured and ready to remind me."**

---

## 4. Future Action Capture

Future Action Capture (FAC) describes the moment when a user encounters something that:

1. cannot or should not be handled immediately, and
2. requires attention or action in the future.

WillDo is designed around this moment.

The product should make capturing a future action require less cognitive effort than manually creating a structured reminder.

### Core Flow

Capture  
→ Understand  
→ Review  
→ Confirm  
→ Remind  
→ Done

---

## 5. Target Users

### Primary target

Young adults who manage many small personal responsibilities across different apps, messages, screenshots, websites, and offline situations.

They are comfortable using digital tools but do not consistently maintain a structured task-management system.

### Possible characteristics

Users may:

- live independently or rent
- manage their own appointments, bills, subscriptions, purchases, and household matters
- frequently take screenshots to remember things
- send messages or notes to themselves
- forget small tasks because information is distributed across different places
- find traditional task-management tools too structured for quick everyday capture

### Current assumption

The primary user group is intentionally broad at this stage.

User research should determine whether a narrower segment has a significantly stronger need for the product.

---

## 6. Jobs To Be Done

### Primary Job

When I encounter something that I cannot deal with right now but need to remember later,

I want to save it with almost no effort,

so that I can trust that it will not disappear or be forgotten.

### Supporting Jobs

When I capture messy information,

I want the important details to be identified for me,

so that I do not have to manually turn it into a reminder.

When AI interprets something for me,

I want to review what it understood,

so that I remain in control.

When the right time comes,

I want the reminder to be clear and noticeable,

so that I immediately understand what I need to do.

---

## 7. Value Proposition

WillDo reduces the cognitive effort between encountering information and creating a future action.

Instead of asking users to manually decide how information should be stored, WillDo helps turn unstructured input into a clear, editable reminder.

### Product promise

**Capture now. Deal with it later.**

### Working positioning statement

WillDo is a capture-first AI reminder inbox that turns everyday information into future actions.

---

## 8. Primary Input Methods

WillDo is designed around fast capture.

The intended input methods are:

### Text

Users can type or paste unstructured information.

Example:

> Need to cancel my gym membership before August 12.

### Image

Users can upload or share screenshots and photos containing information that may require future action.

Examples:

- appointment confirmation
- event poster
- receipt
- bill
- chat screenshot
- return policy

### Voice

Users can quickly describe something they need to deal with later.

Example:

> Remind me to contact the property manager again next Wednesday if they haven't replied.

---

## 9. Role of AI

AI is an enabling layer, not the primary reason to use the product.

The product should remain useful because it makes future-action capture faster, clearer, and easier.

AI should reduce the amount of interpretation and manual entry required from the user.

### AI may identify

- reminder title
- relevant date
- relevant time
- deadline
- location
- amount
- person or organisation
- likely next action
- suggested reminder time
- possible follow-up
- category

### AI principles

AI should:

- reduce cognitive effort
- structure messy input
- surface uncertainty
- allow correction
- avoid inventing missing information

AI should not:

- silently create confirmed reminders
- make high-impact decisions for the user
- hide uncertainty
- assume information that is not supported by the input

The user remains the final decision-maker.

---

## 10. Core Product Experience

The ideal WillDo interaction should feel closer to dropping something into an inbox than filling in a task form.

### Example

A user receives:

> Your dentist appointment is confirmed for Tuesday, August 18 at 3:30 PM, 120 Collins Street.

Instead of manually opening a calendar or reminder app and recreating the information:

1. The user sends or pastes it into WillDo.
2. WillDo extracts the appointment details.
3. The user sees a compact review card.
4. The user confirms or edits the result.
5. WillDo stores the future action and prepares the appropriate reminder.

The user should not need to decide the structure before capturing the information.

---

## 11. Example Capture Moments

WillDo should support situations such as:

1. Receiving an appointment confirmation while busy.
2. Seeing an event poster with a registration deadline.
3. Buying something with a return window.
4. Receiving a bill that is due later.
5. Needing to cancel a subscription before renewal.
6. Being told to follow up with someone next week.
7. Receiving a landlord or repair update that requires future action.
8. Seeing a warranty expiration date.
9. Getting a pickup or collection deadline.
10. Thinking of something important while unable to deal with it immediately.

These examples are illustrative rather than a fixed feature taxonomy.

---

## 12. Product Principles

### Capture First

The fastest and most prominent action in the product should be capturing something.

WillDo should not require users to organise information before saving it.

### Low Cognitive Load

Users should not have to think like project managers.

Avoid unnecessary fields, prioritisation systems, project structures, or workflow configuration.

### AI Should Remove Work

AI should simplify the workflow rather than create an additional AI interface users must manage.

### Review Before Action

AI-extracted information must remain editable and should be confirmed by the user before becoming a saved reminder.

### Clear Over Comprehensive

It is better to show the few pieces of information that matter than every detail AI can extract.

### Privacy by Default

Only information necessary for the feature should be processed or stored.

The product should avoid requiring broad access to users' email, financial accounts, or devices.

---

## 13. What WillDo Is Not

WillDo is not intended to become:

- a generic to-do list
- a project management tool
- a Kanban board
- a notes application
- a calendar replacement
- a document vault
- an email assistant
- a financial management product
- a family management platform
- a general-purpose AI assistant

Existing task and calendar products already perform many management functions well.

WillDo should focus on the moment before those systems become useful:

**capturing and understanding future action.**

---

## 14. Relationship With Existing Tools

WillDo does not need to replace existing calendars or reminder systems.

A future version may allow confirmed actions to be sent to system calendars or reminder services.

WillDo's value is primarily in helping the user get from:

unstructured information

to

a structured future action.

### Working competitive distinction

Notes help users store information.

Calendars help users manage events.

Reminder apps help users manage known reminders.

WillDo helps users **discover and capture the future actions hidden inside everyday information.**

---

## 15. Privacy Position

WillDo may process personal everyday information, so privacy should be treated as a product requirement rather than an afterthought.

For the MVP:

- no email account connection
- no bank account connection
- no automatic device scanning
- user-initiated capture only
- AI output must be reviewed before saving
- original images should not be retained by default
- highly sensitive documents are outside the intended MVP use case
- users must be able to delete saved information

More detailed rules are defined in `PRIVACY_RULES.md`.

---

## 16. MVP Direction

The first MVP will be a native mobile application designed around fast, low-friction Future Action Capture.

The product will be built for mobile from the beginning because WillDo's core interaction happens in everyday moments when users encounter information they cannot deal with immediately.

The MVP will be:

- mobile-first and native
- built with React Native and Expo
- local-first
- usable without an account
- designed to be cloud-ready
- focused on validating the complete Future Action Capture workflow

The MVP should eventually support the three primary capture methods:

- text
- image
- voice

The core MVP experience is:

Capture  
→ AI Understand  
→ Review  
→ Confirm  
→ Remind  
→ Done

Development will still be incremental.

### Phase 1 — Core Interaction

Validate the interaction model before adding real AI or device integrations.

Text capture  
→ mock extraction  
→ review  
→ confirm  
→ local reminder card  
→ inbox

### Phase 2 — AI Understanding

Replace mock extraction with real AI-powered structured extraction while preserving the same review experience.

### Phase 3 — Multimodal Capture

Add image capture/upload and voice capture so users can save future actions in the form most convenient at the moment.

### Phase 4 — Native Reminder Experience

Add native local notifications and reminder interactions so confirmed future actions can actively return to the user's attention.

### Phase 5 — System Integration

Explore calendar integration and faster system-level capture workflows where they meaningfully reduce capture friction.

Account creation, cloud storage, cross-device sync, and more advanced integrations are intentionally excluded from the initial local-first MVP.

The architecture should allow these capabilities to be introduced later without redesigning the core product model.

See `MVP_SCOPE.md` for detailed implementation scope.

---

## 17. Key Product Hypotheses

The following assumptions should be validated through research and testing.

### H1 — Capture friction

Users encounter future actions more often than they formally record them because creating structured reminders requires more effort than temporary capture methods such as screenshots or self-messages.

### H2 — AI value

AI-assisted extraction can meaningfully reduce the cognitive effort required to convert unstructured information into a useful reminder.

### H3 — Review is acceptable

Users will tolerate a short review step if AI has already completed most of the structuring work.

### H4 — Capture-first positioning

Users will understand value in a product focused primarily on capture even if reminder and calendar management already exist elsewhere.

### H5 — Trust

User-controlled capture and review will create a lower trust barrier than products that automatically scan email, bank accounts, or other personal sources.

---

## 18. Early Success Criteria

For the MVP, success does not mean replacing a user's existing reminder or calendar app.

The product should demonstrate that users can:

1. understand what WillDo is for
2. recognise moments when they would use it
3. capture an item quickly
4. understand AI-extracted information
5. correct extraction mistakes easily
6. confirm a reminder without unnecessary effort
7. find captured items later
8. feel that the process is easier than their current workaround

---

## 19. Open Questions

These questions are intentionally unresolved and should be explored through research and testing:

- What types of Capture Moments occur most frequently?
- What do users currently do when they cannot process information immediately?
- Why do users choose screenshots, self-messages, Notes, Reminders, or Calendar?
- How much review is acceptable before saving?
- What information should appear on a reminder card by default?
- How visible should reminders be?
- Should WillDo primarily remind users itself or hand reminders off to system tools?
- Which image-based scenarios provide the strongest value?
- How important is voice capture compared with text and images?
- Is the initial target user group too broad?
- What level of AI confidence should be communicated to users?

---

## 20. North Star

Every major product decision should answer the following question:

> Does this make it easier for users to capture something they need to deal with later?

If a feature primarily improves task management rather than Future Action Capture, it should not automatically become part of WillDo.