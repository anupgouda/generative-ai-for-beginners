# Lesson 4 — Prompt Engineering Fundamentals

## Overview

Prompt Engineering is the process of designing and optimizing prompts to deliver consistent and quality responses for a given application objective and model.

Prompts are an important programming interface for Generative AI applications because the way a prompt is written can influence the quality and format of the model's response.

Prompt engineering is an iterative process:

```text
Design Prompt
     ↓
Test Response
     ↓
Identify Problems
     ↓
Refine Prompt
     ↓
Test Again
Learning Objectives

In this lesson, I learned:

What Prompt Engineering is and why it matters
How prompts are constructed
How tokenization affects LLM processing
The difference between base and instruction-tuned LLMs
Zero-shot, one-shot, and few-shot prompting
Prompt cues
Prompt templates
Primary and secondary content
Prompt engineering best practices
How to iteratively improve prompts
Q1. What is Prompt Engineering?

Prompt Engineering is the process of designing and optimizing text inputs to deliver consistent and quality responses for a given application objective and model.

It is important because carefully designed prompts can improve the quality, consistency, and usefulness of model responses.

Prompt engineering involves:

Designing the initial prompt
Testing the response
Refining the prompt
Validating the result
Q2. What is the Difference Between a Base LLM and an Instruction-Tuned LLM?
Base LLM

A Base LLM or Foundation Model primarily predicts the next token based on statistical patterns learned from its training data.

It does not necessarily understand instructions in the same way an instruction-tuned model does.

Instruction-Tuned LLM

An Instruction-Tuned LLM starts with a foundation model and is further trained using examples containing instructions and desired responses.

This helps the model follow instructions and produce responses that are better suited to practical applications.

Foundation Model
       ↓
Instruction Tuning
       ↓
Instruction-Tuned LLM
       ↓
Better Task Following
Q3. Why Does Tokenization Matter?

Tokenization is the process of converting text into smaller units called tokens.

LLMs process tokens rather than raw text.

Different models can tokenize the same prompt differently. Since LLMs are trained on tokens, tokenization can affect how the prompt is processed and how the model generates its response.

Tokens also influence:

Context window usage
Prompt length
Processing requirements
Token-based API costs
Q4. What Are the Three Major Challenges That Make Reliable LLM Responses Difficult?

The lesson identifies three important challenges:

1. Stochastic Responses

The same prompt can produce different responses across different models, model versions, or repeated executions.

2. Fabrications

Models can generate information that is inaccurate, imaginary, or contradictory to known facts.

The lesson uses the term fabrication for this behavior.

3. Different Model Capabilities

Different models and model generations have different capabilities, strengths, limitations, costs, and complexity.

Prompt engineering can help create better guardrails and workflows that account for these differences.

Q5. What is the Difference Between a Basic Prompt and an Instruction Prompt?
Basic Prompt

A basic prompt is a simple text input sent to the model.

Example:

Explain Python.
Instruction Prompt

An instruction prompt provides more detailed guidance about the task and desired output.

Example:

Explain Python to a beginner.

Use three simple examples.
Keep the explanation under 200 words.

The instruction prompt provides clearer guidance about the expected task and format.

Q6. Explain Zero-Shot, One-Shot, and Few-Shot Prompting
Zero-Shot Prompting

Zero-shot prompting gives the model a task without providing examples.

Example:

Classify this review as positive or negative:

"The product was excellent."
One-Shot Prompting

One-shot prompting provides one example before asking the model to perform the task.

Example:

"The product was terrible." → Negative

Now classify:

"The product was excellent."
Few-Shot Prompting

Few-shot prompting provides multiple examples before the actual task.

Example:

"Excellent product." → Positive
"Very disappointing." → Negative
"I really liked it." → Positive

Now classify:

"The quality was amazing."

Examples help the model infer the desired pattern.

Q7. What is a Prompt Cue?

A prompt cue is a word, phrase, or structure that nudges the model toward a desired response.

Example:

Summarize this article.

Top 3 things we learned:
1.

The beginning of the expected output acts as a cue that guides the model toward the desired format.

Another example:

Explain the solution step-by-step:

The cue encourages the model to produce a step-by-step response.

Q8. What is a Prompt Template and Why is it Useful?

A prompt template is a predefined and reusable recipe for constructing prompts.

It can contain placeholders that are replaced with information from different sources.

Example:

Student Project:
{project_description}

Technology Stack:
{tech_stack}

Current Problem:
{problem}

Relevant Error:
{error_message}

Prompt templates are useful because they provide:

Reusability
Consistency
Easier maintenance
Programmatic customization
Scalability

For a real application, a template can be reused for many users while changing only the relevant input data.

Q9. What Are the Three Prompt Engineering Mindset Principles?

The lesson identifies three important principles.

1. Domain Understanding Matters

Understanding the application domain helps customize prompts, templates, examples, cues, and context for specific users and use cases.

2. Model Understanding Matters

Different models have different:

Training data
Capabilities
APIs or SDKs
Strengths
Limitations
Optimization areas

Prompt engineering should take these model-specific characteristics into account.

3. Iteration and Validation Matters

Prompt engineering is a trial-and-error process.

Prompts should be tested, refined, and validated using the actual application domain and expected results.

Successful approaches can be recorded and organized into reusable prompt libraries.

Q10. Prompt Engineering Best Practices

Important best practices from the lesson include:

1. Evaluate the Latest Models

Evaluate newer models for quality, features, cost, and impact before making migration decisions.

2. Separate Instructions and Context

Clearly distinguish instructions from primary and secondary content.

3. Be Specific and Clear

Provide details about:

Context
Desired outcome
Length
Format
Style
4. Be Descriptive and Use Examples

Start with zero-shot prompting and use one-shot or few-shot examples when examples can improve the desired behavior.

5. Use Cues

Provide leading words or phrases that guide the model toward a desired response.

6. Double Down

Sometimes important instructions can be repeated before and after the primary content.

7. Order Matters

The order in which information is presented can affect model output.

8. Give the Model an "Out"

Provide a fallback response that the model can use when it cannot reliably complete the task.

Example:

If you do not have enough information to answer,
say "I don't have enough information" instead of guessing.
Practical Challenge — AI Engineering Project Mentor
Version 1 — Basic Improvement

Weak prompt:

Help me with my project.

Improved version:

I am working on an engineering project.

Help me understand and solve the problem I am currently facing.

Explain the problem clearly, identify possible causes,
and suggest a practical solution.

If code is needed, provide a simple example and explain it.
Version 2 — Instruction Prompt
Role:

You are an experienced software engineering mentor who helps
engineering students build and debug projects.

Task:

Analyze the student's project problem and provide practical
guidance to solve it.

Context:

The student will provide their project description,
technology stack, current implementation, and the problem
they are facing.

Instructions:

- Understand the problem before suggesting a solution.
- Identify the most likely causes.
- Do not invent information about the student's project.
- If information is missing, clearly state what is needed.
- Explain technical concepts in a way that a student can understand.
- Provide code examples when they are useful.
- Explain why the recommended solution should work.
- If you are uncertain, clearly state the uncertainty.

Expected Output:

1. Problem analysis
2. Possible causes
3. Recommended solution
4. Code example, if needed
5. Verification steps
6. Possible limitations or risks
Version 3 — Reusable Few-Shot / Template Prompt
# AI Engineering Project Mentor

You are an experienced software engineering mentor helping
college students understand, build, debug, and improve
their engineering projects.

## Student Project

Project Description:
{project_description}

Technology Stack:
{tech_stack}

Current Implementation:
{current_implementation}

Current Problem:
{problem}

Relevant Error Message:
{error_message}

## Instructions

1. First understand the student's project and current problem.
2. Use the provided information instead of making assumptions.
3. If important information is missing, identify what is missing.
4. Explain the problem in simple language.
5. Identify the most likely cause.
6. Recommend a practical solution.
7. Provide code only when it is useful.
8. Explain the important parts of the code.
9. Include verification steps.
10. If uncertain, clearly state the uncertainty instead of inventing information.

## Example

Student Project:
College Inventory Management System

Technology Stack:
React, Node.js, PostgreSQL

Current Problem:
New inventory records disappear after refreshing the page.

Expected Analysis:

- Check whether the frontend only stores records in memory.
- Check whether the API request is being sent.
- Check whether the backend saves the record to PostgreSQL.
- Verify the database record after the request.

Example Output:

1. Problem analysis
2. Possible cause
3. Recommended solution
4. Code example
5. Verification steps

## Analyze This Student's Project

Student Project:
{project_description}

Technology Stack:
{tech_stack}

Current Problem:
{problem}

Expected Output:

1. Problem analysis
2. Possible cause
3. Recommended solution
4. Code example
5. Verification steps
6. Limitations or risks
Applying Prompt Engineering to a Real AI System

Prompt engineering can be combined with application context and retrieved information.

For my AI Project Mentor:

Student Input
      ↓
Prompt Template
      ↓
Project Context
      ↓
Retrieved Information / RAG
      ↓
LLM
      ↓
Response
      ↓
Evaluation

The prompt should instruct the model to use the information provided and avoid inventing information when sufficient context is unavailable.

Key Takeaways
Prompt engineering is the design and optimization of prompts.
Prompt quality can influence response quality and consistency.
Base LLMs primarily perform token prediction.
Instruction-tuned LLMs are trained to follow instructions.
Tokenization converts text into tokens processed by LLMs.
Zero-shot uses no examples.
One-shot uses one example.
Few-shot uses multiple examples.
Prompt cues guide the model toward a desired response.
Prompt templates enable reusable and scalable prompt design.
Domain understanding is important.
Model understanding is important.
Iteration and validation are essential.
Clear and specific prompts generally provide better guidance.
Examples, cues, templates, and fallback responses can improve prompt design.
Project Application

For my AI Engineering Project Mentor, prompt engineering can help create more consistent and reliable responses.

The system can use reusable prompt templates containing:

Student project information
Technology stack
Current implementation
Error messages
Relevant documentation
Expected response format

The model should use the available information and clearly state when it does not have enough information rather than inventing an answer.