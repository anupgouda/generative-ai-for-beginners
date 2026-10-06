
# Lesson 17 — AI Agents

## 1. Key Idea

AI Agents allow Large Language Models (LLMs) to perform tasks by giving them access to **state** and **tools**.

A useful mental model is:

```text
AI Agent
 ├── LLM   → reasoning / decision-making
 ├── State → context of the current task
 └── Tools → actions and access to external systems
```

The agent can decide what action is needed, use a tool, observe the result, maintain relevant context, and continue toward the user's goal.

For the College Inventory System:

```text
User
 ↓
LLM / Agent
 ↓
Choose tool
 ↓
Backend validates request
 ↓
PostgreSQL / API
 ↓
Tool result
 ↓
Agent decides next step
 ↓
Final response
```

The LLM should not directly access or modify PostgreSQL. The application/backend executes the tool.

---

## 2. What Is an AI Agent?

An AI Agent is an LLM-powered system that uses state and tools to work toward a user's goal.

### LLM

The LLM acts as the reasoning and decision-making component.

### State

State contains the context the LLM is working with, including relevant conversation history, previous actions, tool results, decisions, and current task status.

### Tools

Tools give the agent capabilities beyond text generation. They can connect the agent to:

- Databases
- APIs
- External applications
- File systems
- Ticket systems
- Other LLMs or AI services

### Example

```text
User
 ↓
LLM
 ↓
Decides current stock is required
 ↓
get_stock()
 ↓
PostgreSQL
 ↓
Stock = 100
 ↓
LLM
 ↓
"A4 paper stock is 100 units."
```

---

## 3. AI Agent vs Normal Chatbot

The main difference is **action and autonomy**.

| Normal LLM Chatbot | AI Agent |
|---|---|
| Mainly generates responses | Can perform multi-step tasks |
| Often responds to individual messages | Maintains state/context |
| Limited external interaction | Can use tools/APIs |
| User often decides next step | Agent can decide the next appropriate action |
| Mainly conversational | Goal/task oriented |

### College Inventory Example

A normal chatbot might say:

> "Go to the Stock Management page."

An agent can actually perform the lookup:

```text
User
 ↓
Inventory Agent
 ↓
get_stock("A4 paper")
 ↓
PostgreSQL
 ↓
100 units
 ↓
Agent
 ↓
"A4 paper stock is currently 100 units."
```

It can then decide whether another tool is required.

---

## 4. State

State is the information an agent keeps about the current task and previous interactions.

It can include:

- Conversation history
- User request
- Previous tool calls
- Tool results
- Decisions already made
- Current task status
- Important variables
- Relevant authorization information

### Example

```text
User:
Check A4 paper stock.

Agent:
get_stock("A4 paper")
→ 35 units

User:
What about pending purchase orders?

Agent:
get_pending_purchase_orders("A4 paper")
→ No pending orders

User:
Create a ticket for it.

Agent:
Understands "it" refers to the low A4 paper stock
→ create_support_ticket()
```

Possible state:

```text
State
 ├── User = Faculty user
 ├── Item = A4 paper
 ├── Current stock = 35
 ├── Threshold = 50
 ├── Pending PO = None
 ├── Decision = Need procurement/support ticket
 └── Ticket status = Created
```

Without state, each message could be treated as a completely new request.

---

## 5. Tools

Tools are predefined functions that allow an AI Agent to interact with external systems.

The LLM decides when a tool is needed, while the application executes the tool.

### General examples

```text
get_weather()
search_web()
send_email()
book_reservation()
query_database()
```

Other examples include payment APIs, calendar APIs, file search, CRM systems, and ticket systems.

### College Inventory Tools

#### `get_stock()`

```text
get_stock("A4 paper")
```

Purpose:

- Query PostgreSQL
- Find current inventory
- Return quantity/location/details

Example:

```json
{
  "item": "A4 paper",
  "quantity": 35,
  "location": "Main Store"
}
```

#### `get_pending_purchase_orders()`

```text
get_pending_purchase_orders("A4 paper")
```

Purpose:

- Search purchase orders
- Filter pending orders
- Check whether the item is already being purchased

Example:

```json
{
  "pending_orders": 0
}
```

#### `create_support_ticket()`

```text
create_support_ticket(
    department="Administration",
    issue="A4 paper stock below threshold"
)
```

Purpose:

- Create a ticket
- Store the issue in PostgreSQL
- Return the ticket ID

Example:

```json
{
  "ticket_id": "TKT-1042",
  "status": "Created"
}
```

### Important architecture

```text
LLM
 ↓
Chooses tool
 ↓
Application validates request
 ↓
Tool executes
 ↓
Database/API
 ↓
Result
 ↓
LLM
```

---

# 6. LangChain Agents

LangChain Agents implement the agent definition using state and tools.

## AgentExecutor

`AgentExecutor` runs the agent's reasoning/action loop.

Conceptually:

```text
User request
 ↓
Agent decides
 ↓
Tool call
 ↓
Tool result
 ↓
Agent decides again
 ↓
Another tool
 ↓
Final answer
```

It coordinates the process rather than requiring every step to be manually implemented.

## State

The execution process can carry relevant context/results between steps.

```text
Stock checked → 35
 ↓
Purchase orders checked → None
 ↓
Ticket needs to be created
```

## Tools

Tools give the agent capabilities beyond text generation:

```text
LLM
 ├── get_stock()
 ├── get_purchase_orders()
 ├── create_ticket()
 └── send_email()
```

## Visibility

Agent workflows may contain many reasoning and tool steps. Visibility helps developers inspect:

```text
User request
 ↓
Agent decision
 ↓
Tool selected
 ↓
Arguments
 ↓
Tool result
 ↓
Next decision
```

This improves debugging and monitoring.

## LangSmith

LangSmith is used for observing, debugging, evaluating, and monitoring LLM/agent applications.

It can help investigate:

- Which tool was called
- Arguments passed
- Tool results
- Operation duration
- Agent workflow failures
- Why a particular response was produced

---

# 7. AutoGen

AutoGen focuses on applications where multiple AI agents can communicate and collaborate.

Instead of:

```text
One LLM → Answer
```

a system can have:

```text
Agent A
   ↕
Agent B
   ↕
Agent C
```

## Conversable Agents

Agents can start and continue conversations with other agents.

Example:

```text
Research Agent
 ↓
Writer Agent
 ↓
Reviewer Agent
```

## Customizable Agents

Agents can be configured with different:

- Roles
- Instructions
- Models
- Tools
- Human interaction
- Behaviors

For the inventory system:

```text
Inventory Agent
 → inventory expertise

Procurement Agent
 → purchasing expertise

Support Agent
 → ticketing expertise
```

## AssistantAgent

An `AssistantAgent` represents an AI assistant that can reason about a task and participate in an agent conversation.

## UserProxyAgent

A `UserProxyAgent` can represent the human/user side of the interaction and can participate in code or tool/action workflows.

Conceptually:

```text
Human/User
 ↕
UserProxyAgent
 ↕
AssistantAgent
```

### Why multiple agents?

Specialized agents can focus on different responsibilities instead of putting every responsibility into one large prompt.

---

# 8. Microsoft Agent Framework

Microsoft Agent Framework is Microsoft's open-source SDK/framework for building AI agents and multi-agent workflows.

The lesson presents it as bringing together capabilities from earlier Microsoft projects, particularly:

```text
Semantic Kernel
      +
AutoGen
      ↓
Microsoft Agent Framework
```

It supports:

- AI agents
- Tool-using agents
- Stateful conversations
- Multi-agent workflows
- Sequential workflows
- Concurrent workflows
- Observability

## State with Threads

A **thread** represents ongoing interaction context.

Example:

```text
Thread 001

User:
Check A4 stock.

Agent:
35 units.

User:
Check purchase orders.

Agent:
No pending PO.

User:
Create a ticket.

Agent:
Ticket created.
```

The thread allows the agent to maintain relevant context across those interactions.

## Tools

Tools/functions can be registered with an agent:

```text
Inventory Agent
 ├── get_stock()
 ├── get_pending_purchase_orders()
 └── create_support_ticket()
```

The framework can use function calling to select appropriate tools.

## MCP

**MCP = Model Context Protocol.**

It provides a standardized way for AI applications/agents to connect to external tools and data sources.

```text
AI Agent
 ↓
MCP
 ↓
Tools / Data / Services
```

## OpenTelemetry

OpenTelemetry provides standardized observability.

It can help collect:

- Traces
- Metrics
- Logs
- Latency
- Tool execution
- Workflow behavior

For production agents, this helps observe the path:

```text
User request
 ↓
Agent
 ↓
Tool
 ↓
Database
 ↓
Response
```

---

# 9. Multi-Agent Workflows

## Sequential Workflow

Agents execute one after another.

```text
Researcher
 ↓
Writer
 ↓
Editor
 ↓
Final document
```

The Writer depends on the Researcher's output, and the Editor depends on the Writer's output.

Use sequential execution when there are dependencies between steps.

## Concurrent Workflow

Agents work independently at the same time.

```text
             ┌→ Analyst 1
Manager ─────┼→ Analyst 2
             └→ Analyst 3
                  ↓
             Combine results
```

For example:

```text
Analyst 1 → Market
Analyst 2 → Technology
Analyst 3 → Competition
```

Then:

```text
Analyst 1 ─┐
Analyst 2 ─┼→ Aggregator → Final report
Analyst 3 ─┘
```

### Simple distinction

> **Sequential = dependent tasks performed one after another.**

> **Concurrent = independent tasks performed at the same time.**

---

# 10. Taskweaver

Taskweaver is a **code-first agent framework**.

It is useful for tasks involving data analysis and executable code, including working with Python DataFrames.

Example:

> "Analyze this inventory CSV and find departments where stock is below the threshold."

The agent could generate Python code and use a DataFrame to perform the analysis.

## Planner

The Planner is an LLM that maps a user request into tasks.

Example:

```text
User:
Find departments with low inventory.

Planner:
1. Load inventory
2. Group by department
3. Compare quantity with threshold
4. Generate report
```

## Plugins

Plugins provide additional capabilities and can be Python classes or code-interpreter capabilities.

Examples:

```text
Database plugin
Email plugin
Inventory plugin
File plugin
```

## DataFrames

DataFrames are useful for structured data analysis.

```text
Department | Item       | Quantity
-----------------------------------
CSE        | A4 Paper   | 35
AIML       | Projector  | 4
ECE        | Keyboard   | 12
```

The agent can use Python/data-analysis operations to process this information.

## Experience

Experience allows context from previous successful tasks to be stored and reused for future similar tasks.

```text
Previous successful task
 ↓
Stored experience
 ↓
Future similar task
 ↓
Better planning/execution
```

---

# 11. JARVIS

JARVIS takes a different approach by connecting an LLM with specialized AI models.

The LLM acts as an orchestrator.

```text
                User
                  ↓
                 LLM
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
  Vision Model  Speech Model  Other AI
        └─────────┼─────────┘
                  ↓
             Final response
```

Example:

```text
User:
"What objects are present in this image?"

LLM
 ↓
Vision model
 ↓
Objects detected
 ↓
LLM
 ↓
Natural-language response
```

The main idea is:

> **LLM + specialized models + orchestration**

---

# 12. College Inventory AI Agent

## User Request

> "Check the current stock of A4 paper. If the stock is below 50, check pending purchase orders. If there isn't an appropriate pending order, create a support/procurement ticket."

## Agent

The LLM should decide the next appropriate action. It should not directly modify the database.

```text
Need current stock
 ↓
Call get_stock()
 ↓
Is quantity < 50?
 ↓
YES
 ↓
Call get_pending_purchase_orders()
 ↓
Is there an appropriate pending order?
 ↓
NO
 ↓
Call create_support_ticket()
```

The LLM acts as the orchestrator.

## State

The agent can maintain:

```text
Item = A4 paper
Threshold = 50
Current stock = 35
Storage location = Main Store
Pending orders = None
Ticket required = Yes
Ticket ID = TKT-1042
```

It can also maintain:

- User identity
- Department
- Conversation history
- Tool results
- Current workflow step
- Authorization information
- Action status

## Tools

```text
get_stock(item_name)
get_pending_purchase_orders(item_name)
create_support_ticket(department, issue, priority)
```

## Complete Flow

```text
                         USER
                           │
                           ▼
                  "Check A4 paper stock"
                           │
                           ▼
                     AI AGENT / LLM
                           │
                           ▼
                      get_stock()
                           │
                           ▼
                       PostgreSQL
                           │
                           ▼
                      Quantity = 35
                           │
                           ▼
                      Agent evaluates
                         35 < 50?
                           │ YES
                           ▼
              get_pending_purchase_orders()
                           │
                           ▼
                       PostgreSQL
                           │
                           ▼
                     No pending PO
                           │
                           ▼
                  Agent decides ticket
                           │
                           ▼
                create_support_ticket()
                           │
                           ▼
                    Ticket System/DB
                           │
                           ▼
                    TKT-1042 Created
                           │
                           ▼
                    FINAL RESPONSE
```

Example final response:

> "A4 paper stock is currently 35 units, which is below the threshold of 50. There are no appropriate pending purchase orders, so I created procurement ticket TKT-1042."

---

# 13. Preventing Invented Stock Information

The LLM should never be treated as the source of truth for current inventory.

## 1. Always retrieve live stock

```text
Current stock question
 ↓
get_stock()
 ↓
PostgreSQL
```

Do not allow the LLM to answer current inventory questions from its own knowledge.

## 2. Backend validation

The backend should verify:

- User authorization
- Item ID
- Tool parameters
- Database result

## 3. Structured tool results

Example:

```json
{
  "success": true,
  "quantity": 35,
  "source": "PostgreSQL",
  "last_updated": "2026-10-05T..."
}
```

## 4. Don't fabricate missing data

If the database fails:

```text
Database unavailable
 ↓
Agent
 ↓
"I can't verify the current stock right now."
```

Not:

> "The stock is probably 35."

## 5. Keep the database authoritative

```text
PostgreSQL = Source of Truth
LLM = Reasoning / Orchestration
```

---

# 14. Single-Agent College Inventory Architecture

```text
                    USER
                      │
                      ▼
                    LLM
                  AI Agent
                      │
           ┌──────────┼──────────┐
           ▼          ▼          ▼
      get_stock()   get_PO()   create_ticket()
           │          │          │
           └──────────┼──────────┘
                      ▼
                  BACKEND API
                      │
                      ▼
                  PostgreSQL
```

### Responsibilities

**User** — provides the goal.

**LLM** — understands the request and decides which tool to use.

**State** — maintains conversation and workflow information.

**Tools** — provide access to real systems.

**Backend** — executes and validates operations.

**Database** — stores authoritative inventory information.

---

# 15. Multi-Agent College System

A larger system can use specialized agents:

```text
                         USER
                           │
                           ▼
                     MANAGER AGENT
                           │
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
    INVENTORY AGENT   PROCUREMENT AGENT  SUPPORT AGENT
           │               │               │
           ▼               ▼               ▼
       Inventory DB      PO System      Ticket System
```

## Inventory Agent

Responsibilities:

- Stock lookup
- Asset lookup
- Stock thresholds
- Asset availability
- Inventory reports

Tools:

```text
get_stock()
get_asset()
get_low_stock_items()
```

## Procurement Agent

Responsibilities:

- Purchase orders
- Pending orders
- Vendors
- Quotations
- Procurement workflow

Tools:

```text
get_pending_purchase_orders()
get_vendor()
create_purchase_request()
```

## Support Agent

Responsibilities:

- Tickets
- Asset issues
- IT support
- Escalations
- Ticket status

Tools:

```text
create_support_ticket()
get_ticket_status()
update_ticket()
```

## Manager Agent

The Manager Agent acts as the orchestrator.

Example:

```text
"A4 paper is low and there is no pending order. Create a ticket."

Manager
 ↓
Inventory Agent → Stock = 35
 ↓
Procurement Agent → No pending PO
 ↓
Support Agent → Create ticket
```

Specialization makes the system easier to maintain and extend.

---

# 16. Agent Safety

An agent connected to databases, purchase APIs, tickets, and email can create serious risks.

## Risk 1 — Hallucinated actions

The agent might claim an action succeeded when it did not.

### Mitigation

Only report success after actual tool confirmation:

```text
Tool execution successful
 ↓
PO/ticket ID returned
 ↓
Only then tell user it was created
```

## Risk 2 — Unauthorized actions

A student should not automatically be able to approve a purchase order.

### Mitigation

```text
Authentication
 +
RBAC
 +
Backend authorization
```

The LLM should never decide permissions by itself.

## Risk 3 — Wrong tool calls

The agent might call the wrong function.

### Mitigation

- Carefully defined tools
- Clear tool descriptions
- Parameter validation
- Tool allowlists
- Confirmation for sensitive operations

## Risk 4 — Excessive permissions

Giving one agent full access to everything is dangerous.

### Mitigation: least privilege

```text
Inventory Agent
 → read inventory

Procurement Agent
 → read PO + create procurement request

Support Agent
 → create/update tickets
```

## Risk 5 — Destructive operations

For destructive operations:

```text
Agent requests action
 ↓
Backend validation
 ↓
Human confirmation
 ↓
Execute
 ↓
Audit log
```

## Risk 6 — Incorrect data

Use:

- Database constraints
- Validation
- Audit logs
- Data-quality checks
- Source-of-truth rules
- Human review for important changes

## Risk 7 — Lack of auditability

Record:

```text
Who?
What?
When?
Why?
Which tool?
Which parameters?
What result?
```

Architecture:

```text
User → Agent → Tool → Backend → Database
                  ↓
               Audit Log
```

---

# 17. Business Meeting Multi-Agent System

The lesson assignment proposes an application simulating a business meeting with different departments of an education startup, where different personas ask follow-up questions to improve a product idea.

Example proposal:

> "Let's build an AI-powered student placement platform."

Architecture:

```text
                         USER
                           │
                           ▼
                       CEO AGENT
                           │
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
      ENGINEERING        FINANCE        MARKETING
         AGENT            AGENT           AGENT
           │               │               │
           └───────────────┼───────────────┘
                           ▼
                  Meeting Coordinator
                           │
                           ▼
                    Refined Product
```

## CEO Agent

Persona: strategic startup leader.

Priorities:

- Business viability
- Student value
- Growth
- Competitive advantage
- Long-term vision

Example question:

> "What problem are we solving that existing placement platforms don't solve?"

## Engineering Agent

Persona: technical architect.

Priorities:

- Architecture
- AI feasibility
- Security
- Scalability
- Development cost
- Integration

Example questions:

> "How will student data be protected?"

> "Which AI models do we need?"

> "Can the platform integrate with college ERP systems?"

## Finance Agent

Persona: financial analyst/CFO.

Priorities:

- Development cost
- Infrastructure cost
- Revenue
- ROI
- Pricing
- Financial risk

Example questions:

> "What will it cost to operate the AI features?"

> "Who is the paying customer?"

## Marketing Agent

Persona: growth and marketing strategist.

Priorities:

- Target audience
- Positioning
- Student adoption
- College partnerships
- Competitors
- Marketing channels

Example question:

> "Why would students use this instead of LinkedIn or existing placement platforms?"

## Communication

```text
User proposal
 ↓
CEO Agent
 ↓
Engineering ──┐
Finance ──────┼→ Discussion
Marketing ────┘
```

The coordinator can summarize responses and generate follow-up questions.

## Product refinement

Initial idea:

> "AI-powered student placement platform."

After discussion:

```text
AI Student Placement Platform
 ├── AI Resume Analyzer
 ├── Skill Gap Detection
 ├── Personalized Job Matching
 ├── Interview Preparation Agent
 ├── Placement Analytics
 ├── College Admin Dashboard
 └── Employer Portal
```

The final proposal can include:

- Target users
- Core features
- Technical architecture
- Business model
- Costs
- Risks
- Go-to-market strategy
- MVP scope

The value of the multi-agent system is that different agents challenge the idea from different perspectives.

---

# 18. Function Calling vs AI Agent

### Interview Answer

Function calling and AI Agents are related, but they are not exactly the same.

In a function-calling application, the LLM can request a predefined function such as `get_stock()` or `get_weather()`. The application executes the function and returns the result to the model. The workflow is usually relatively controlled.

An AI Agent goes further. It can maintain state, decide which tools it needs, use multiple tools, observe their results, and continue taking actions toward a larger goal.

For example, in the College Inventory System, a simple function-calling application could call `get_stock()` when the user asks for A4 paper stock. An agent could check the stock, determine that it is below the threshold, check pending purchase orders, and if no suitable order exists, create a support ticket.

> **Function calling gives an LLM the ability to use tools, while an AI Agent uses tools, state, and an execution loop to accomplish a multi-step goal.**

---

# 19. Final Mental Model

```text
                     AI AGENT
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
            LLM        STATE       TOOLS
             │           │           │
         Reasoning     Context      Actions
             │           │           │
             └───────────┼───────────┘
                         ▼
                    Agent Loop
                         │
                    ┌────┴────┐
                    ▼         ▼
                  Observe   Decide
                    │         │
                    └────┬────┘
                         ▼
                        Act
                         │
                       Tool
                         │
                       Result
                         │
                         └────→ Decide again
```

For the College Inventory System:

```text
              USER
                │
                ▼
        INVENTORY AI AGENT
                │
        ┌───────┼────────┐
        ▼       ▼        ▼
   PostgreSQL   RAG      APIs
   live data   policies  actions
        │       │        │
        └───────┼────────┘
                ▼
              STATE
                │
                ▼
          FINAL RESPONSE
```

### Key distinctions

```text
LLM     = brain / reasoning
State   = memory/context of the current task
Tools   = capabilities/actions
Agent   = system combining them to pursue a goal
Multi-agent system = multiple specialized agents collaborating on a larger goal
```

## Lesson 17 Takeaways

1. AI Agents give LLMs access to **state and tools**.
2. State lets an agent maintain context across a task.
3. Tools connect agents to real systems and actions.
4. LangChain Agents provide an agent execution loop and LangSmith provides visibility.
5. AutoGen focuses on conversable and customizable agents.
6. Microsoft Agent Framework brings together capabilities from Semantic Kernel and AutoGen and supports state, tools, workflows, and observability.
7. Sequential workflows are useful for dependent tasks.
8. Concurrent workflows are useful for independent tasks.
9. Taskweaver is a code-first framework suited to data and executable-code workflows.
10. JARVIS uses an LLM to orchestrate specialized AI models.
11. PostgreSQL should remain the source of truth for live inventory.
12. Agent permissions should use backend authorization and least privilege.
13. Sensitive/destructive operations should require validation and, where appropriate, human confirmation.
14. Agent actions should be observable and auditable.
15. Function calling is a capability; an AI Agent combines tools, state, and an execution loop to pursue a multi-step goal.
"""

path = Path("/mnt/data/lesson-17-notes.md")
path.write_text(notes, encoding="utf-8")
print(f"Created: {path}")
print(f"Lines: {len(notes.splitlines())}")
