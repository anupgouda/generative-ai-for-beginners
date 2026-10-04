# Lesson 12 — Designing UX for AI Applications

## Overview

AI UX is not just about making an AI model work. It is about designing how users understand, interact with, correct, and trust the AI system.

AI applications differ from traditional applications because their outputs can be dynamically generated and may be correct, incomplete, uncertain, incorrect, or unexpected.

Therefore, AI UX must account for:

- Uncertainty
- AI limitations
- Feedback
- Context
- Errors
- Transparency
- Recovery

---

# Q1. What is AI UX?

AI UX is the design of how users interact with an AI-powered application.

Traditional applications generally follow predictable workflows:

User → Button/Form → Predefined Operation → Result

AI applications can use natural language:

User → Natural Language → AI → Generated Response

Traditional UX is mostly predictable and deterministic, while AI UX must handle dynamic and probabilistic outputs.

| Traditional UX | AI UX |
|---|---|
| Mostly predictable | Output can vary |
| Buttons/forms | Natural language + other interaction methods |
| Fixed workflows | Dynamic interactions |
| Mostly deterministic | Can be probabilistic |
| System errors | AI can misunderstand or hallucinate |
| Less uncertainty communication | Must communicate limitations |

---

# Q2. Why is UX particularly important for AI applications?

### 1. AI can make mistakes

AI can generate incorrect answers, so users should not assume that everything the AI says is automatically true.

### 2. Users need to understand what the AI is doing

The UI should communicate whether the system is:

- Generating an answer
- Searching a database
- Calling an API
- Processing a document
- Waiting for another system

### 3. Users need recovery mechanisms

Users should be able to:

- Correct the AI
- Retry
- Edit their request
- Provide additional information
- Start a new conversation

### 4. AI output is not always predictable

Similar questions can sometimes produce different responses, so the UX must handle variability.

### 5. Trust is important

For applications such as a college inventory system, users need to understand where information about stock, purchase orders, vendors, approvals, and tickets came from.

---

# Q3. What should an AI application communicate?

An AI application should clearly communicate:

## AI limitations

Example:

> AI-generated responses may contain errors. Verify important information.

## Uncertainty

If information cannot be verified, the AI should not pretend to know.

Example:

> I couldn't verify the assignment deadline from the available college data.

## Generated content

AI-generated content can be explicitly labelled:

> ✨ AI-generated response

## Errors

If an API or database fails, the UI should communicate the actual failure.

Example:

> I couldn't access the assignment database. Please try again.

It should not incorrectly display "No assignments found."

## Expectations

The UI should communicate what the AI can and cannot do.

Example:

```text
I can help you:
✓ Find assignments
✓ Check deadlines
✓ Search college policies

I cannot:
✗ Change official grades
✗ Approve leave requests
Q4. What is an AI interaction pattern?

An AI interaction pattern is a common way users interact with an AI system.

Examples:

1. Chat
User → "Show my pending assignments."
AI → "You have 3 pending assignments."
2. Natural-language search
User → "Find all laptops assigned to CSE."
3. AI-assisted forms

The user fills out a form while AI helps complete or validate information.

4. AI suggestions

Example:

Write email
     ↓
AI suggests:
"Request for assignment extension"
5. Voice interaction
User speaks
     ↓
AI understands
     ↓
Response
6. AI actions

The AI can interact with tools:

User
 ↓
AI
 ↓
Create support ticket

A good AI application can combine multiple interaction patterns.

Q5. Why should users be able to correct or refine AI output?

AI can misunderstand user intent.

For example:

User:
"Show my AI assignments."

AI:
"Here are your Artificial Intelligence assignments."

User:
"I meant assignments related to AI projects."

The UI should provide mechanisms such as:

[Correct Response] [Refine]

This creates a better experience than forcing users to start over.

Q6. What is conversational UX?

Conversational UX is the design of user interactions using natural conversation.

Instead of navigating:

Assignments
 ↓
Subject
 ↓
Semester
 ↓
Date
 ↓
Search

the user can simply ask:

Show me my AI assignments due this week.

The AI interprets the request and returns the result.

Traditional UX
Click
 ↓
Select
 ↓
Fill form
 ↓
Submit
Conversational UX
User asks question
 ↓
AI understands intent
 ↓
Result

Conversational UX is especially useful when users do not know exactly where information is located.

Q7. Why is context important?

Context allows the AI to understand references from previous interactions.

Example:

User:
"Show my AI assignments."

AI:
"Here are your AI assignments."

User:
"Which one is due first?"

The AI understands that "which one" refers to the previously displayed assignments.

Another example:

User:
"Show A4 paper stock."

AI:
"There are 100 units."

User:
"Where is it stored?"

AI:
"The 100 units are stored in the Main Store."

Context makes conversations more natural.

Q8. What problems occur if an AI remembers too much context?

Remembering everything is not always beneficial.

Privacy

The system may retain sensitive information unnecessarily.

Security

Sensitive information could be exposed to unauthorized users if context is not properly controlled.

Incorrect assumptions

For example:

Old context:
"I'm working with the CSE department."

Later:
User moves to AIML.

If the AI continues assuming CSE, it may produce incorrect results.

Outdated information

Example:

Old:
Assignment deadline = October 10

New:
Assignment deadline = October 15

The AI should use current authoritative data instead of relying on stale context.

Principle

Context should be relevant, secure, and current.

Q9. What is transparency in AI UX?

Transparency means making it clear that the user is interacting with AI and helping the user understand where responses come from.

Example:

🤖 College AI Assistant

AI-generated response

Source: Academic Regulations 2026

The UI can also show system activity:

🔎 Searching college database...

or:

⚡ Checking inventory system...

For data-based responses:

Source: Inventory Database
Last updated: 10:42 AM

This improves trust and helps users understand the origin of information.

Q10. How can an AI application remain usable when AI makes a mistake?

Useful UX techniques include:

1. Allow correction
Was this helpful?

👍 Yes
👎 No
2. Provide retry
[Try Again]
3. Allow refinement
[Refine Question]
4. Show sources

Users can verify important information.

5. Provide fallback options
AI unavailable

[Retry]
[Search manually]
[Contact Support]
6. Do not hide failures

Bad:

No records found.

when the database is actually unavailable.

Better:

The inventory database is currently unavailable.

Challenge 1 — College AI Assistant

For the question:

When is my AI assignment due?

The UI should contain:

Chat history
User messages
AI responses
Loading indicator
Input box
Send button
Retry button
Feedback/correction controls

Example:

┌────────────────────────────────────────────┐
│ 🤖 College AI Assistant                   │
├────────────────────────────────────────────┤
│ Student:                                   │
│ When is my AI assignment due?              │
│                                            │
│ 🤖 AI Assistant:                           │
│ Your AI assignment is due on October 15.   │
│                                            │
│ 📚 Source: Assignment Database             │
│ 🕐 Last updated: 10:42 AM                  │
│                                            │
│ [View Assignment] [Correct] [Retry]        │
├────────────────────────────────────────────┤
│ Ask something...                    [Send] │
└────────────────────────────────────────────┘

If information cannot be found, the AI should not guess.

Instead:

I couldn't find a verified deadline for this assignment in the college assignment system.

Possible actions:

[Search Again]
[View Assignments]
[Contact Faculty]

Sources should appear directly below the answer.

Example:

Answer
Your assignment is due on October 15.

Source
Assignment Database
Assignment ID: ASSIGN-102
Last updated: 10:42 AM
Challenge 2 — Inventory AI Assistant

For:

How much A4 Xerox paper is available?

The system can use function calling:

User
 ↓
LLM
 ↓
get_stock()
 ↓
Backend API
 ↓
PostgreSQL
 ↓
Result
 ↓
LLM
 ↓
UI

The UI should present structured information:

Field	Value
Item	A4 Xerox Paper
Quantity	100
Location	Main Store
Last Updated	04 Oct 2026, 10:42 AM
Source	Inventory Database

Structured UI is easier to scan than a long generated sentence.

Database/API unavailable

The system should not invent or display an unverified quantity.

Instead:

⚠️ Inventory information unavailable

I couldn't connect to the inventory database,
so I cannot verify the current stock quantity.

[Retry]

Last successfully retrieved:
100 units at 10:20 AM

Current data and last-known data must be clearly separated.

Challenge 3 — AI Error Handling

Suppose:

AI:
"There are 500 A4 papers in Main Store."

PostgreSQL:
Quantity = 100

The application should prioritize the authoritative database.

Correct flow:

User
 ↓
LLM
 ↓
get_stock()
 ↓
PostgreSQL
 ↓
Quantity = 100
 ↓
LLM
 ↓
Final answer = 100

The system should not confidently display 500.

If a discrepancy is detected:

⚠️ Information mismatch

The AI-generated response did not match
the current inventory database.

Verified quantity:
100 units

Source:
PostgreSQL Inventory Database

[Refresh Data]
[Report Issue]

For critical values, the LLM should not invent values.

The value should come from the structured function result.

For example:

Database
   ↓
100
   ↓
Structured function result
   ↓
LLM
   ↓
"100 units"

The user-facing explanation should say:

The generated response did not match the latest inventory data.

rather than exposing technical terminology such as "hallucination."

A report mechanism can capture:

User
Timestamp
Request
Tool result
AI response
System status
Bonus Interview Answer

An AI application is a complete engineering system, not just an AI model.

The model handles tasks such as understanding language, reasoning, and generating responses, but it does not by itself provide reliable application data, authentication, business logic, security, or monitoring.

For example, in a college inventory system, the model should not decide that there are 500 A4 papers. The backend should retrieve the actual quantity from PostgreSQL, and the AI can explain the verified result to the user.

The backend handles:

Authentication
Authorization
APIs
Database access
Business rules

The frontend handles:

Presentation
User interaction
Clear information display
AI UX

Production AI systems also require monitoring to detect:

Failures
Latency
Incorrect responses
Unexpected behavior

Therefore:

AI Model + Backend + Data + UX + Security + Safety + Monitoring

The AI model is one important component, but it is not the entire product.

Architecture — College Inventory AI Assistant
                       USER
                         │
                         ▼
                  ┌───────────────┐
                  │   React UI    │
                  │  AI Chat UX   │
                  └───────┬───────┘
                          │
                          ▼
                  ┌───────────────┐
                  │   AI / LLM    │
                  └───────┬───────┘
                          │
             ┌────────────┼────────────┐
             │            │            │
             ▼            ▼            ▼
       Function       Knowledge     Response
        Calling        Sources      Generation
             │
             ▼
       ┌───────────────┐
       │  Node.js API  │
       └───────┬───────┘
               │
               ▼
       ┌───────────────┐
       │  PostgreSQL   │
       └───────┬───────┘
               │
        ┌──────┼────────┬─────────┐
        ▼      ▼        ▼         ▼
      Stock  Assets   Tickets     POs

Across the entire system:

Security
+
Error Handling
+
Monitoring
+
Human Feedback
+
Good UX
Key Takeaway

A good AI application isn't one that merely produces impressive answers. It gives users useful, understandable, verifiable, and recoverable interactions — even when the AI or an underlying system makes a mistake.