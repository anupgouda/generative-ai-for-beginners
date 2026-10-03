# Lesson 7 — AI-Powered Chat Applications

## Q1. Traditional Chatbot vs Generative AI Chat Application

A traditional chatbot usually works with predefined rules, commands, or fixed responses. It may only understand a limited set of questions.

A Generative AI chat application uses an LLM to understand natural-language questions and generate new responses based on the conversation and available context.

For example, a traditional college chatbot might return a fixed answer when a student asks about exam dates, while a Generative AI assistant could explain the information, answer follow-up questions, and adapt its response to the student's question.

---

## Q2. Why Use Pre-built SDKs and APIs?

SDKs and APIs make it easier to integrate an LLM without building the entire AI infrastructure ourselves.

### Benefits

1. **Faster development** — Integrate an existing model instead of building one from scratch.
2. **Access to powerful models** — Use advanced LLMs through an API.
3. **Less infrastructure work** — The provider handles much of the model serving and infrastructure.
4. **Scalability** — APIs can support applications as usage grows.
5. **Easier integration** — SDKs provide programming libraries that simplify API calls.

---

## Q3. Important UX Considerations

Three important UX considerations for generative AI chat applications are:

1. **Clear communication** — Users should understand what the AI can and cannot do.
2. **Context and conversation flow** — The assistant should understand relevant previous messages.
3. **User control and correction** — Users should be able to correct misunderstandings, provide clarification, and recover from incorrect responses.

---

## Q4. Context Retention and Privacy Risk

Context retention means keeping relevant information from previous messages so the AI can use it in later parts of the conversation.

For example, if a student tells the assistant that they are working on a Python project, the assistant can remember that context for the next question.

### Privacy Risk

Conversations may contain personal, academic, or sensitive information. If this information is stored or accessed incorrectly, it could be exposed to unauthorized people.

Therefore, applications should control:

- What information is stored
- Who can access it
- How long it is retained

---

## Q5. Microsoft's System Message Framework

The four areas are:

1. **Model identity, capabilities, and limitations**
2. **Output format**
3. **Examples of intended behavior**
4. **Behavioral guardrails**

Together, these help define how the assistant should behave and what it should and should not do.

---

## Q6. Domain-Specific Language/Model

A domain-specific language or model is designed or adapted for a specific field or type of task instead of being focused on general-purpose use.

An application might use one when specialized knowledge, terminology, accuracy, or behavior is particularly important.

For example, a healthcare application may need an AI system specifically optimized for medical terminology and healthcare workflows.

---

## Q7. Fine-Tuning

Fine-tuning means taking a pretrained model and training it further using a specialized dataset.

For a chat application, fine-tuning might be considered when we need the model to consistently follow a particular style, behavior, format, or specialized task that prompting alone isn't achieving reliably.

Fine-tuning should not automatically be the first choice for adding frequently changing information; approaches such as RAG can be more appropriate for that.

---

# Q8. Metrics for Monitoring AI Chat Applications

Useful metrics include:

1. **Uptime**
2. **Response Time**
3. **Precision**
4. **Recall**
5. **F1 Score**
6. **User Satisfaction**
7. **Error Rate**
8. **Anomaly Detection**

These measure different aspects of the system, including reliability, speed, response quality, user experience, and unexpected behavior.

---

# Q9. Precision, Recall, F1 Score, and Response Time

### Precision

Precision measures how many of the results identified as positive are actually relevant or correct.

**Precision = Correct positive results / All predicted positive results**

### Recall

Recall measures how many of the relevant positive cases the system successfully identified.

**Recall = Correct positive results / All actual positive cases**

### F1 Score

F1 Score combines precision and recall into one metric using their harmonic mean.

It is useful when we want a balance between finding relevant results and avoiding incorrect positive results.

### Response Time

Response time measures how long the system takes to return a response after receiving a request.

For a chat application, lower response time generally provides a better user experience.

---

# Q10. Microsoft's Six Responsible AI Principles

## 1. Fairness

The system should provide comparable quality of service to different groups of students and avoid unfair discrimination.

**Practical consideration:** Test responses using different student backgrounds and check for unfair differences.

## 2. Reliability and Safety

The assistant should behave reliably and avoid generating unsafe or seriously incorrect responses.

**Practical consideration:** Test the system with incorrect information, harmful requests, and unexpected inputs.

## 3. Privacy and Security

Student information and conversations should be protected from unauthorized access.

**Practical consideration:** Use authentication, access controls, encryption, and secure data handling.

## 4. Inclusiveness

The application should be usable by people with different abilities, languages, and backgrounds.

**Practical consideration:** Design accessible interfaces and support different ways of interacting with the system where appropriate.

## 5. Transparency

Users should understand that they are interacting with AI and should be informed about important limitations.

**Practical consideration:** Clearly label AI-generated responses and explain when answers may need verification.

## 6. Accountability

There should be clear responsibility for how the AI system is designed, monitored, and maintained.

**Practical consideration:** Keep logs, monitor failures, provide reporting mechanisms, and establish processes for fixing problems.

---

# Challenge 1 — AI College Assistant

## 1. Target Users

The main users would be **college students**. Faculty or administrators could also use specific parts of the system if appropriate.

## 2. Main Functionality

The assistant could help students with:

- Course-related questions
- College procedures
- Project guidance
- Programming and technical questions
- General student support
- Finding relevant college information

For college-specific information, I would use trusted college documents and data rather than relying only on the LLM's pretrained knowledge.

## 3. How the LLM Is Integrated

The frontend would send the student's question to a backend API.

The backend would construct the appropriate prompt and call the LLM through an API.

For college-specific questions, the backend could use **RAG** to retrieve relevant information from approved college documents before sending the context to the LLM.

## 4. Conversation Context

The system could maintain relevant conversation history for the current session.

Instead of sending an unlimited conversation history every time, the application could keep only relevant messages or create summaries when conversations become long.

## 5. Handling Ambiguous Answers

The assistant could allow students to:

- Ask a follow-up question
- Provide clarification
- Give a thumbs-up/down or report incorrect information
- Request the answer again with more context

For important college information, the system could also provide the source used to generate the answer.

## 6. Protecting User Data

I would use:

- Authentication and authorization
- HTTPS
- Secure database access
- Environment variables/secrets management
- Access controls
- Data minimization
- Appropriate retention policies
- Separation between different students' data

---

# Challenge 2 — System Message

```text
You are an AI College Assistant designed to help college students with academic, technical, project, and college-related questions.

## 1. Identity, Capabilities, and Limitations

You are an AI assistant, not a human faculty member or administrator.

You can:
- Explain technical and academic concepts.
- Help students understand programming problems.
- Provide project guidance.
- Answer questions using trusted information provided to you.

You cannot guarantee that every answer is correct. If you do not have enough reliable information, clearly say so instead of inventing an answer.

For college-specific information, prefer the provided official sources or retrieved college documents.

## 2. Output Format

Give clear and concise answers.

For technical questions:
1. Explain the problem.
2. Explain the concept.
3. Provide an example when useful.
4. Give verification steps.

Use headings, bullet points, and code blocks when they improve readability.

## 3. Intended Behavior

Example:

Student:
"What is an API?"

Assistant:
"An API is a way for two software systems to communicate. For example, your React frontend can call a backend API to retrieve inventory data."

Student:
"My code gives an error."

Assistant:
"Please provide the relevant code and the complete error message. I will analyze the likely cause before suggesting a fix."

## 4. Behavioral Guardrails

- Do not invent college policies, dates, rules, or procedures.
- Clearly distinguish known information from assumptions.
- Do not expose private student information.
- Do not provide harmful or unsafe instructions.
- Treat users fairly and respectfully.
- Ask for clarification when a question is ambiguous.
- Encourage verification for important information.
- Never claim that code has been tested when it has not actually been tested.
Challenge 3 — Monitoring
Metric	What would I measure?	Why is it important?
Uptime	Percentage of time the assistant is available	Shows system reliability
Response Time	Time from user request to generated response	Measures user experience and performance
Precision	Percentage of identified relevant answers/results that are actually correct or relevant	Helps measure correctness of positive responses
Recall	Percentage of relevant cases that the system successfully identifies	Helps determine whether important information is being missed
F1 Score	Balance between precision and recall	Useful when both false positives and false negatives matter
User Satisfaction	Ratings, feedback, and successful interactions	Shows whether students find the assistant useful
Error Rate	Frequency of failed requests or incorrect system responses	Helps identify reliability problems
Anomaly Detection	Unusual spikes in errors, latency, usage, or unexpected behavior	Helps detect problems that may require investigation
Challenge 4 — Engineering-Level Design
Student
   ↓
Chat UI
   ↓
Backend API
   ↓
Authentication
   ↓
Conversation / Context Management
   ↓
LLM / RAG
   ↓
Response Validation
   ↓
Safety / Responsible AI Checks
   ↓
Final Response
   ↓
Monitoring & Metrics
1. Layer Responsibilities
Student

The student asks a question or provides information.

Chat UI

Provides the interface for sending messages, displaying responses, showing sources, and reporting incorrect answers.

Backend API

Receives requests from the frontend and manages the application's business logic.

It should communicate with the LLM rather than exposing sensitive credentials to the browser.

Authentication

Verifies the student's identity and controls what information they are allowed to access.

Conversation / Context Management

Stores or manages relevant conversation history so the assistant can understand follow-up questions.

LLM / RAG

The LLM generates the response.

RAG can retrieve relevant information from trusted college documents or other approved knowledge sources before generation.

Response Validation

Checks whether the response follows the expected structure and, where possible, verifies important information.

Safety / Responsible AI Checks

Checks for harmful content, privacy problems, unsupported claims, and other safety concerns.

Final Response

The validated response is returned to the student.

Monitoring & Metrics

The application records appropriate operational and quality metrics such as uptime, response time, errors, user feedback, and evaluation results.

2. API Key Storage

The API key should be stored on the backend, not inside frontend code.

For local development:

.env
OPENAI_API_KEY=your_secret_key

The .env file should be included in .gitignore.

In production, I would use the hosting platform's secure secret-management or environment-variable system.

3. Conversation Context Management

I would store only the context that is necessary for the conversation.

For longer conversations, I could summarize older messages and keep the most relevant recent context.

Sensitive information should not be retained unnecessarily.

Each user's conversation should also be associated with the correct authenticated account so that one student cannot access another student's history.

4. Handling Ambiguous AI Responses

If the system detects that the question is unclear or lacks enough information, it should ask a clarification question instead of guessing.

Example:

"Are you asking about the college's attendance policy or how to calculate your attendance percentage?"

For important college procedures, I would also provide the relevant source so the student can verify the information.

5. Quality Monitoring

I would combine automated metrics with human/user feedback.

I would monitor:

Uptime
Response time
Error rate
Precision
Recall
F1 score where applicable
User satisfaction
Hallucination/grounding evaluations
Safety incidents
Unusual system behavior

I would also maintain a test dataset of common college questions and regularly evaluate the assistant against it.

6. Applying the Six Responsible AI Principles

Fairness: Test whether different groups of students receive comparable quality of assistance.

Reliability and Safety: Evaluate answers, detect failures, and add safeguards against unsafe outputs.

Privacy and Security: Protect student conversations and personal information using authentication, authorization, encryption, and secure storage.

Inclusiveness: Make the interface accessible and consider students with different abilities and backgrounds.

Transparency: Tell students that they are interacting with an AI system, communicate limitations, and provide sources where appropriate.

Accountability: Maintain monitoring, logging, feedback, and incident-handling processes so problems can be identified and corrected.

Lesson 7 Takeaway

An AI chat application is not just "connect a chatbot to an LLM."

A production-quality system needs:

Good UX
Context management
Authentication
Secure API integration
Grounding/RAG
Response validation
Monitoring
Responsible AI safeguards