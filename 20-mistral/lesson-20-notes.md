# Lesson 20 — Building with Mistral Models

## Core Idea

This lesson is mainly about choosing the right Mistral model for the job rather than assuming the biggest model is always the best choice.

For the College Inventory AI:

- **Mistral Large** → complex tasks, RAG, coding, multilingual and reasoning-heavy workloads
- **Mistral Small** → high-volume, simpler, cost- and latency-sensitive workloads
- **Mistral NeMo** → customization, fine-tuning, native function calling and flexible/self-hosted use cases

The engineering decision should consider:

```text
Task
 ↓
Required quality
 ↓
Latency
 ↓
Cost
 ↓
Deployment requirements
 ↓
Choose model

Q1. Mistral Model Family
The lesson compares three Mistral models:
1. Mistral Large 2 (2407)
2. Mistral Small
3. Mistral NeMo
The goal is not simply to determine which model is "best", but to understand their different strengths and trade-offs.
Model	Main strength
Mistral Large 2	Complex reasoning, RAG, coding, multilingual tasks
Mistral Small	Lower cost and latency for simpler workloads
Mistral NeMo	Fine-tuning, function calling, flexible/self-hosted use cases


Q2. Mistral Large 2
Mistral Large 2 (2407) is an improved version of the original Mistral Large.
The lesson highlights improvements in:
1. Context Window
Mistral Large 2 has a much larger context window than the original model.
A larger context window allows the model to work with more information in a single request, which is useful for RAG systems.
Large context
      ↓
Multiple retrieved documents
      ↓
Mistral Large 2
      ↓
More informed answer

2. Math and Coding
Mistral Large 2 improves capabilities in:
- Mathematics
- Coding
- Reasoning
- Technical tasks
This makes it more suitable for complex technical applications.
3. Multilingual Capabilities
Mistral Large 2 has improved multilingual performance.
The lesson mentions languages including:
- English
- French
- German
- Spanish
- Italian
- Portuguese
- Dutch
- Russian
- Chinese
- Japanese
- Korean
- Arabic
- Hindi
This can be useful when an AI application serves users across different languages.
Q3. Mistral Large Use Cases
1. RAG
Mistral Large is useful for RAG because the model may need to:
- Understand the user's question
- Process retrieved context
- Connect information from multiple chunks
- Produce a grounded answer
User question
      ↓
Retriever
      ↓
Multiple documents
      ↓
Mistral Large
      ↓
Answer

Its larger context and reasoning capabilities are useful for complex retrieval tasks.
2. Function Calling
Function calling requires the model to understand what the user wants and which function should be called.
Example:
User:
"Check A4 stock."

      ↓

Mistral Large

      ↓

get_stock("A4 paper")

      ↓

PostgreSQL

For complex workflows involving several tools, a stronger model can be useful.
3. Code Generation
Mistral Large is suitable for:
- Generating code
- Debugging
- Code explanation
- Architecture assistance
- Complex programming tasks
Example:
Create an Express API that retrieves inventory data from PostgreSQL and exposes it through a function-calling tool.

Q4. Mistral Large + RAG
The RAG pipeline is:
                  DOCUMENT
                     │
                     ▼
                  Chunking
                     │
                     ▼
                 Embeddings
                     │
                     ▼
               FAISS Vector Store
                     │
                     │
User Question ───────┤
       │             │
       ▼             ▼
Question          Similarity
Embedding          Search
       │             │
       └──────┬──────┘
              ▼
       Retrieved Chunks
              │
              ▼
        Mistral Large
              │
              ▼
        Final Answer

Step 1 — Document
Example college documents:
Procurement_Policy.pdf
Asset_Manual.pdf
Inventory_SOP.pdf

Step 2 — Chunking
Large documents are divided into smaller pieces.
Document
   ↓
Chunk 1
Chunk 2
Chunk 3
...

Chunking makes retrieval more precise.
Step 3 — Embeddings
Each chunk is converted into a numerical vector representing its semantic meaning.
Chunk
 ↓
Embedding
 ↓
Vector

Step 4 — FAISS Vector Store
The embeddings are stored/indexed using FAISS.
FAISS enables efficient similarity searching.
Step 5 — Question Embedding
The user's question is also converted into an embedding.
Example:
What is the approval process for purchasing equipment?

Question
   ↓
Embedding

Step 6 — Similarity Search
The question vector is compared with stored document vectors.
The most relevant chunks are retrieved.
Step 7 — Retrieved Chunks
The relevant information is provided to Mistral Large as context.
Question
+
Retrieved information
        ↓
Mistral Large

Step 8 — Answer
Mistral Large generates the final response based on the retrieved context.
This helps ground the response in the college's actual documentation.
Q5. Mistral Small
Mistral Small should be considered when the task does not require the additional capabilities of Mistral Large.
Cost
Smaller models generally cost less to run.
For thousands of simple requests, using a large model for every request may be unnecessarily expensive.
Latency
Smaller models generally require less computation and can provide faster responses depending on the deployment environment.
Resource Requirements
Mistral Small requires fewer computational resources than Mistral Large.
This makes it attractive for:
- Smaller infrastructure
- High-volume workloads
- Resource-constrained deployment
Suitable Workloads
Examples:
"What is the stock of keyboards?"
"Where is the main store?"
"What is an Asset Master?"
"Show pending purchase orders."

These queries generally do not require the reasoning capabilities of the largest model.
Q6. Mistral Small vs Large
For:
50,000 simple queries/day

I would initially consider Mistral Small because the workload is:
- High volume
- Repetitive
- Relatively simple
- Cost-sensitive
- Latency-sensitive
For example:
What is the stock of monitors?

does not need the full reasoning capability of Mistral Large.
When to use Large
Large should be used when requests become more complex.
Example:
Analyze the last six months of procurement data and determine why the IT department is consistently exceeding its equipment budget.

Or:
Read these procurement policies and explain the conflict between the two approval procedures.

These tasks require more reasoning and potentially more context.
Model Routing
Instead of:
Every request
      ↓
Mistral Large

use:
                    User
                      ↓
                 Query Router
                  /         \
                 /           \
        Simple query       Complex query
              ↓                  ↓
       Mistral Small       Mistral Large
              \                  /
               \                /
                  Final answer

This is a model-routing strategy.
Q7. Mistral NeMo
Mistral NeMo is particularly interesting for customization and flexible deployment.
1. Apache 2.0 License
Mistral NeMo is released under the Apache 2.0 license.
This provides broad permissions for using, modifying and distributing the software subject to the license terms.
2. Tekken Tokenizer
NeMo uses the Tekken tokenizer.
Tokenization affects how text is converted into tokens before being processed by the model.
A more efficient tokenizer can provide practical benefits for:
- Context usage
- Multilingual text
- Token efficiency
- Processing efficiency
3. Fine-Tuning
NeMo is suitable for customization and fine-tuning.
Example:
Base Mistral NeMo
       ↓
College inventory dataset
       ↓
Fine-tuning
       ↓
College Inventory Assistant

This connects directly to Lesson 18.
4. Native Function Calling
NeMo supports function calling capabilities.
Example:
User
 ↓
Mistral NeMo
 ↓
get_stock()
 ↓
PostgreSQL
 ↓
Result
 ↓
Mistral NeMo

This makes NeMo interesting for tool-based applications.
Q8. Tokenization
Tokenization converts text into smaller units called tokens that a language model can process.
Conceptually:
"Check A4 paper stock"
          ↓
       Tokenizer
          ↓
[Check] [A4] [paper] [stock]

The actual tokens are not necessarily identical to whole words.
Why Tokenization Matters
Tokens affect:
- Context window usage
- Input/output length
- Cost
- Processing efficiency
- Representation of different languages and text types
NeMo's Tokenizer
The lesson highlights the Tekken tokenizer as an improvement over the tokenizer used by earlier Mistral models.
A more efficient tokenizer can represent certain text, particularly multilingual text, using fewer or more appropriate tokens.
More efficient tokenization
          ↓
Less context consumed
          ↓
Potentially better efficiency

The tokenizer alone does not make NeMo universally better. It is one factor in model selection.
Q9. Model Selection
A. Complex RAG + Reasoning
Choice: Mistral Large
Reasons:
- Multiple retrieved documents
- Large context
- Reasoning across information
- Complex questions
Documents
   ↓
RAG
   ↓
Mistral Large
   ↓
Complex answer

B. High-Volume Simple Queries
Choice: Mistral Small
Examples:
"What is the A4 stock?"
"Where is the main store?"
"How many keyboards are available?"

Small can potentially provide sufficient quality with lower cost and latency.
C. Fine-Tuned Self-Hosted Assistant
Choice: Mistral NeMo
Reasons:
- Fine-tuning
- Apache 2.0 licensing
- Native function calling
- Flexible deployment
Therefore, NeMo is a strong candidate for a customized/self-hosted assistant.
Q10. Engineering Decision
Requirements:
- PostgreSQL = authoritative inventory data
- Natural-language questions
- Documentation retrieval
- Function calling
- Cost efficiency
- Possible local deployment
A strong architecture is a hybrid model architecture.
                         USER
                           │
                           ▼
                     AI APPLICATION
                           │
                           ▼
                      QUERY ROUTER
                     /            \
                    /              \
                   ▼                ▼
            Simple request     Complex request
                   │                │
                   ▼                ▼
           Mistral Small      Mistral Large
                   │                │
                   └───────┬────────┘
                           ▼
                         AGENT
                           │
                 ┌─────────┼─────────┐
                 ▼         ▼         ▼
                RAG    Function Calls  Other Tools
                 │         │
                 │         ▼
                 │      Backend API
                 │         │
                 │         ▼
                 │      PostgreSQL
                 │
                 ▼
           College Documents
                           │
                           ▼
                     Final Response

Current Inventory Data
For:
How many A4 papers are available?

Use:
Mistral
   ↓
get_stock()
   ↓
Backend
   ↓
PostgreSQL

PostgreSQL is the source of truth.
Documentation
For:
What is the college procurement approval procedure?

Use:
Question
   ↓
RAG
   ↓
College policy
   ↓
Mistral
   ↓
Answer

Complex Reasoning
For:
Analyze the procurement policy and determine which approval route applies to this unusual purchase.

Route to Mistral Large.
Simple Queries
For:
Where is the stationery store?

Route to Mistral Small.
Local Fallback
If local deployment becomes important:
Cloud
 ├── Mistral Small
 └── Mistral Large

Local
 └── Mistral NeMo

NeMo can be investigated as the customizable/self-hosted option.
Practical Challenge 1 — Model Selection
Requirement	Model	Reason
Complex RAG	Mistral Large	Stronger reasoning and larger context
High-volume queries	Mistral Small	Lower cost/latency for simpler workloads
Fine-tuning	Mistral NeMo	Designed with customization in mind
Native function calling	Mistral NeMo	Supports native function calling
Local/self-hosted possibility	Mistral NeMo	Flexible deployment and Apache 2.0 licensing


These are initial engineering choices, not absolute rules. Production decisions should be validated by benchmarking the actual workload.
Practical Challenge 2 — Inventory Assistant
User:
How many A4 75gsm Xerox papers are available, and do we have enough for next month's requirement?

This contains two different types of information.
Part 1 — Current Stock
Current stock is dynamic.
Therefore:
User
 ↓
Mistral
 ↓
get_stock("A4 75gsm Xerox Paper")
 ↓
Backend
 ↓
PostgreSQL
 ↓
Current quantity

Example:
Current stock = 100

Part 2 — Next Month's Requirement
The requirement could come from:
- College procurement policy
- Department forecast
- User-provided requirement
If it is documented information:
RAG
 ↓
Relevant policy/forecast
 ↓
Mistral

If the user explicitly says:
We expect to need 150 next month.

Then:
Current stock = 100
Expected requirement = 150

100 < 150

The system can explain:
The current stock is insufficient by 50 units.

Complete Architecture
                     USER
                       │
                       ▼
                     MISTRAL
                       │
              ┌────────┴────────┐
              ▼                 ▼
         get_stock()            RAG
              │                 │
              ▼                 ▼
         PostgreSQL       College Documents
              │                 │
              └────────┬────────┘
                       ▼
                     MISTRAL
                       │
                       ▼
                 Final Response

Important
PostgreSQL → Current inventory
RAG        → Documented knowledge
Mistral    → Reasoning + response

The model should not invent current stock.
Practical Challenge 3 — Cost Optimization
Current architecture:
Every request
      ↓
Mistral Large

But 80% of requests are simple.
Introduce model routing:
                         USER
                           │
                           ▼
                      Query Router
                           │
               ┌───────────┴───────────┐
               ▼                       ▼
        Simple request          Complex request
               │                       │
               ▼                       ▼
        Mistral Small            Mistral Large
               │                       │
               └───────────┬───────────┘
                           ▼
                      Final response

Simple Queries
Examples:
"What is the stock of monitors?"
"Where is the stationery store?"
"Show pending purchase orders."

→ Mistral Small
Complex Queries
Examples:
"Analyze why procurement spending increased."

"Compare these procurement policies."

"Summarize these documents and recommend a procurement strategy."

→ Mistral Large
Benefits
Lower Cost
Most requests use the smaller model.
Lower Latency
Simple requests can receive faster responses.
Scalability
High request volumes become easier to handle economically.
Large Remains Available
The stronger model remains available for difficult tasks.
Core principle:
Use the smallest model that can reliably solve the task.

Practical Challenge 4 — Interview Question
Why shouldn't you simply use the largest model for every AI application?
The largest model isn't automatically the best engineering choice.
Larger models generally require more compute and can have higher inference costs and latency. If an application receives a large number of simple requests, using a large model for every request can make the system unnecessarily expensive and difficult to scale.
I would first classify workloads and determine the capability actually required.
For simple tasks such as retrieving inventory quantities or answering straightforward questions, a smaller model may provide sufficient quality with lower cost and latency.
A larger model can then be reserved for:
- Complex reasoning
- Difficult RAG questions
- Coding
- Multi-step workflows
I would also consider:
- Hardware
- Privacy
- Deployment requirements
- Model specialization
- Reliability
The goal isn't to choose the largest model. It is to choose the model that provides sufficient quality for each task at an acceptable cost and performance level.
For a College Inventory AI:
Mistral Small → high-volume simple queries
Mistral Large → complex analysis
Function calling → authoritative PostgreSQL data

Lesson 20 — Final Mental Model
                   MISTRAL FAMILY
                        │
           ┌────────────┼─────────────┐
           ▼            ▼             ▼
     Mistral Large  Mistral Small  Mistral NeMo
           │            │             │
           ▼            ▼             ▼
        Complex       Simple       Customize
        reasoning     high-volume  / self-host
        RAG            low latency   fine-tuning
        coding         lower cost    function calling
           │            │             │
           └────────────┼─────────────┘
                        ▼
                 MODEL SELECTION
                        │
                        ▼
              Choose based on workload

For the College Inventory AI:
                         USER
                           │
                           ▼
                     Query / Agent
                           │
                    ┌──────┴──────┐
                    ▼             ▼
             Simple query    Complex query
                    │             │
                    ▼             ▼
             Mistral Small  Mistral Large
                    │             │
                    └──────┬──────┘
                           ▼
                     ┌───────────┐
                     │    RAG    │
                     │   Docs    │
                     └─────┬─────┘
                           │
                           ▼
                    Function Calling
                           │
                           ▼
                        Backend
                           │
                           ▼
                       PostgreSQL
                    Source of Truth

If local/private deployment becomes important:
              Local / Self-hosted
                       │
                       ▼
                  Mistral NeMo
                       │
                 ┌─────┴─────┐
                 ▼           ▼
            Fine-tuning  Function Calling
                              │
                              ▼
                          PostgreSQL

Key Lesson 20 Takeaway
Don't choose a Mistral model based only on model size. Match the model to the workload: use a stronger model when reasoning and context matter, a smaller model when volume/cost/latency matter, and a flexible model such as NeMo when customization and self-hosted deployment are important.

For the College Inventory AI, the strongest architecture is not one model for everything.
A model-routing strategy + RAG for documents + function calling for live PostgreSQL data provides a more realistic production architecture.
Interview Mental Model
RAG
 ↓
Documents / policies / manuals

Function Calling
 ↓
Live data / actions / APIs

PostgreSQL
 ↓
System of record

Mistral Small
 ↓
Simple + high-volume

Mistral Large
 ↓
Complex + reasoning-heavy

Mistral NeMo
 ↓
Customization + flexible deployment

