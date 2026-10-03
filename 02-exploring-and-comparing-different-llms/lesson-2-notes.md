# Lesson 2 — Exploring and Comparing Different LLMs

## Learning Objectives

- Understand why different LLMs are suitable for different use cases
- Understand how to evaluate and compare LLMs
- Understand model cards and benchmarks
- Understand multimodal models
- Understand prompt engineering, RAG, and fine-tuning
- Understand important factors when selecting a model for a production application

---

## Q1. Why shouldn't we always choose the most powerful LLM?

We should not always choose the most powerful LLM because it may be more expensive, slower, and require more resources. For a simple task, a smaller model might give good enough results while being faster and cheaper.

The goal is to choose a model that provides the required quality without unnecessary cost or latency.

---

## Q2. What factors would you consider when selecting an LLM for a production application?

I would consider:

- Accuracy
- Cost
- Response speed
- Context-window size
- Reliability
- Privacy and security
- Scalability
- API support
- Coding ability
- Performance for the specific task

The goal is to choose a model that provides the required quality without unnecessary cost or latency.

---

## Q3. What is a model card?

A model card is documentation that provides important information about an AI model. It can explain the model's intended use, limitations, training information, evaluation results, and possible risks.

It helps developers understand how and when the model should be used.

---

## Q4. What is the difference between a benchmark and testing a model on your own application data?

A benchmark tests a model using a standardized dataset or task so that models can be compared.

Testing on our own application data checks how well the model actually performs for our specific use case.

A model can perform well on general benchmarks but still perform poorly on a specific application.

---

## Q5. What is RAG?

RAG stands for **Retrieval-Augmented Generation**.

It allows an LLM to retrieve relevant information from an external knowledge source and use that information to generate its answer.

This means the model does not have to rely only on the knowledge it learned during training.

---

## Q6. When would RAG be more appropriate than fine-tuning?

RAG is more appropriate when the AI needs to use private, domain-specific, or frequently changing information.

For example, if an application needs to answer questions using company documents, we can retrieve the relevant documents when the user asks a question instead of retraining the model every time the documents change.

---

## Q7. What is fine-tuning?

Fine-tuning is the process of taking a pretrained model and training it further on a specialized dataset.

It can make the model better suited to a particular task, behavior, or style.

---

## Q8. AI Project Mentor and Private GitHub Repository

### My Choice: B. RAG

I would use RAG because the student's GitHub repository contains private and project-specific information that the model would not know from its pretrained knowledge.

The system could retrieve relevant files or code from the repository and provide that information to the LLM when answering the student's question.

RAG is also useful because the repository can change over time. We can update the retrieved knowledge without retraining the entire model.

### Why Not A?

The model's pretrained knowledge would not contain the student's private repository or the latest changes in their code.

### Why Not C?

Fine-tuning is not the best way to provide constantly changing repository-specific information. RAG is better suited for retrieving the current information when it is needed.

---

## Q9. Prompt Engineering vs RAG vs Fine-tuning

Prompt engineering means designing better instructions and context for an existing model.

RAG means connecting the model to external knowledge and retrieving relevant information when the user asks a question.

Fine-tuning means training an existing model further on a specialized dataset so that it learns a particular task, behavior, or style.

In simple terms:

| Technique | What it does |
|---|---|
| Prompt Engineering | Changes how we ask the model |
| RAG | Gives the model relevant external information |
| Fine-tuning | Changes the model through additional training |

---

# Model Selection Analysis

For my AI Project Mentor, I would not automatically choose the largest or most powerful model.

The model should be selected based on the actual requirements of the application.

| Factor | Why it matters |
|---|---|
| Accuracy | The AI needs to give useful technical explanations and code suggestions. |
| Cost | Students may use the system frequently, so API costs need to be controlled. |
| Latency | Students should not have to wait too long for responses. |
| Context window | The AI may need to understand multiple files from a project. |
| Coding ability | The model should be capable of understanding and explaining source code. |
| Privacy | Student repositories may contain private code and information. |
| Reliability | The application should provide consistent responses. |
| Scalability | The system should support many students if colleges use it. |

## My Conclusion

I would choose the model based on the balance between **quality, cost, speed, context handling, coding ability, privacy, and scalability** rather than simply choosing the model with the highest benchmark score.

For private GitHub repositories, I would combine the LLM with **RAG** so that the model can retrieve the student's current project information when answering questions.

---

# Key Takeaways

- The most powerful LLM is not automatically the best choice for every application.
- Model selection should consider quality, cost, latency, context, privacy, reliability, and scalability.
- Model cards provide important information about a model's intended use, limitations, evaluation, and risks.
- Benchmarks are useful for comparison, but application-specific testing is also important.
- RAG allows an LLM to use relevant external information at response time.
- Fine-tuning adapts a pretrained model using specialized training data.
- Prompt engineering improves the instructions and context provided to an existing model.
- RAG is useful for private or frequently changing information such as project repositories.
- Production AI systems should select models based on actual application requirements rather than benchmark scores alone.