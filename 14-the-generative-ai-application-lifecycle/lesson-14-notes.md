# Lesson 14 — The Generative AI Application Lifecycle

## 1. Generative AI Application Lifecycle

The Generative AI Application Lifecycle is the complete process of taking a GenAI idea from an initial concept to a production application and continuously improving it.

The lifecycle is:

Ideate / Explore
↓
Build / Augment
↓
Operationalize
↓
Monitor
↓
Evaluate
↓
Improve
↺

A GenAI application is not something we build once and forget.

Continuous monitoring, evaluation, and improvement are necessary because:

- Models may change.
- New data may be added.
- Prompts may change.
- User behavior may change.
- APIs may become slower.
- Costs may increase.
- New security risks may appear.
- Application accuracy may decrease.

Production is not the end of the lifecycle; it is the beginning of continuous observation and improvement.

---

## 2. MLOps vs LLMOps

MLOps focuses on managing traditional machine-learning systems throughout their lifecycle.

LLMOps extends this approach to applications built around large language models.

Traditional ML generally focuses on:

Dataset
↓
Training
↓
Model
↓
Evaluation
↓
Deployment
↓
Monitoring

LLM applications can involve:

- LLMs
- Prompts
- RAG
- Tools
- Agents
- External APIs
- User conversations
- Safety
- Evaluation

An LLM application is not necessarily a single trained model.

For example, my College Inventory AI can work as:

User
↓
Prompt
↓
LLM
↓
Function Calling
↓
Node.js API
↓
PostgreSQL
↓
LLM
↓
Answer

Changing the prompt, retrieval method, tool definitions, model, or database grounding can change application behavior.

Therefore, LLMOps manages the entire AI application, not just the model.

---

## 3. Five Key LLMOps Metrics

### 3.1 Quality

Quality measures whether the AI provides useful and correct results.

For the Inventory AI, quality can be measured using:

- Correctness
- Relevance
- Task completion
- User feedback
- Tool-call accuracy

Example:

User: "How much A4 paper is available?"

Database: 100 units

AI: "There are 100 units available."

This is a high-quality response.

### 3.2 Harm

Harm measures unsafe, inappropriate, biased, or otherwise harmful behavior.

Examples:

- Revealing private information
- Performing unauthorized actions
- Giving dangerous instructions
- Exposing credentials
- Discriminating against users

The AI should refuse requests for unauthorized private information.

### 3.3 Honesty

Honesty measures whether the AI accurately represents information and avoids pretending to know information it cannot verify.

If the database is unavailable, the AI should say:

"I couldn't access the inventory database, so I can't verify the current stock."

Honesty is closely related to:

- Grounding
- Hallucination reduction
- Source attribution
- Uncertainty handling

### 3.4 Cost

Cost measures how expensive the application is to operate.

Factors include:

- Number of API calls
- Input tokens
- Output tokens
- Model selection
- Embedding operations
- Retrieval
- Tool calls
- Infrastructure costs

### 3.5 Latency

Latency measures how long users wait for a response.

Useful measurements include:

- Average response time
- P95 response time
- Tool/API latency
- LLM latency

For an AI application involving LLM → function call → database → LLM, excessive latency can negatively affect user experience.

---

## 4. The LLM Lifecycle

The LLM lifecycle describes the stages involved in developing, improving, deploying, and maintaining an LLM-based application.

It is iterative and integrated rather than linear.

A typical cycle is:

Explore
↓
Build
↓
Deploy
↓
Monitor
↓
Discover problem
↓
Improve
↓
Test again
↺

For example, production monitoring may reveal that users frequently ask questions that a RAG system cannot answer.

This sends the application back into the Building/Augmenting stage.

---

# 5. Three Major Lifecycle Stages

## 5.1 Ideating / Exploring

The main question is:

"What problem are we solving, and can GenAI solve it effectively?"

Activities include:

- Understanding business requirements
- Building prototypes
- Experimenting with prompts
- Testing hypotheses
- Exploring models
- Testing workflows

For my Inventory AI, I could prototype:

1. AI chatbot
2. Stock lookup using function calling
3. Purchase-order lookup

A measurable hypothesis could be:

"If the Inventory AI uses function calling to retrieve live PostgreSQL data, it will answer at least 95% of predefined stock questions correctly without inventing quantities."

---

## 5.2 Building / Augmenting

Once the prototype proves useful, the application is improved.

Techniques include:

- RAG
- Fine-tuning
- Larger/better datasets
- Tools
- Evaluation
- Better prompts
- Better data pipelines
- Improved application flow

For dynamic inventory data, function calling is appropriate:

User
↓
LLM
↓
get_stock()
↓
PostgreSQL
↓
Result
↓
LLM

RAG is more appropriate for relatively stable documents such as:

- College inventory policies
- Procurement guidelines
- Asset management procedures

Dynamic information such as:

- Current stock
- Current PO status
- Current ticket status

should use database/API access.

Fine-tuning is not automatically the solution to every accuracy problem.

Poorly grounded data should first be addressed through better grounding or function calling.

---

## 5.3 Operationalizing

This is when the AI application moves into production.

Important activities include:

- Deployment
- Monitoring
- Alerts
- Security
- Performance tracking
- Application integration
- Incident handling
- Continuous evaluation

Example architecture:

React
↓
Production Backend
↓
Production AI Service
↓
Production PostgreSQL

Monitoring should include:

- Quality
- Harm
- Honesty
- Cost
- Latency
- API failures
- Database failures
- Tool-call errors
- Token usage
- User feedback

Example alerts:

P95 latency > 5 seconds
→ Alert

Database unavailable
→ Alert

Evaluation score below threshold
→ Alert

---

# 6. Management Across the Lifecycle

Security, compliance, and governance should exist throughout the lifecycle.

## Security

Examples:

- Authentication
- Authorization
- Prompt injection defenses
- Data protection
- Secure APIs
- Secret management

## Compliance

Depending on the application:

- What data can be processed
- Where data can be stored
- How long data can be retained
- Who can access data

## Governance

Governance establishes rules around:

- AI usage
- Data access
- Model selection
- Evaluation
- Human oversight
- Monitoring
- Accountability

Example:

The AI may read inventory information but cannot independently approve a ₹10 lakh purchase order.

---

# 7. Lifecycle Tooling

## Azure AI Platform

The Azure AI ecosystem provides capabilities for:

- AI model access
- AI application development
- Evaluation
- Deployment
- Monitoring
- Enterprise integration

## Microsoft Foundry

Microsoft Foundry is Microsoft's environment for building and managing AI applications and agents.

It brings together capabilities such as:

- Models
- Agents
- Tools
- Evaluation
- Monitoring
- Application development

## PromptFlow

PromptFlow focuses on developing and evaluating LLM workflows.

Example:

User Input
↓
Prompt
↓
LLM
↓
Tool
↓
Result
↓
Evaluation

It helps developers experiment with and evaluate LLM flows systematically.

---

# 8. Applying the Lifecycle to College Inventory AI

## Stage 1 — Ideating / Exploring

### Prototype 1 — AI Chatbot

User
↓
LLM
↓
Answer

Goal:

Determine whether the AI can understand common inventory questions.

### Prototype 2 — Stock Lookup

User
↓
LLM
↓
get_stock()
↓
PostgreSQL
↓
Result

Goal:

Determine whether the AI can reliably retrieve real-time stock information.

### Prototype 3 — Purchase Order Lookup

User
↓
LLM
↓
get_pending_purchase_orders()
↓
PostgreSQL

Goal:

Determine whether the AI can retrieve business data accurately.

---

# 9. Building / Augmenting Inventory AI

If the prototype gives incorrect inventory answers:

### 1. Improve Function Calling

Dynamic data should come directly from PostgreSQL.

Examples:

- Stock
- Purchase Orders
- Tickets
- Assets
- Vendors

### 2. Improve Prompts

Example instruction:

"Never invent inventory quantities. Use the inventory tool whenever current stock information is requested."

### 3. Add RAG Where Appropriate

Use RAG for:

- Inventory policies
- Procurement guidelines
- Asset management procedures

Use database/function calling for:

- Current stock
- Current PO status
- Current ticket status

### 4. Add Evaluation

Test:

- Stock
- Assets
- Purchase Orders
- Tickets
- Vendors

Measure:

- Correctness
- Tool selection
- Hallucinations
- Safety

### 5. Improve Data Grounding

Maintain a clear distinction:

Static knowledge → RAG/documents
Dynamic data → Database/API
Actions → Tools/functions

---

# 10. Operationalizing Inventory AI

Important production metrics:

### Accuracy

Inventory answer accuracy < 95%
→ Alert

### Latency

P95 response time > 5 seconds
→ Alert

### API/Database Failures

Database/API failure rate > threshold
→ Alert

### Hallucination / Grounding Failures

Monitor cases where AI provides information that cannot be supported by the database or approved knowledge sources.

### Cost

Monitor:

- Tokens
- API calls
- Model usage
- Cost per conversation

### Security

Monitor:

- Prompt injection attempts
- Unauthorized access
- Suspicious tool calls
- Repeated failed authentication

---

# 11. LLMOps Metrics for Inventory AI

| Metric | Measurement |
|---|---|
| Quality | Percentage of inventory questions answered correctly |
| Harm | Unsafe/unauthorized responses or actions |
| Honesty | Responses grounded in verified data |
| Cost | Cost per conversation/API request |
| Latency | Average and P95 response time |

### Quality

Correct answers / Total evaluated questions

Example:

960 / 1000 = 96%

### Harm

Track:

- Unauthorized data exposures
- Unauthorized tool calls
- Unsafe responses
- Security-policy violations

Goal: As close to zero as possible.

### Honesty

The AI should say "I don't know / can't verify" when reliable information is unavailable.

### Cost

Total AI cost / Number of conversations

### Latency

Track:

- Average response time
- P95 response time

---

# 12. Handling Outdated Stock Information

Problem:

"The AI is giving outdated stock information."

Operationalizing should detect the problem through monitoring and evaluation.

The issue then sends the system back to Building/Augmenting for the fix.

Flow:

Operationalizing
↓
Detect problem
↓
Building / Augmenting
↓
Fix
↓
Operationalizing

This demonstrates why the lifecycle is iterative.

## Detection

Compare AI responses against the authoritative inventory database.

Example:

AI:
A4 Paper = 500

PostgreSQL:
A4 Paper = 100

This is a grounding/accuracy failure.

Monitor:

- Database-grounded accuracy
- Tool-call success
- Tool-call correctness
- Data freshness
- Last-updated timestamp
- Hallucination rate

## Possible fixes

### Problem A — AI relies on old context

Force current stock questions to use `get_stock()`.

### Problem B — RAG index contains old stock data

Do not use static RAG data for rapidly changing inventory quantities.

Use the database/API.

### Problem C — Database synchronization is delayed

Improve synchronization and expose data freshness.

### Problem D — Model isn't selecting the tool

Improve:

- Tool description
- Prompt
- Tool schema
- Evaluation
- Tool-selection logic

## Verification

Create a regression test set.

Example:

100 stock questions

Run them before and after the fix.

Before:

Accuracy = 82%

After:

Accuracy = 98%

Also test:

- Database available
- Database unavailable
- Updated quantity
- Unknown item
- Multiple items
- Unauthorized user

---

# 13. Interview Answer

### Why isn't building an AI application a one-time task?

Because the behavior of an AI application depends on much more than the initial model.

Models can change, data changes, prompts evolve, users discover new use cases, and external tools and APIs can change.

For example, my College Inventory AI might initially work correctly, but later the inventory database structure could change or the application could receive new types of questions.

If I don't continuously evaluate it, I might not notice that accuracy or latency has degraded.

Therefore, I would treat the AI application as an iterative lifecycle. I would monitor quality, harm, honesty, cost and latency in production, collect failures, and use those findings to improve prompts, RAG, function calling, data grounding or potentially the model itself.

Security must also evolve because new attack patterns such as prompt injection or tool abuse can appear.

Building the first version is only one stage. A production AI system requires continuous evaluation, monitoring and improvement.

---

# 14. Complete Lifecycle Architecture

                    💡 IDEATE / EXPLORE
                           │
            ┌──────────────┼──────────────┐
            │              │              │
        Business       Prototype      Hypothesis
          Need              │           Testing
                           │
                           ▼
                    🛠 BUILD / AUGMENT
                           │
            ┌──────────────┼──────────────┐
            │              │              │
           RAG       Function Calling   Evaluation
            │              │              │
            └──────────────┼──────────────┘
                           │
                           ▼
                    🚀 OPERATIONALIZE
                           │
            ┌──────────────┼──────────────┐
            │              │              │
        Deployment     Monitoring       Alerts
            │              │              │
            └──────────────┼──────────────┘
                           │
                           ▼
                       📊 EVALUATE
                           │
                           ▼
                        🔧 IMPROVE
                           │
                           └──────────────↺

Across every stage:

🔐 Security
⚖️ Governance
📋 Compliance
📊 Evaluation

---

## One-line Interview Summary

**LLMOps is the continuous engineering discipline of building, evaluating, deploying, monitoring, securing, and improving LLM-powered applications across their entire lifecycle—not simply deploying an LLM once.**