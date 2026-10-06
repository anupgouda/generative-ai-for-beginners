# Lesson 21 — Building with Meta Models

## Core Idea

This lesson focuses on two Meta model families:

- **Llama 3.1**
- **Llama 3.2**

The main engineering idea is to match the model to the workload rather than treating every Llama model as interchangeable.

For the College Inventory AI:

- **Llama 3.1** → demanding text workloads, RAG, function calling, synthetic data
- **Llama 3.2 Vision** → image + text understanding
- **Llama 3.2 1B/3B** → lightweight edge/mobile text workloads

---

# Q1. Meta Model Family

The two model families covered are:

1. **Llama 3.1**
2. **Llama 3.2**

The four variants specifically covered are:

```text
Llama 3.1 - 70B Instruct
Llama 3.1 - 405B Instruct
Llama 3.2 - 11B Vision Instruct
Llama 3.2 - 90B Vision Instruct

The purpose of comparing them is to understand that different models are appropriate for different workloads.
The lesson also mentions Llama 3.2 1B and 3B text-only variants for edge/mobile scenarios.
Q2. Llama 3.1 Improvements
The lesson highlights three major improvements over Llama 3.
1. Larger Context Window
Llama 3.1 increased the context window from:
8K → 128K tokens

A larger context window is useful for applications that need to process a large amount of information.
For example:
Multiple documents
       ↓
Large context
       ↓
Llama 3.1
       ↓
Answer

This is especially useful for RAG.
2. Higher Maximum Output
The maximum output increased from:
2,048 → 4,096 tokens

This gives the model more room to produce longer responses when required.
3. Better Multilingual Support
Llama 3.1 was trained with more tokens, improving multilingual capabilities.
This makes it more useful when applications need to handle multiple languages.
Overall Impact
These improvements help Llama 3.1 handle demanding GenAI workloads including:
- Function calling
- RAG
- Synthetic data generation
- Complex text tasks
Q3. Llama 3.1 Use Cases
1. Native Function Calling
Llama 3.1 was improved to work effectively with function/tool calling.
Example:
User
 ↓
Llama 3.1
 ↓
get_stock()
 ↓
PostgreSQL
 ↓
Result
 ↓
Llama 3.1
 ↓
Answer

For the College Inventory System:
How many A4 papers are available?

The model can determine that it needs get_stock() rather than inventing a quantity.
2. RAG
The larger context window makes Llama 3.1 useful for RAG.
Example:
User question
      ↓
Retrieve multiple policy chunks
      ↓
Llama 3.1
      ↓
Grounded answer

The model can process more retrieved information in one request.
3. Synthetic Data Generation
Llama 3.1 can generate examples that can be used to create or expand datasets.
Example:
Existing examples
       ↓
Llama 3.1
       ↓
Generate additional examples
       ↓
Review / clean
       ↓
Training dataset

For the College Inventory System, candidate examples could cover:
- Indent Master
- Asset Master
- Stock Entry
- Vendor Master
- Procurement workflows
Generated data should be reviewed and validated before being used for training.
Q4. Native Function Calling
The basic flow is:
User
 ↓
Llama 3.1
 ↓
Tool selection
 ↓
External tool/function
 ↓
Tool result
 ↓
Llama 3.1
 ↓
Final response

The model does not magically execute the tool itself.
It produces a tool-call request that an executor/application can execute.
Brave Search
The lesson gives Brave Search as a built-in tool that Llama 3.1 can recognize.
For example:
What is the weather in Stockholm?

The model can produce something conceptually like:
brave_search.call(query="Stockholm weather")

The application must then execute the search.
Wolfram Alpha
For mathematical calculations:
User
 ↓
Llama 3.1
 ↓
wolfram_alpha.call(...)
 ↓
Wolfram Alpha
 ↓
Result
 ↓
Llama 3.1

This allows the model to delegate calculations to a specialized tool.
Custom Tools
Developers can define their own tools.
For the College Inventory System:
get_stock()
get_pending_purchase_orders()
create_support_ticket()

The model can select the appropriate tool based on the user's request.
Important Limitation
The lesson's Brave Search example only demonstrates the model generating the tool call.
It does not actually execute the search and return the result.
The complete flow requires:
Llama 3.1
 ↓
brave_search.call(...)
 ↓
Brave API
 ↓
Search results
 ↓
Llama 3.1
 ↓
Answer

The core principle is:
The LLM requests the tool; the application executes it.

Q5. College Inventory Function Calling
User:
How many A4 Xerox papers are currently available?

Architecture:
                    USER
                      │
                      ▼
                  Llama 3.1
                      │
                      ▼
              Decide: get_stock()
                      │
                      ▼
                  get_stock()
                      │
                      ▼
                  Backend API
                      │
                      ▼
                  PostgreSQL
                      │
                      ▼
                 Current quantity
                      │
                      ▼
                  Llama 3.1
                      │
                      ▼
                Final response

For example:
PostgreSQL:
A4 Xerox Paper = 100 units

The model can respond:
There are currently 100 A4 Xerox paper units available.

Why Llama Should Not Be the Source of Truth
The model does not automatically know the current database state.
Therefore:
LLM
= understands request + decides action

PostgreSQL
= authoritative inventory data

This prevents hallucinated quantities.
Q6. Llama 3.2
The major capability added by Llama 3.2 is multimodality.
Llama 3.2 adds models that can understand:
Text + Images

Instead of:
Text
 ↓
Model
 ↓
Text

we can have:
Image + Text
       ↓
     Model
       ↓
    Response

For example:
Upload a photograph of a damaged desktop and ask: "What appears to be wrong with this computer?"

The model can use both the image and the question.
Why Different Sizes?
Different sizes provide different deployment options.
Larger
→ stronger capability
→ more resources

Smaller
→ lower resource requirements
→ easier local/edge deployment

The lesson specifically identifies:
- 11B and 90B Vision Instruct
- 1B and 3B text-only models
The 1B/3B variants are particularly useful for edge/mobile deployment and low-latency scenarios.
Q7. Llama 3.2 Variants
11B / 90B Vision Instruct
These are vision-capable instruction-following models.
They can work with:
Image + Text

For example:
Image:
Laptop photograph

Question:
"What appears damaged?"

The 90B version has more capacity, while the 11B version is more resource-efficient.
1B / 3B Text-Only
These are much smaller models intended for lightweight deployment.
They are useful when:
- Hardware is limited
- Low latency matters
- Edge deployment is required
- Mobile deployment is required
- The task does not require a huge model
Example:
Mobile inventory assistant
        ↓
Llama 3.2 1B/3B
        ↓
Basic text interaction

Q8. Multimodal AI
Suppose a faculty member uploads:
A photograph of a damaged desktop computer.

We could use Llama 3.2 Vision like this:
             Image
               +
          User Question
               │
               ▼
        Llama 3.2 Vision
               │
               ▼
         Image Analysis
               │
               ▼
           Application
          /           \
         ▼             ▼
    Response       Optional tool
                       call

Example response:
The image appears to show a cracked monitor/display and possible damage around the screen housing.

The application should ask for confirmation before creating a ticket.
Scenario 1 — Damaged Equipment
Photo of laptop
 ↓
Vision model
 ↓
Identify visible damage
 ↓
Maintenance ticket

Scenario 2 — Asset Label Extraction
Photo of asset label
 ↓
Vision model
 ↓
Extract:
Asset ID
Serial number
Manufacturer
Model
 ↓
Database lookup

Scenario 3 — Physical Inventory Inspection
A staff member could photograph equipment and ask:
Does this appear to be the same type of monitor recorded for this asset?

The model can analyze the visual information while the backend/database verifies the actual asset record.
Q9. Model Selection
Scenario	Model	Reason
Complex reasoning + large context	Llama 3.1 405B Instruct	Highest-capacity model covered
Native tool/function calling	Llama 3.1 70B/405B Instruct	Llama 3.1 is specifically highlighted for function/tool calling
Image + text understanding	Llama 3.2 11B/90B Vision Instruct	Vision capability
Edge/mobile low-latency assistant	Llama 3.2 1B/3B text-only	Small models designed for edge/mobile
Synthetic training data generation	Llama 3.1	Specifically identified as a use case


Important:
The best model depends on the workload.

A 405B model is not automatically the best choice for a simple inventory lookup.
Q10. Engineering Architecture — Meta-Based College Inventory AI
A strong architecture adds:
- Model router
- RAG layer
- Tool layer
- Smaller edge models
- Vision model
- PostgreSQL source of truth
                              USER
                                │
                     ┌──────────┴──────────┐
                     │                     │
                 Text Query           Asset Image
                     │                     │
                     ▼                     ▼
              Query / Model Router    Llama 3.2 Vision
                     │                     │
           ┌─────────┴─────────┐           │
           ▼                   ▼           ▼
     Simple Request       Complex Request Image Analysis
           │                   │           │
           ▼                   ▼           │
    Llama 3.2 1B/3B      Llama 3.1 70B/    │
                           405B Instruct    │
           │                   │           │
           └─────────┬─────────┘           │
                     │                     │
               ┌─────┴─────┐               │
               ▼           ▼               ▼
              RAG     Function Calling  Asset Verification
               │           │
               ▼           ▼
        College Docs    Backend API
                           │
                           ▼
                       PostgreSQL
                      Source of Truth
                           │
                           ▼
                      Final Response

Llama 3.1
Use for:
- Complex reasoning
- RAG
- Function calling
- Synthetic data generation
- Difficult text workflows
Llama 3.2 Vision
Use for:
- Asset photographs
- Equipment labels
- Visual damage
- Images requiring analysis
Llama 3.2 1B/3B
Use for:
- Edge assistants
- Mobile applications
- Simple classification
- Low-latency text interactions
RAG
Use RAG for stable/semi-changing college knowledge:
Procurement Policy
Asset Manual
Inventory SOP
Approval Procedures

Function Calling
Use functions for dynamic operations:
get_stock()
get_pending_purchase_orders()
create_support_ticket()
get_asset()

PostgreSQL
PostgreSQL remains the source of truth for:
Stock
Assets
Purchase Orders
Tickets
Vendors
Users

The model should never invent these values.
Practical Challenge 1 — Llama 3.1 vs Llama 3.2
Feature	Llama 3.1	Llama 3.2
Text generation	Excellent	Yes
Function calling	Strong focus	Possible depending on variant/use; lesson emphasizes Llama 3.1
RAG	Strong, especially due to larger context	Can also be used for text/vision applications
Multimodal image input	Not the focus	Major new capability
Edge/mobile options	Not the focus	1B/3B text-only variants
Best use case	Complex text, RAG, tools, synthetic data	Vision + text and lightweight edge scenarios


Key distinction:
Llama 3.1
→ Advanced text-oriented workloads

Llama 3.2
→ Multimodal + smaller deployment options

Practical Challenge 2 — Asset Damage Detection
User:
What appears to be wrong with this asset?

with a laptop photograph.
Step 1 — Accept Image
Faculty
 ↓
Upload laptop image

Step 2 — Send Image + Question
Image
+
"What appears to be wrong?"
        ↓
Llama 3.2 Vision

Step 3 — Analyze
The model might produce a structured result such as:
{
  "asset_type": "Laptop",
  "visible_issue": "Cracked display",
  "severity": "Medium",
  "confidence": "High"
}

The exact output should be validated rather than blindly trusted.
Step 4 — Application Response
The application can display:
The image appears to show a cracked display. Would you like to create a maintenance ticket?

Step 5 — Function Calling
If the user confirms:
User
 ↓
create_support_ticket()
 ↓
Backend
 ↓
PostgreSQL
 ↓
Ticket ID

Therefore:
Vision model
→ Understand image

Function calling
→ Perform action

PostgreSQL
→ Store authoritative ticket

Practical Challenge 3 — Inventory Assistant
User:
Check the current A4 paper stock and tell me whether we need to purchase more.

Current Stock
Use function calling:
Llama 3.1
 ↓
get_stock("A4 paper")
 ↓
Backend
 ↓
PostgreSQL
 ↓
100 units

Purchasing Requirement
If the requirement comes from a college policy/document:
Question
 ↓
RAG
 ↓
Procurement policy
 ↓
Required threshold/criteria

The model can then compare the live stock with the documented requirement.
Example:
Current stock = 100
Required threshold = 50

Complete Flow
                     USER
                       │
                       ▼
                   Llama 3.1
                       │
               ┌───────┴───────┐
               ▼               ▼
         Function Call         RAG
               │               │
               ▼               ▼
         PostgreSQL       College Policy
               │               │
               └───────┬───────┘
                       ▼
                   Llama 3.1
                       │
                       ▼
                  Final Answer

If the user asks only for the current quantity, RAG is not necessary.
Practical Challenge 4 — Model Routing
Workload:
60% simple inventory
25% complex text/RAG
10% image
5% edge/mobile

Routing strategy:
                         USER
                           │
                           ▼
                      MODEL ROUTER
                           │
        ┌───────────┬───────┼──────────┐
        ▼           ▼       ▼          ▼
      Simple      Complex  Image      Edge
        │           │       │          │
        ▼           ▼       ▼          ▼
    Small/      Llama 3.1  Llama 3.2  Llama 3.2
    lightweight  70B/405B   Vision     1B/3B

60% Simple
Use a smaller model.
Benefits:
- Lower cost
- Lower latency
- Less infrastructure
25% Complex
Use:
Llama 3.1 70B/405B Instruct
for difficult RAG and reasoning.
10% Image
Use:
Llama 3.2 Vision
because the workload requires image understanding.
5% Edge/Mobile
Use:
Llama 3.2 1B/3B
because the lesson specifically identifies these smaller text-only models for edge/mobile deployment.
Overall Benefit
Instead of:
100% → huge model

we get:
60% → small
25% → large
10% → vision
5%  → edge

This can reduce:
- Average inference cost
- Average latency
- Required infrastructure
while preserving stronger models for difficult tasks.
Practical Challenge 5 — Interview Question
Why would you choose Llama 3.2 instead of Llama 3.1?
I would not choose Llama 3.2 simply because it is newer.
I would choose it when the application's workload benefits from the capabilities that Llama 3.2 adds.
The major difference highlighted in this lesson is multimodal capability, meaning Llama 3.2 can work with image and text inputs through its Vision models.
That makes it suitable for applications such as:
- Analyzing equipment photographs
- Asset label extraction
- Visual damage analysis
Llama 3.2 also provides smaller 1B and 3B text-only models, which are useful for edge and mobile deployment where memory, compute, and latency are important.
In contrast, I would choose Llama 3.1 when the application is primarily text-based and needs:
- Strong RAG
- Function calling
- Complex reasoning
- Synthetic data generation
Therefore:
Llama 3.2 for multimodal or lightweight edge scenarios, and Llama 3.1 for demanding text, RAG, and tool-use workloads.

Lesson 21 — Final Mental Model
                     META LLAMA
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
          Llama 3.1              Llama 3.2
              │                       │
       ┌──────┼──────┐          ┌─────┴──────┐
       ▼      ▼      ▼          ▼            ▼
      RAG   Tools  Synthetic  Vision     Small models
                   Data                    1B / 3B
       │             │          │            │
       ▼             ▼          ▼            ▼
 Complex text    Training    Image + text  Edge/mobile

For the College Inventory AI:
                         USER
                           │
                           ▼
                       AI ROUTER
                           │
              ┌────────────┼─────────────┐
              ▼            ▼             ▼
         Simple text   Complex text    Image
              │            │             │
              ▼            ▼             ▼
         Llama 3.2      Llama 3.1      Llama 3.2
           1B/3B        70B/405B         Vision
              │            │             │
              └────────────┼─────────────┘
                           ▼
                     ┌─────────────┐
                     │ RAG / Tools │
                     └──────┬──────┘
                            │
                    ┌───────┴────────┐
                    ▼                ▼
              College Docs       PostgreSQL
                  RAG            Source of Truth

Key Lesson 21 Takeaway
Llama 3.1 is particularly useful for demanding text-based workloads such as RAG, function calling, and synthetic data generation, while Llama 3.2 expands the family into multimodal vision and lightweight edge/mobile models.EOF
