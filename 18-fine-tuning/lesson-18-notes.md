# Lesson 18 — Fine-Tuning

## Core Idea

Fine-tuning means taking an already pretrained model and training it further on a smaller, task-specific dataset so that it becomes better at a particular task, style, domain, or behavior.

The most important engineering lesson is knowing **when not to fine-tune**.

The goal is to choose the simplest technique that solves the actual problem.

---

# Q1. What is Fine-Tuning?

Fine-tuning takes a pretrained model and performs additional training using task-specific examples.

```text
Large Pretrained Model
        ↓
Task-specific Training Data
        ↓
Additional Training
        ↓
Fine-tuned Model

The model already has general language knowledge. Fine-tuning adjusts its parameters so that it behaves more consistently for the target task.
Fine-tuning can improve:
- Domain-specific understanding
- Response style
- Task performance
- Instruction following
- Output consistency
- Specialized behavior
Example
Input:
What is an Asset Master?

Output:
Asset Master is the module used to maintain information
about college assets such as asset ID, category, serial number,
department, location, and status.

Q2. Fine-Tuning vs Prompt Engineering vs RAG
Prompt Engineering
Prompt engineering does not change the model.
It improves the instructions and context given to the model.
Model + Better Prompt
        ↓
Better Response

RAG
RAG also does not change the model.
Instead, it retrieves relevant external information and provides it to the model at query time.
User Query
    ↓
Retrieve Documents
    ↓
Relevant Information
    ↓
LLM

Fine-Tuning
Fine-tuning actually trains the model further.
Pretrained Model
       ↓
Training Examples
       ↓
Updated Model

Simple Distinction
Prompt Engineering changes the instructions.

RAG changes the information available to the model at query time.

Fine-Tuning changes the model's learned behavior.

Q3. Why Can Few-Shot Prompting Have Limitations?
Few-shot prompting provides several examples directly inside the prompt.
Example 1 → Question / Answer
Example 2 → Question / Answer
Example 3 → Question / Answer

Now answer this:
...

This can work well, but two major limitations are:
1. Token Limits
Every example consumes tokens.
Prompt
 ├── Example 1
 ├── Example 2
 ├── Example 3
 ├── ...
 ├── Example 100
 └── User Question

A very large prompt can approach the model's context/token limit and leave less space for the actual conversation or retrieved information.
2. Token Costs
Larger prompts generally mean more input tokens.
If the same examples are repeatedly sent across thousands of API requests, the cost can become significant.
How Fine-Tuning Can Help
Instead of repeatedly sending the same examples:
Every Request
     ↓
Huge Prompt
     ↓
100 Examples
     ↓
Question

the examples can be incorporated into the model through training:
Training Examples
      ↓
Fine-tuned Model
      ↓
Shorter Prompt
      ↓
User Question

However, fine-tuning itself introduces training and hosting costs.
Q4. When Should You Consider Fine-Tuning?
Do not fine-tune simply because you have a custom dataset.
First identify the actual problem.
Example: Missing Knowledge
"The model doesn't know our current procurement policy."

This is primarily a RAG problem because the information is external and may change.
Example: Behavioral Problem
"The model understands the information but consistently fails to follow our required output behavior."

This could potentially be a fine-tuning problem.
Recommended Engineering Process
Baseline
   ↓
Prompt Engineering
   ↓
RAG
   ↓
Evaluate
   ↓
Fine-Tuning?

Before fine-tuning, consider:
- Can better prompts solve the problem?
- Can RAG provide the missing information?
- Is the behavior stable enough to train?
- Is the expected improvement worth the cost?
- Do we have high-quality training data?
Potential costs include:
- Training compute
- Dataset preparation
- Engineering effort
- Evaluation
- Hosting
- Maintenance
- Future retraining
Q5. Fine-Tuning vs Prompt Engineering vs RAG for College Inventory AI
Technique	What Changes?	Best Use
Prompt Engineering	Instructions/context	Improving behavior without training
RAG	Information provided at runtime	External, private, current or changing knowledge
Fine-Tuning	Model parameters/learned behavior	Consistent specialized behavior or task performance


Prompt Engineering
If the AI needs to consistently answer:
Item:
Quantity:
Location:
Status:

a good system prompt may be enough.
RAG
For documents such as:
Procurement Policy.pdf
Asset Management Manual.pdf
College Inventory SOP.pdf

RAG is appropriate.
Fine-Tuning
If the model repeatedly misunderstands college terminology and workflows even after good prompting and relevant RAG context, fine-tuning can be evaluated as a possible solution.
Examples:
"Create an indent"
        ↓
Correctly understand the college workflow

"Check Asset Master"
        ↓
Understand the expected operation

"Perform Material Checkout"
        ↓
Produce the required structured response

Q6. What Are the Costs of Fine-Tuning?
1. Tunability
Fine-tuning changes model behavior.
A model optimized for one behavior may not automatically be ideal for another.
Therefore, consider:
How much do I actually want to customize the model?

2. Engineering Effort
Fine-tuning requires:
Collect Data
    ↓
Clean Data
    ↓
Format Data
    ↓
Validate Data
    ↓
Train
    ↓
Evaluate
    ↓
Iterate
    ↓
Deploy

3. Compute
Training may require:
- GPUs
- Cloud compute
- Training time
- Storage
4. Data Quality
Good fine-tuning requires good training examples.
Bad data can teach bad behavior.
10,000 examples
30% incorrect

may be worse than:
1,000 examples
95% high quality

Important Engineering Principle
Having a dataset does not automatically justify fine-tuning.
The data might be better used with:
- RAG
- Prompt engineering
- Traditional software logic
- Database queries
- Function calling
For the College Inventory AI, current stock should remain in PostgreSQL rather than being taught through fine-tuning.
Q7. How Do You Know Fine-Tuning Actually Helped?
You need a baseline.
Do not simply say:
"The fine-tuned model feels better."

Instead, compare measurable results.
                    Evaluation Dataset
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
      Base + Prompt    Base + RAG    Fine-tuned
             │             │             │
             └─────────────┼─────────────┘
                           ▼
                    Compare Results

Important Metrics
Metric	What It Checks
Accuracy	Is the answer correct?
Quality	Is it useful?
Groundedness	Is it supported by the source?
Cost	How expensive is it?
Latency	How quickly does it respond?
Task Success	Does it complete the task?
Consistency	Does it behave reliably?


At minimum compare:
Base Model + Prompt Engineering
              vs
Base Model + RAG
              vs
Fine-Tuned Model

Also consider:
- Task success
- Domain performance
- Instruction following
- Token cost
- Extensibility
Q8. What Do You Need to Fine-Tune a Model?
Four important components are:
1. Pretrained Model
2. Training Data
3. Training/Fine-Tuning Method
4. Compute/Infrastructure

1. Pretrained Model
A base model is required.
Base LLM
   ↓
Fine-Tuning

2. Training Data
The data contains examples showing the desired behavior.
Example:
{
  "messages": [
    {
      "role": "user",
      "content": "What is an Asset Master?"
    },
    {
      "role": "assistant",
      "content": "Asset Master stores information about college assets..."
    }
  ]
}

3. Training Method
Possible techniques include:
SFT
DPO
RFT

4. Compute/Infrastructure
Training may require:
- Cloud GPUs
- Managed training infrastructure
- Storage
- Evaluation infrastructure
Q9. Microsoft Foundry, LoRA, SFT, DPO and RFT
Microsoft Foundry
Microsoft Foundry provides AI model capabilities including evaluation and fine-tuning of supported models.
It provides managed capabilities so developers do not have to build the entire training infrastructure themselves.
LoRA
LoRA = Low-Rank Adaptation
Instead of updating all parameters of a large model, LoRA adds smaller trainable components/adapters.
Original Model
      │
      ├── Mostly Frozen
      │
      └── Small Trainable Adapters
                    ↓
             Fine-Tuned Behavior

This can make fine-tuning more efficient in terms of trainable parameters and compute.
SFT — Supervised Fine-Tuning
SFT uses:
Input → Desired Output

It is the natural starting point for many customization tasks.
DPO — Direct Preference Optimization
DPO uses preference information.
Instead of only providing the correct answer, we can provide:
Response A → Preferred
Response B → Less Preferred

The model is trained toward preferred responses.
RFT — Reinforcement Fine-Tuning
RFT uses a way to evaluate or reward output quality.
Model Generates Answer
        ↓
Evaluate Answer
        ↓
Reward / Feedback
        ↓
Training
        ↓
Improved Behavior

RFT is appropriate when a reliable evaluation/reward signal can determine whether the answer is good.
Which Should You Start With?
The lesson recommends starting with SFT because it is relatively straightforward:
Good Examples
     ↓
SFT
     ↓
Evaluate

Q10. Fine-Tuning Best Practices
1. Establish a Baseline First
Before fine-tuning:
Base Model
    +
Prompt Engineering
    +
RAG if Appropriate

Measure performance first.
2. Start Small
Start with approximately:
50–100 high-quality examples

Then increase the dataset as needed, potentially toward:
500+ examples for production

3. Prioritize Data Quality
Bad examples can teach bad behavior.
Garbage Data
     ↓
Fine-Tuning
     ↓
Garbage Behavior

4. Use JSONL
Training datasets commonly use JSON Lines (JSONL).
Each line represents one training example.
{"messages":[{"role":"user","content":"What is Asset Master?"},{"role":"assistant","content":"Asset Master stores information about college assets..."}]}

5. Keep Validation Data Separate
Do not evaluate only on training examples.
Training Data
     ↓
Model Learns

Validation Data
     ↓
Model Evaluated

6. Keep the System Prompt Consistent
Keep the system prompt and intended behavior consistent between training and evaluation where appropriate.
7. Evaluate Checkpoints
Training
   ↓
Checkpoint 1 → Evaluate
   ↓
Checkpoint 2 → Evaluate
   ↓
Checkpoint 3 → Evaluate

Choose based on validation performance rather than blindly selecting the final checkpoint.
8. Watch Token Costs
Larger datasets and longer examples increase training costs.
More data is not automatically better.
9. Consider Continuous Fine-Tuning Carefully
Application behavior can change over time.
However, continuous fine-tuning should be controlled rather than blindly adding every new example.
10. Consider Hosting Costs
Total cost can include:
Training
   +
Hosting
   +
Inference
   +
Storage
   +
Monitoring
   +
Maintenance

A cheaper RAG solution may make more business sense if it provides nearly the same quality.
Practical Challenge 1 — College Inventory Training Examples
These examples teach domain understanding and behavior.
They are not a replacement for live database queries.
1. Indent Master
Input: What is an Indent Master?
Output: Indent Master is the module used to create and manage requests for materials or items required by a department.
2. Purchase Order
Input: What is a Purchase Order?
Output: A Purchase Order is an official document sent to a vendor to request approved goods or services under agreed terms.
3. Asset Master
Input: What is an Asset Master?
Output: Asset Master maintains information about college assets such as asset ID, serial number, category, department, location, and status.
4. Stock Entry
Input: What is Stock Entry?
Output: Stock Entry records materials received into the college inventory, including item details, quantity, invoice information, and storage location.
5. Vendor Master
Input: What is Vendor Master?
Output: Vendor Master stores information about registered suppliers, such as vendor identity, contact details, and procurement-related information.
6. Material Checkout
Input: What is Material Checkout?
Output: Material Checkout records when an inventory item or material is issued to a person or department and tracks the checkout details.
7. HOD Approval
Input: What is HOD Approval?
Output: HOD Approval is the step where the Head of Department reviews and approves or rejects a department's request.
8. Bill Master
Input: What is Bill Master?
Output: Bill Master manages bill records associated with purchases, including vendor, invoice, amount, and related procurement information.
9. Bill Payment
Input: What is Bill Payment?
Output: Bill Payment records and manages payments made against approved vendor bills.
10. Transport Master
Input: What is Transport Master?
Output: Transport Master maintains information related to transportation used for moving materials or assets, such as vehicle and transport details.
Important Distinction
Do not train dynamic information such as:
A4 paper quantity = 100

and expect the model to know the current quantity.
Current inventory should come from:
LLM
 ↓
get_stock()
 ↓
PostgreSQL

PostgreSQL remains the source of truth.
Practical Challenge 2 — Fine-Tuning Decision
Given:
Base + Prompt = 82%
RAG            = 91%
Fine-Tuned     = 93%

RAG = ₹2 / 1M tokens

Fine-Tuning + Hosting
= ₹15 / 1M equivalent usage

I would not immediately choose fine-tuning.
The improvement is:
93% - 91% = 2 percentage points

The remaining questions are:
1. Is the 2% improvement important?
2. What types of errors remain?
3. What is the total cost?
4. Does fine-tuning improve consistency, latency, prompt size, or domain behavior?
The decision should consider:
Training
+
Hosting
+
Inference
+
Maintenance
+
Evaluation

If RAG already meets the application's requirements, I would probably choose RAG.
If the additional 2% is critical and the economics justify it, fine-tuning could be considered.
Practical Challenge 3 — Data Quality
I would choose:
1,000 high-quality examples

over:
10,000 examples with 30% incorrect answers.

Incorrect examples can teach incorrect behavior.
10,000 examples
30% wrong
      ↓
Noisy training signal

versus:
1,000 examples
Almost all correct
      ↓
Cleaner training signal

The lesson recommends starting with a small amount of high-quality data rather than assuming more data automatically produces a better model.
Practical Challenge 4 — Overfitting
Given:
Training Accuracy   = 98%
Validation Accuracy = 87%

this could indicate overfitting.
The model performs extremely well on training examples but significantly worse on unseen validation data.
What I Would Investigate
1. Validation Dataset
Check whether it is:
- Correct
- Representative
- Free from training-data contamination
2. Training Data
Look for:
- Incorrect examples
- Duplicate examples
- Inconsistent answers
- Poor formatting
3. Checkpoints
Evaluate different checkpoints:
Checkpoint 1 → Validation
Checkpoint 2 → Validation
Checkpoint 3 → Validation

4. Generalization
Use completely new inventory questions.
For example:
Training:
What is an Asset Master?

Test:
How should an asset be tracked after assignment to a department?

5. Compare Against Baselines
Base Model
    vs
RAG
    vs
Fine-Tuned Model

If fine-tuning does not provide meaningful improvement, do not deploy it.
Practical Challenge 5 — Choose the Technique
A. Inventory Policy Changes Every Month
Answer: RAG
Policy Documents
      ↓
RAG
      ↓
Current Policy
      ↓
LLM

The information changes frequently, so retraining the model every time the policy changes would be inefficient.
B. Model Doesn't Consistently Follow Exact Response Format
Answer: Prompt Engineering First
Try:
System Prompt
     +
Output Schema
     +
Examples

If the problem persists consistently and is suitable for learned behavior, fine-tuning can then be evaluated.
C. Model Needs Specialized Domain Style and Behavior Consistently
Answer: Fine-Tuning
Fine-tuning is a strong candidate when:
- Prompting is insufficient
- RAG is not solving the problem
- High-quality examples are available
- The specialized behavior is stable
- The improvement justifies the cost
D. Current Stock Quantities from PostgreSQL
Answer: Function Calling
Do not use fine-tuning for current stock numbers.
Use:
User
 ↓
Agent / LLM
 ↓
get_stock()
 ↓
PostgreSQL
 ↓
Current Quantity
 ↓
LLM

Best Architecture
Static Knowledge → RAG
Current Data     → Function Calling + PostgreSQL
Behavior/Style   → Fine-Tuning if Justified
Instructions     → Prompt Engineering

Bonus — When Would You Choose Fine-Tuning Over RAG?
Interview Answer
I would choose fine-tuning over RAG when the main problem is not missing information, but the model's behavior or performance on a specialized task.
For example, in my College Inventory AI, if the model needs to consistently understand college-specific terminology and follow a particular response or workflow style, fine-tuning could be useful.
But I wouldn't immediately fine-tune. I would first establish a baseline using the base model with prompt engineering, and use RAG if the problem involves external or changing knowledge such as procurement policies or college manuals.
I would then evaluate those approaches using a representative test set. If the model still has a consistent behavioral limitation, and I have enough high-quality training examples, I would test fine-tuning against the baseline.
I would compare quality, task accuracy, consistency, token costs, latency, and hosting costs. My decision would be based on measurable improvement and business value, not simply on having a custom dataset.
In short, RAG is mainly for giving the model the right information at runtime, while fine-tuning is for teaching the model more consistent specialized behavior.

Lesson 18 — Final Mental Model
                    AI PROBLEM
                        │
                        ▼
                  What is missing?
                        │
           ┌────────────┼────────────┐
           ▼            ▼            ▼
     Instructions    Knowledge     Behavior
           │            │            │
           ▼            ▼            ▼
        PROMPT          RAG      FINE-TUNING

For the College Inventory AI, there is another important branch:
                  Current Data?
                       │
                       ▼
                Function Calling
                       │
                       ▼
                   PostgreSQL

College Inventory AI Architecture
┌───────────────────────────────────────────────┐
│             College Inventory AI              │
├───────────────────────────────────────────────┤
│                                               │
│ Prompt Engineering → Instructions/behavior   │
│                                               │
│ RAG → Policies / Manuals / SOPs / Documents │
│                                               │
│ Function Calling → Current DB/API data      │
│                                               │
│ Fine-Tuning → Specialized stable behavior   │
│                                               │
└───────────────────────────────────────────────┘

Most Important Lesson 18 Principle
Baseline first → Prompt Engineering/RAG → Evaluate → Fine-tune only if the remaining problem justifies the additional data, compute, effort, and hosting cost.

And for the College Inventory AI:
Don't put dynamic inventory data into fine-tuning. PostgreSQL remains the source of truth for current stock, purchase orders, tickets, and other transactional information.