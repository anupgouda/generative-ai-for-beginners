🚀 Lesson 16 — Open Models

The key idea of this lesson is:

"Open model" does not necessarily mean "fully open-source." Some models provide weights and other useful resources but don't publish everything required by the traditional open-source definition.

For an AI engineer, the important question is not simply "Is it open?", but "How much control, transparency, customization, and flexibility does this model actually give me?"

Q1. What is an Open Model?

An open model is an AI/LLM model whose creators make significant parts of the model available for others to use, study, modify, or deploy.

However, different models make different amounts of information available.

Traditional open-source expectations

For an LLM to fully align with a traditional open-source idea, you would ideally want access to:

Training datasets — the data used to train the model.
Full model weights — the learned parameters of the model.
Evaluation code — code used to test the model.
Fine-tuning code — code/process used to adapt the model.
Training metrics — information about how the model performed during training.

The more of these are available, the more transparent and reproducible the model is.

Why "open models" instead of always saying "open-source models"?

Because many models called "open" don't satisfy all traditional open-source requirements.

For example:

Model
 ├── Weights available       ✅
 ├── Fine-tuning possible    ✅
 ├── Training data public    ❌
 ├── Training code public    ❌
 └── Evaluation code public  ❓

It would therefore be misleading to automatically call every such model "open-source."

So the lesson uses the broader term:

Open models

Q2. Three major benefits of Open Models

The three major benefits are:

Customizability
Cost
Flexibility
1. Customizability

Open models can often be modified, fine-tuned, adapted, or deployed according to your requirements.

Example

Suppose your college needs a model specialized for:

Procurement and inventory terminology.

You could start with an open model and fine-tune/adapt it using suitable examples.

Open Model
    ↓
College-specific data
    ↓
Fine-tuning / adaptation
    ↓
Inventory-specialized model
2. Cost

Open models can potentially reduce API costs, especially when you have the infrastructure to run them yourself.

Instead of:

Your application
      ↓
Paid API
      ↓
Pay per usage

you might deploy a suitable open model on your own infrastructure:

Your application
      ↓
Your inference infrastructure
      ↓
Open model

However, self-hosting isn't automatically cheaper. GPUs, storage, engineering, electricity/cloud infrastructure, and maintenance also cost money.

So the real question is:

What is the total cost for the required performance?

3. Flexibility

Open models can give engineers more options regarding:

Model selection
Deployment
Fine-tuning
Hardware
Infrastructure
Integration
Switching between models

For example, you might use one model for general conversation and another specialized model for a specific task.

Q3. Why is Customizability important?

Customizability allows developers and researchers to adapt models for specific requirements instead of using a general-purpose model exactly as provided.

For example:

General model
       ↓
Specialized data
       ↓
Fine-tuning
       ↓
Specialized model

Possible specialized areas include:

Mathematics
Coding
Healthcare
Finance
Legal information
Scientific tasks
Customer support
Domain-specific business tasks

For your project, you could potentially specialize an open model around:

College inventory and procurement terminology

such as:

Indent
Purchase Order
Asset Master
Stock Entry
Vendor Master
Material Checkout
HOD Approval

The important point is that specialization can improve task-specific behavior, but you still need proper evaluation and grounding.

Q4. Why can Open Models be cost-effective?

AI engineers shouldn't automatically choose the most powerful model.

Instead, think about:

Performance
     vs
Price

Suppose:

Model A
Very high quality
₹10 / 1M tokens

Model B
Slightly lower quality
₹2 / 1M tokens

If Model B performs sufficiently well for your application, paying 5× more for Model A may not make sense.

Example

For a simple inventory query:

"What is the current stock?"

you may not need the largest available model.

A smaller model could potentially:

Understand the question
Select get_stock()
Pass the correct parameter
Present the result

Therefore:

Use the smallest/cheapest model that reliably meets your application's requirements.

Q5. What does Flexibility mean with Open Models?

Flexibility means having more choices in how and where you use models.

You might:

Model A → General conversation
Model B → Coding
Model C → Mathematics

Or compare several models before selecting one.

You could also potentially:

Run models locally
Deploy them in your own cloud
Fine-tune them
Change models
Combine different models
Choose different models for different tasks
HuggingChat Assistants example

The HuggingChat Assistants concept demonstrates that users can create/use assistants based on different available models and configure them for particular purposes.

The broader lesson is:

You don't have to treat one model as the answer to every problem.

You can experiment with different models and choose the one that fits the task.

🚀 Q6. What is Llama 2?

Llama 2 is a family of large language models developed by Meta.

It includes models intended for general language tasks and models adapted for conversational use.

Why is it optimized for chat applications?

A chat-oriented model is adapted to follow instructions and participate in dialogue more effectively.

This involves techniques such as:

Fine-tuning

A base model can be further trained using examples designed to improve its instruction-following and conversational behavior.

Base Llama
    ↓
Fine-tuning
    ↓
Chat-oriented model
Dialogue data

The model can be trained using conversational examples showing how users and assistants interact.

Human feedback

Human feedback can be used to help align model responses with desired behavior.

Example fine-tuned Llama versions

The lesson examples include:

Llama 2 Chat
Code Llama

Code Llama is adapted specifically toward coding-related tasks.

🚀 Q7. What is Mistral known for?

Mistral is known for developing efficient, capable language models, including models that use architectures designed to improve computational efficiency.

One important concept discussed is Mixture-of-Experts (MoE).

What is Mixture-of-Experts?

Instead of activating every part of a large model for every request, an MoE architecture contains multiple specialized expert components.

Conceptually:

                    Input
                      ↓
                Router/Gating
                      ↓
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Expert A    Expert B    Expert C
          │
          └───────────┬───────────┘
                      ↓
                   Output

The routing mechanism selects appropriate experts for the input.

Why can this be efficient?

A model can have many total parameters while only activating a subset for a particular input.

So:

Total model capacity
        ≠
Parameters used for every request

This can potentially provide strong model capacity while reducing computation compared with activating the entire model each time.

Example fine-tuned Mistral models

Examples include:

Mistral 7B Instruct
Mixtral 8x7B Instruct
🚀 Q8. What is Falcon?

Falcon is a family of open models developed by the Technology Innovation Institute.

The lesson specifically discusses Falcon-40B.

40 billion parameters

The "40B" means approximately:

40 billion model parameters.

Parameters are learned numerical values that help determine how the model processes and generates language.

A larger parameter count can provide significant model capacity, but:

More parameters does not automatically mean a better application.

FlashAttention

FlashAttention is an optimized approach for performing attention computations more efficiently.

It can improve:

Memory efficiency
Computation efficiency
Speed in suitable environments
Multiquery attention

Multiquery attention is an attention technique designed to reduce the memory requirements associated with attention, particularly during inference.

This can help make large models more practical to serve.

Reduced memory requirements

These architectural/implementation optimizations can make a model more efficient to run than a naïve implementation of a similarly sized model.

That matters for chat applications because the system needs to process many requests efficiently.

Fine-tuned Falcon examples

Examples from the lesson include:

Falcon-40B-Instruct
Falcon-7B-Instruct

The "Instruct" versions are adapted to follow user instructions more effectively.

🚀 Q9. How should you choose an Open Model?

There is no universally best open model.

The right model depends on the application.

I would follow a process like:

Application requirements
        ↓
Candidate models
        ↓
Benchmarks
        ↓
Real-world testing
        ↓
Cost + latency + hardware
        ↓
Final selection
Microsoft Foundry model catalog

Use the model catalog to discover and compare available models.

I would look at:

Model capabilities
Supported tasks
Deployment options
Performance
Licensing
Availability
Task filters

Filter models based on the task.

For example:

Chat
Coding
Reasoning
Vision
Embeddings
Mathematics

For an inventory chatbot, I'd focus on models suitable for:

instruction following + tool/function calling + conversational tasks.

Hugging Face LLM Leaderboard

Hugging Face leaderboards can help compare models using standardized benchmarks.

But benchmarks shouldn't be the only deciding factor.

A model could perform extremely well on a benchmark but poorly on your specific inventory questions.

Artificial Analysis

Artificial Analysis can be useful for comparing models across areas such as:

Quality
Speed
Cost
Context
Other practical characteristics

This helps you think beyond a single benchmark score.

Fine-tuned models

If a model has already been fine-tuned for your particular task, it can be a strong candidate.

For example:

General model
       vs
Math-specialized model

For mathematical reasoning, the specialized model may be worth testing first.

Experimentation

Finally:

Actually test the models on your application.

Create a representative evaluation dataset.

For your Inventory AI:

100 inventory questions
+
50 procurement questions
+
50 ticket questions

Then compare models based on real results.

🔥 Q10. Model Selection Scenario

We have:

Model A
Excellent quality
Very high resource requirements
High cost
Model B
Good quality
Moderate resources
Lower cost
Model C
Excellent performance for a specific domain
Fine-tuned for your use case
Moderate resources
Which would I test first?

Model C.

Why?

Because it is already specialized for the use case while having moderate resource requirements.

But I wouldn't immediately deploy it.

I'd compare all three using a realistic evaluation.

What would I evaluate?
1. Task accuracy

Can it correctly answer inventory questions?

2. Tool/function calling

Can it correctly choose:

get_stock()
get_pending_purchase_orders()
create_support_ticket()

and provide valid arguments?

3. Quality

Are the answers useful and accurate?

4. Hallucination

Does it invent information?

5. Cost

How much does each request cost?

6. Latency

How quickly does it respond?

7. Hardware requirements

Can we actually run it on our available infrastructure?

8. Context length

Can it handle the amount of information required?

9. Licensing

Can we legally use and deploy the model for our intended purpose?

This is particularly important with "open" models because openness and licensing terms vary.

10. Security/privacy

Can the model be deployed in an environment that satisfies our data requirements?

🚀 Challenge 1 — Open Model Selection

For your College Inventory AI, I'd define the criteria like this:

Factor	Importance	Why?
Quality	High	Must understand user requests correctly
Cost	High	Inventory queries may become frequent
Latency	High	Users expect quick responses
Hardware requirements	High	Determines whether we can practically deploy it
Customization	High	We may need domain-specific behavior
Task performance	Very High	Must perform well on our actual inventory tasks
Domain specialization	High	A specialized model may outperform a general model
Tool/function calling	Very High	The AI needs to interact with our backend
Context length	Medium/High	Useful for longer documents/conversations
Licensing	Very High	Determines whether and how we can legally use it
Security/privacy	High	Important if processing college data
Model size	Medium	Affects deployment requirements
Community/ecosystem	Medium	Helps with troubleshooting and integrations
My evaluation process

I'd create a benchmark specifically for the application:

200 test cases
      ↓
Model A ──┐
Model B ──┼──→ Evaluate
Model C ──┘
             ↓
Quality
Tool calling
Cost
Latency
Safety
             ↓
Final decision

This is much better than simply saying:

"Model C has the highest benchmark score."

🚀 Challenge 2 — Specialized Model

The college wants an AI assistant specifically for mathematical problem solving.

We have:

General-purpose open model
        vs
Math-focused fine-tuned open model

I would investigate the math-focused fine-tuned model first.

Why?

Because it has already been adapted toward the specific task.

For example:

General model
     ↓
Broad capabilities

Math-specialized model
     ↓
Mathematical reasoning
     ↓
Potentially better task performance

This connects directly to the lesson's concept of specialized/fine-tuned open models.

However, I would still benchmark it against the general model.

A specialized model isn't automatically better in every situation.

I'd test:

Accuracy
Multi-step reasoning
Different difficulty levels
Mathematical notation
Hallucination
Response time
Cost

Then choose based on evidence.

🚀 Challenge 3 — Cost vs Performance

We have:

Model A
Accuracy: 95%
Cost: ₹10 / 1M tokens

Model B
Accuracy: 93%
Cost: ₹2 / 1M tokens

Application usage:

10 million tokens/month
Monthly cost

Model A:

10 × ₹10 = ₹100/month

Model B:

10 × ₹2 = ₹20/month

So:

Model	Accuracy	Monthly cost
A	95%	₹100
B	93%	₹20

Model A costs 5× more for a 2-percentage-point accuracy improvement.

Would I automatically choose A?

No.

I'd first ask:

Does the extra 2% accuracy provide enough business value to justify the additional ₹80/month?

For a low-risk chatbot, Model B might be perfectly adequate.

For a high-stakes application, the additional accuracy could potentially justify the cost.

I'd also evaluate:

Latency
Error types
Tool calling
Hardware
Safety
User experience
Scaling costs
🚀 Challenge 4 — Engineering Decision

Team:

"Let's just use the biggest open model because it will give us the best results."

My response:

"I wouldn't select the model purely based on size. A larger model may provide better general capability, but it can also require more memory, compute, cost and latency. More importantly, benchmark performance doesn't necessarily translate into better performance on our specific application.

For our College Inventory AI, I'd first define our requirements and create an evaluation dataset containing real inventory questions, procurement questions and tool-calling scenarios. Then I'd compare multiple models on accuracy, function calling, hallucination rate, latency, cost, hardware requirements, security and licensing.

If a smaller or specialized model provides sufficient quality at a fraction of the cost and latency, it could be the better engineering choice.

So I'd optimize for application-level performance, not simply model size."

The key principle:

The best model is not necessarily the biggest model; it's the model that provides the best trade-off for the application's requirements.

🔥 Bonus Interview Question
Interviewer:

"Why would a company choose an open model instead of a proprietary LLM API?"

Strong answer:

"A company might choose an open model when it needs greater control over customization, deployment, cost and model selection. With an open model, depending on its license and what is actually released, the company may be able to fine-tune it for a specific domain, deploy it on its own infrastructure, or choose hardware and inference strategies that fit its requirements.

Cost can also be an important factor at scale. Instead of paying an API provider for every token, a company can evaluate whether self-hosting or managed deployment of an open model makes economic sense.

Another advantage is flexibility. A company can experiment with different open models and select specialized models for different tasks rather than depending completely on one proprietary provider.

However, I wouldn't automatically choose an open model. I'd compare it with proprietary APIs based on quality, cost, latency, security, licensing, hardware requirements, customization needs and operational complexity.

So the decision isn't 'open is better than proprietary.' It's about choosing the architecture and model that gives the best combination of performance, cost, customization, flexibility and business requirements."

🎯 Final Mental Model for Lesson 16

Remember:

                 OPEN MODELS
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
  Customization     Cost       Flexibility
        │             │             │
        ▼             ▼             ▼
   Fine-tuning    Deployment    Model choice
   Specialization  Economics    Different models

And for selecting a model:

                  Business Need
                       ↓
                 Candidate Models
                       ↓
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      Quality         Cost          Latency
        ↓              ↓              ↓
   Tool Calling     Hardware       Security
        ↓              ↓              ↓
      Domain       Licensing     Customization
        └──────────────┼──────────────┘
                       ↓
                Real Evaluation
                       ↓
                 Model Selection
🔑 Best interview takeaway

Open models give AI engineers more control and flexibility, but model selection should be based on the application's actual requirements rather than simply choosing the largest or highest-scoring model.