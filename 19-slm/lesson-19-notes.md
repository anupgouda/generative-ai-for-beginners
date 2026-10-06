# Lesson 19 — Small Language Models (SLMs)

## Core Idea

A Small Language Model (SLM) is a language model with fewer parameters and generally lower computational requirements than a large language model (LLM).

The key engineering idea is:

> **An SLM is not automatically better than an LLM. It can be a better engineering choice when the task is focused and requirements such as privacy, offline operation, latency, cost, and limited hardware matter.**

---

# Q1. What is an SLM?

**SLM** stands for **Small Language Model**.

An SLM has fewer parameters and generally requires less:

- Compute
- Memory
- Storage
- Infrastructure

Conceptually:

```text
Large Language Model
        ↓
More parameters
More compute
More memory
Broader capabilities

Small Language Model
        ↓
Fewer parameters
Less compute
Less memory
Faster / easier deployment
SLMs can still perform:
- Question answering
- Summarization
- Classification
- Text generation
- Basic reasoning
- Instruction following
Trade-off
Smaller models can have reduced capabilities compared with larger models, especially for complex reasoning, broad knowledge, and difficult tasks.
Therefore, the important engineering question is:
Which model is capable enough for my task while meeting my resource, cost, latency, and privacy requirements?

Q2. SLM vs LLM
Factor	SLM	LLM
Model size	Smaller	Larger
Comprehension/generalization	Usually narrower	Generally broader
Computing requirements	Lower	Higher
Bias	Can still contain bias	Can also contain bias
Inference speed	Usually faster	Usually slower/more resource-intensive


Model Size
SLMs have fewer parameters, making them easier to store and deploy.
Comprehension and Generalization
Large models generally have greater capacity for:
- Diverse topics
- Complex instructions
- Unusual queries
- Multi-step reasoning
SLMs can perform extremely well on targeted tasks but may have more limitations outside their intended scope.
Computing
SLM → Lower compute
LLM → Higher compute

This is a major reason SLMs are useful for local deployment.
Bias
An SLM is not automatically unbiased.
Both SLMs and LLMs can inherit bias from:
- Training data
- Fine-tuning
- Evaluation data
- Design
Inference Speed
Because SLMs generally require less computation, they can often generate responses faster, especially on constrained hardware.
This is useful for:
- Local applications
- Edge devices
- Offline assistants
- Real-time interfaces
Q3. Applications of SLMs
1. Local Assistants
An SLM can run on a laptop or local computer.
Example:
User:
How do I create an indent?

       ↓

Local SLM

2. Edge Devices
Smaller models can operate on devices with limited compute and memory.
Device
  ↓
SLM
  ↓
Local Assistant / Classification

3. Text Classification
Examples:
- Ticket classification
- Sentiment classification
- Spam detection
- Intent detection
- Department classification
4. Summarization
An SLM can summarize:
- Support tickets
- Inventory reports
- Procurement requests
- Meeting notes
5. Domain-Specific Assistants
A smaller model can be adapted to a focused domain.
For the College Inventory AI:
College Inventory Terminology
          ↓
         SLM
          ↓
Inventory Assistant

If the tasks are well-defined, a huge model may not be necessary.
Q4. Microsoft Phi-3 / Phi-3.5 Family
The Phi family consists of Microsoft's smaller language models designed to provide strong capabilities while being more resource-efficient than many larger models.
Phi-3-mini
Designed for:
- Lightweight deployment
- General language tasks
- Local/edge scenarios
- Resource-constrained environments
Phi-3-small
A larger variant than Phi-3-mini that provides more capacity while still targeting efficient deployment.
Phi-3-mini
    ↓
Smaller / lighter

Phi-3-small
    ↓
More capacity

Phi-3-medium
A larger Phi-3 model designed for stronger capabilities when more resources are available.
Phi-3.5-mini
An updated small Phi model designed to improve capabilities while retaining the advantages of a relatively small model.
Phi-3-Vision
Adds vision capabilities for understanding visual information.
Potential applications include:
- Image question answering
- Document/image analysis
- Visual inspection
Phi-3.5-Vision
An updated vision-oriented Phi model with multimodal capabilities for visual and textual information.
Phi-3.5-MoE
Uses Mixture of Experts (MoE).
Instead of activating the entire model for every input, selected expert components can process the input.
Q5. Phi-3 Instruct vs Phi-3 Vision
The simple distinction is:
Phi-3 Instruct
       ↓
Text / Instructions / Chat

Phi-3 Vision
       ↓
Text + Visual Understanding

Phi-3 Instruct
Designed for text-based instruction following.
Example:
Explain what an Asset Master is.
        ↓
Phi-3 Instruct
        ↓
Text Response

Useful for:
- Question answering
- Summarization
- Text generation
- Instruction following
- Basic coding assistance
Phi-3 Vision
Designed for tasks involving images.
Example:
Equipment Label Photo
        ↓
Phi-3 Vision
        ↓
Visual Understanding
        ↓
Asset ID
Serial Number
Manufacturer
Model

Therefore:
Instruct = primarily text interaction

Vision = text + image understanding

Q6. Mixture of Experts (MoE)
Mixture of Experts is an architecture where a model contains multiple specialized expert components and a routing mechanism determines which experts should process a particular input.
Instead of:
Every Input
    ↓
Entire Model
    ↓
Output

an MoE model works more like:
                Input
                  ↓
                Router
                  ↓
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    Expert A   Expert B   Expert C
       ✗          ✓          ✗
                  ↓
                Output

The router activates only selected experts.
Why can this reduce compute?
Imagine a model has 8 experts but only 2 are activated for a particular token.
The model can therefore have substantial total capacity without performing the same amount of computation as a dense model that activates everything every time.
Phi-3.5-MoE
Phi-3.5-MoE uses this approach.
The key idea is:
Large overall model capacity does not necessarily mean all parameters must be actively computed for every input.

Q7. Ways to Run Phi-3 / Phi-3.5
Option	Main Idea
Microsoft Foundry Models	Managed cloud/model platform
NVIDIA NIM	Optimized model inference/deployment through NVIDIA infrastructure
Hugging Face Transformers	Python-based model loading and experimentation
Ollama	Simple local model execution
Foundry Local	Run supported AI models locally
ONNX Runtime for GenAI	Optimized generative AI inference using ONNX models


Microsoft Foundry
Useful for managed cloud-based AI model workflows.
Application
     ↓
Microsoft Foundry
     ↓
Phi Model

NVIDIA NIM
Provides optimized inference capabilities, particularly around NVIDIA hardware and GPU deployment.
Hugging Face Transformers
A popular Python ecosystem for:
- Experimentation
- Research
- Fine-tuning
- Custom Python applications
Ollama
Makes running supported models locally relatively simple.
Mac / PC
   ↓
Ollama
   ↓
Local Phi Model
   ↓
Application

Foundry Local
Provides local execution of supported AI models, useful for privacy and offline/low-connectivity scenarios.
ONNX Runtime for GenAI
Provides optimized generative AI inference using ONNX models across supported hardware environments.
Q8. Why Run an SLM Locally?
1. Privacy
Sensitive college information can remain on the college machine.
College Data
     ↓
Local SLM
     ↓
Response

This is useful for:
- Student information
- Asset records
- Procurement information
- Internal documents
2. Reduced Network Dependency
A local SLM can continue operating when internet connectivity is unavailable.
3. Cost
Local inference can reduce per-request cloud API charges for repetitive workloads.
However, local deployment still has:
- Hardware costs
- Electricity costs
- Maintenance
- Storage
- Engineering costs
4. Latency
Local inference can avoid network round trips.
Application
    ↓
Local SLM
    ↓
Response

5. Data Control
The organization controls:
- Where the model runs
- What data it sees
- What gets stored
- How logs are handled
6. Offline Operation
Internet unavailable
        ↓
    Local SLM
        ↓
      Works

7. Deployment Flexibility
Local models can potentially be deployed across:
- College computers
- Local servers
- Edge devices
- Lab machines
Q9. ONNX Runtime for GenAI
What is ONNX Runtime?
ONNX Runtime is a cross-platform runtime for executing machine-learning models represented in the ONNX format.
ONNX Model
    ↓
ONNX Runtime
    ↓
CPU / GPU / Supported Hardware

ONNX Runtime for GenAI
Generative AI requires repeated token generation.
Input
  ↓
Generate Token
  ↓
Generate Next Token
  ↓
Generate Next Token
  ↓
...

ONNX Runtime for GenAI provides functionality around:
- Tokenization
- Model inference
- Token generation
- Decoding
- Generation configuration
- Efficient inference mechanisms
Basic Inference Flow
User Text
    ↓
Tokenizer
    ↓
Input Tokens
    ↓
Generative Model
    ↓
Next-token Generation
    ↓
Output Tokens
    ↓
Decoder
    ↓
Human-readable Text

Example:
"What is an Asset Master?"
        ↓
     Tokenizer
        ↓
    Token IDs
        ↓
     Phi Model
        ↓
 Generated Token IDs
        ↓
      Decoder
        ↓
"Asset Master is..."

This is useful for local inference, hardware acceleration, performance, privacy, and cross-platform deployment.
Q10. Engineering Decision — College Inventory AI
Requirements
- Basic inventory questions
- Local college computer
- No internet requirement
- Private college data
- Limited hardware
- No extremely advanced reasoning
Decision
Choose a Small Language Model.

Why?
Privacy
Private college information does not need to leave the machine.
Offline Capability
The system can continue operating without internet.
Hardware
A sufficiently small model has a better chance of operating within available hardware constraints.
Latency
Local inference can avoid network round trips.
Cost
There can be less dependence on per-request cloud API charges.
Capability
The requirement is basic inventory assistance, so the largest possible model is unnecessary if an SLM can reliably perform the task.
Deployment to Investigate
Phi-3 / Phi-3.5
       ↓
Ollama / Foundry Local / ONNX Runtime for GenAI
       ↓
Local College Computer

The final choice should be benchmarked on the actual college hardware.
Practical Challenge 1 — LLM vs SLM
Requirement	LLM	SLM
Reasoning	Generally stronger	Usually sufficient for focused tasks
Cost	Cloud/API costs can be higher	Local inference can reduce per-request cost
Privacy	Depends on provider/deployment	Strong advantage when fully local
Latency	Network + inference latency	Can be very low locally
Hardware	Usually requires more resources	Lower requirements
Offline usage	Difficult with cloud API	Strong advantage
Domain tasks	Very capable	Can be strong after prompting/adaptation


Choice
For the given College Inventory AI requirements:
SLM

because privacy, offline capability, limited hardware, and focused tasks are more important than maximum reasoning capability.
Practical Challenge 2 — Local AI Architecture
                    USER
                      │
                      ▼
                  Local UI
               React / Desktop UI
                      │
                      ▼
                  Local Agent
                      │
               ┌──────┴──────┐
               ▼             ▼
              SLM        Local Tools
                              │
                     ┌────────┼────────┐
                     ▼        ▼        ▼
                   Stock      PO     Tickets
                     │        │        │
                     └────────┼────────┘
                              ▼
                         PostgreSQL
                              │
                              ▼
                     Natural-language
                         Response

Use the SLM for
- Understanding user questions
- Intent detection
- Natural-language responses
- Choosing appropriate tools
- Explaining database results
- Basic inventory assistance
Do Not Use the SLM as the Source of Truth
For example, don't ask the model:
"What is the current A4 stock?"

Instead:
User
 ↓
SLM
 ↓
get_stock()
 ↓
PostgreSQL
 ↓
35 units
 ↓
SLM
 ↓
"A4 stock is currently 35 units."

The SLM should also not independently decide authorization or directly execute dangerous database operations.
Practical Challenge 3 — Choose the Deployment Method
A. Quickly experiment with Phi-3 on a Mac
Answer: Ollama
Mac
 ↓
Ollama
 ↓
Phi Model

B. Deploy a Phi model through a managed cloud environment
Answer: Microsoft Foundry
C. Build a Python application using pretrained models
Answer: Hugging Face Transformers
Python
 ↓
Transformers
 ↓
Phi

D. Run a local model through a simple command-line experience
Answer: Ollama
E. Deploy an optimized model across different hardware platforms
Answer: ONNX Runtime for GenAI
NVIDIA NIM
NVIDIA NIM is particularly relevant for optimized inference and deployment around NVIDIA GPU infrastructure.
Overall Picture
Microsoft Foundry
→ Managed/cloud AI platform

NVIDIA NIM
→ Optimized NVIDIA inference/deployment

Hugging Face
→ Python/model experimentation

Ollama
→ Easy local model execution

Foundry Local
→ Local Microsoft model execution

ONNX Runtime GenAI
→ Optimized cross-platform GenAI inference

Practical Challenge 4 — Phi-3 Vision
For a photograph of an equipment label, choose a:
Vision model

because the input is an image.
Equipment Photograph
        ↓
Vision Model
        ↓
Extract Information
        ↓
Asset ID
Serial Number
Model
Manufacturer

For a production system, validate the extracted information against the college database.
Photo
 ↓
Phi Vision
 ↓
Extract Asset ID / Serial / Model
 ↓
PostgreSQL Lookup
 ↓
Verify Asset
 ↓
Final Response

Do not blindly trust the vision model's extracted information.
Practical Challenge 5 — Interview Answer
"I would choose a Small Language Model when the application has focused requirements and doesn't need the full capabilities of a very large model. SLMs generally require less memory and compute, which makes them suitable for local or edge deployment. They can also provide advantages in privacy, latency, offline operation, and potentially cost.
For example, in my College Inventory AI, if I only need to answer basic inventory questions and the system must run on a college computer without sending private data to an external API, an SLM such as a Phi model would be a good candidate. I could run it locally and connect it to PostgreSQL through controlled tools.
I wouldn't choose an SLM just because it is cheaper, though. I would benchmark it against a larger model using the actual tasks, measuring accuracy, latency, resource usage, and reliability. If the SLM meets the application's requirements, its lower resource requirements and stronger local deployment advantages could make it the better engineering choice."

Bonus — Connecting Lesson 18 + Lesson 19
A possible architecture is:
College Data
     ↓
High-quality Dataset
     ↓
Fine-Tune SLM
     ↓
Local College AI
     ↓
Function Calling
     ↓
PostgreSQL

However, do not immediately fine-tune.
A stronger engineering process is:
College Data
      ↓
High-quality Examples
      ↓
Baseline Evaluation
      ↓
┌───────────────┬───────────────┐
▼               ▼
Prompt          RAG
Engineering
└───────┬───────┘
        ↓
     Evaluate
        ↓
Still Insufficient?
        ↓
   Fine-tune SLM
        ↓
 Local College AI
        ↓
 Agent / LLM
        ↓
 ┌──────┴──────┐
 ▼             ▼
RAG      Function Calling
              ↓
          PostgreSQL

Why This Architecture Makes Sense
Fine-Tuning
Can teach stable college-specific behavior and terminology such as:
Indent Master
Asset Master
Stock Entry
Vendor Master
Material Checkout

Local SLM
Provides:
- Privacy
- Offline capability
- Lower resource requirements
- Local deployment
Function Calling
Handles dynamic information:
"What is the current A4 stock?"
             ↓
        get_stock()
             ↓
         PostgreSQL

PostgreSQL
Remains the source of truth.
SLM        = Understands / reasons
Tools      = Controlled access
PostgreSQL = Authoritative data

Important Caution
Fine-tuning an SLM does not automatically make it better.
Compare:
SLM + Prompt
      ↓
SLM + RAG
      ↓
Fine-tuned SLM

against the actual requirements.
Lesson 19 — Final Mental Model
                  SLM
                   │
          ┌────────┼────────┐
          ▼        ▼        ▼
       Smaller   Faster   Efficient
          │        │        │
          └────────┼────────┘
                   ▼
              Local / Edge AI
                   │
            ┌──────┴──────┐
            ▼             ▼
         Privacy        Offline
            │             │
            └──────┬──────┘
                   ▼
              Focused Tasks

College Inventory AI
                    USER
                      │
                      ▼
                  Local SLM
                      │
              ┌───────┼───────┐
              ▼       ▼       ▼
             RAG    Tools   Fine-Tuning
          Documents Live Data  Behavior
                      │
                      ▼
                  PostgreSQL
                      │
                      ▼
                   Response

Key Lesson 19 Takeaway
An SLM is not automatically better than an LLM. It is a smaller model that can be a better engineering choice when the task is focused and requirements such as privacy, offline operation, latency, cost, and limited hardware matter.

For the College Inventory AI:
Use the SLM for language understanding and interaction, use RAG for document knowledge, use function calling for live PostgreSQL data, and consider fine-tuning only after establishing that prompting and RAG are not sufficient.
