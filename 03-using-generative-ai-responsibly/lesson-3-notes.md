# Lesson 3 — Using Generative AI Responsibly

## Overview

Responsible AI focuses on building and using AI systems in a way that is safe, fair, reliable, transparent, inclusive, secure, and accountable.

Generative AI can produce incorrect, harmful, biased, or misleading outputs. Therefore, responsible AI practices should be considered throughout the design, development, evaluation, and operation of a GenAI application.

---

# Learning Objectives

After completing this lesson, I learned:

- Why Responsible AI is important for GenAI applications
- The six Responsible AI principles
- How LLM hallucinations can create risks
- Different types of harmful content
- How fairness applies to GenAI
- Different layers for mitigating AI risks
- How RAG can help reduce hallucinations
- Why LLM outputs must be evaluated
- How UX contributes to Responsible AI
- How to design safeguards for an AI application

---

# Q1. What is Responsible AI?

Responsible AI means building and using AI in a way that is:

- Safe
- Fair
- Reliable
- Transparent
- Respectful of privacy

It is important because GenAI can make mistakes, generate harmful content, show bias, or expose private information.

Responsible AI helps reduce these risks and makes AI applications more trustworthy.

---

# Q2. What is an LLM Hallucination?

An LLM hallucination happens when an AI gives information that sounds correct but is actually incorrect, made up, or unsupported.

### AI Project Mentor Example

My AI Project Mentor might claim that a function exists in a student's code when that function is not actually present in their GitHub repository.

This could cause the student to follow incorrect programming advice.

---

# Q3. Six Responsible AI Principles

The six principles covered in this lesson are:

1. **Fairness**
2. **Reliability and Safety**
3. **Privacy and Security**
4. **Inclusiveness**
5. **Transparency**
6. **Accountability**

These principles provide a framework for considering potential risks when designing and deploying AI systems.

---

# Q4. What is Harmful Content?

Harmful content is AI-generated content that could cause harm to people.

Examples include:

- Hate or discriminatory content
- Instructions that could help someone perform dangerous or harmful activities

Other harmful-content categories discussed in the lesson include self-harm, attacks or violence, illegal activities, and sexually explicit content.

---

# Q5. What Does Fairness Mean in GenAI?

Fairness means that an AI system should not unfairly treat people differently based on characteristics such as gender, race, background, or other personal attributes.

For my AI Project Mentor, students from different backgrounds should receive the same quality of assistance.

The system should be tested with diverse user scenarios to identify potential bias.

---

# Q6. Four Layers of Harm Mitigation

The lesson describes multiple layers that can be used to reduce potential harm.

## 1. Model Layer

Choose an appropriate model and configure it to follow safety requirements.

## 2. Safety System Layer

Use safeguards such as:

- Content filtering
- Moderation
- Jailbreak detection
- Detection of unwanted activities

## 3. Grounding / RAG Layer

Provide reliable information from trusted sources so that the model has relevant context instead of relying only on its internal knowledge.

## 4. UX Layer

Design the user experience to:

- Communicate AI capabilities
- Explain limitations
- Constrain inappropriate inputs
- Help users understand AI-generated results
- Provide ways to report problems

Using multiple layers provides stronger protection than depending on a single mechanism.

---

# Q7. Why is RAG Useful from a Responsible AI Perspective?

RAG (Retrieval-Augmented Generation) can reduce hallucinations by providing the LLM with relevant information from trusted sources before generating an answer.

For my AI Project Mentor, RAG could retrieve:

- The student's current code
- Project documentation
- Relevant technical documentation

This allows the AI to answer based on available project information instead of guessing.

---

# Q8. Why Should We Evaluate LLM Outputs?

LLMs can generate answers that sound convincing even when they are:

- Wrong
- Biased
- Incomplete
- Unsafe

Therefore, applications should evaluate model outputs using different scenarios before users depend on them.

Evaluation can help measure factors such as:

- Accuracy
- Groundedness
- Relevance
- Similarity

This supports reliability and transparency.

---

# Q9. Role of the UX Layer in Responsible AI

The UX layer helps users understand the AI's capabilities and limitations.

For my AI Project Mentor, the interface could:

- Warn students that AI-generated code may contain errors
- Encourage users to review important changes
- Show sources used for RAG-based answers
- Provide a way to report incorrect answers

Good UX helps users make informed decisions instead of blindly trusting AI output.

---

# Q10. Reducing the Risk of Incorrect Programming Advice

If the AI Project Mentor gives incorrect programming advice with high confidence, I would use several layers of protection.

### Model

Choose a model that performs well on coding tasks.

### Safety System

Add safeguards to detect unsafe or unsupported outputs.

### Grounding / RAG

Retrieve the student's actual code and relevant technical documentation so the model has reliable context.

### UX

Clearly communicate that AI-generated suggestions can contain errors and encourage students to verify important changes.

### Evaluation

Create test cases containing:

- Programming errors
- Hallucination scenarios
- Security problems
- Repository-specific questions

Then measure how accurately and safely the AI responds.

---

# Responsible AI Design — AI Engineering Project Mentor

## Risk 1 — Hallucination

### What could go wrong?

The AI could:

- Invent functions
- Misunderstand the student's code
- Recommend an incorrect library
- Provide programming advice that does not work

### How would I reduce it?

I would:

- Use RAG to retrieve the student's current code
- Retrieve relevant technical documentation
- Instruct the model to say when it does not have enough information
- Prevent the model from guessing when evidence is unavailable
- Evaluate responses against repository-specific test cases

---

# Risk 2 — Harmful Content

### What could go wrong?

A student could ask the AI for instructions related to:

- Malware
- Credential theft
- Destructive code
- Other harmful activities

### How would I reduce it?

I would use safety policies and input/output moderation.

Harmful requests should be refused or safely redirected while legitimate educational and defensive cybersecurity questions can still be supported.

---

# Risk 3 — Bias

### What could go wrong?

The AI could make unfair assumptions about students based on:

- Gender
- Background
- Language
- College
- Other personal characteristics

### How would I reduce it?

I would:

- Test similar questions using different demographic contexts
- Compare the generated responses
- Identify inconsistent or biased behavior
- Avoid using unnecessary personal information

---

# Risk 4 — Privacy

## What could go wrong with a student's GitHub repository?

A private repository could contain:

- Source code
- API keys
- Passwords
- Database credentials
- Personal information
- Proprietary information

Incorrect handling could expose this information.

### How would I reduce it?

I would:

- Require permission before accessing a repository
- Use least-privilege GitHub permissions
- Avoid storing unnecessary private information
- Detect and remove secrets such as API keys
- Encrypt sensitive data
- Keep each student's project data isolated
- Allow users to disconnect or delete their repository data

---

# Evaluation — Responsible AI Test Suite

I would create a dedicated **Responsible AI Test Suite** for the AI Project Mentor.

## 1. Hallucination Test

Ask about a function that does not exist in the repository.

**Expected behavior:**  
The AI should acknowledge that the function cannot be found instead of inventing information.

## 2. Harmful Content Test

Test requests involving malware, credential theft, or destructive activities.

**Expected behavior:**  
The system should apply safety policies and avoid providing harmful instructions.

## 3. Bias Test

Ask equivalent questions using different demographic contexts.

**Expected behavior:**  
The system should provide consistent and fair assistance.

## 4. Privacy Test

Check whether information from one student's repository can be accessed by another student.

**Expected behavior:**  
Students must only have access to authorized project information.

## 5. Grounding Test

Ask questions that require information from the student's actual code.

**Expected behavior:**  
The response should be based on retrieved project information.

## 6. Incorrect Advice Test

Provide broken code and ask the AI to identify the problem.

**Expected behavior:**  
The AI should correctly analyze the available code and avoid making unsupported claims.

---

# Evaluation Metrics

I would measure:

| Metric | Purpose |
|---|---|
| Accuracy | Measures correctness of responses |
| Hallucination Rate | Measures unsupported or fabricated answers |
| Harmful Response Rate | Measures unsafe outputs |
| Privacy Violations | Measures unauthorized information exposure |
| Groundedness | Measures whether responses are supported by retrieved information |
| Relevance | Measures whether responses address the user's question |
| Consistency | Measures whether similar inputs receive consistent treatment |

---

# Key Takeaways

### 1. AI can make mistakes

LLMs can generate convincing but incorrect information, so their outputs should not automatically be trusted.

### 2. Responsible AI requires multiple safeguards

Model selection alone is not enough. Safety systems, grounding, UX, and evaluation should work together.

### 3. RAG can improve grounding

Retrieving trusted and relevant information can help reduce unsupported responses.

### 4. Privacy must be considered early

Applications working with private repositories or user data should use authorization, least privilege, isolation, and secure data handling.

### 5. Evaluation is essential

Responsible AI requires testing the system against realistic and adversarial scenarios before relying on it.

---

# Project Application

## AI Engineering Project Mentor

For my AI Project Mentor, Responsible AI would be implemented using the following architecture:

```text
Student
   |
   v
User Interface
   |
   v
Safety / Input Validation
   |
   v
Repository Authorization
   |
   v
RAG / Trusted Documentation
   |
   v
LLM
   |
   v
Output Safety Check
   |
   v
Response + Sources + Limitations
   |
   v
Student Feedback / Evaluation