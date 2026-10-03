# Lesson 5 — Creating Advanced Prompts

## 1. Zero-Shot vs Few-Shot Prompting

### Zero-shot prompting
Zero-shot prompting means asking the model to perform a task without providing examples.

Example:

> Classify this review as positive or negative.

### Few-shot prompting
Few-shot prompting provides a few examples of the expected input and output before asking the model to perform the task.

Example:

> "Amazing product." → Positive  
> "Very poor quality." → Negative

Then the model can classify a new review based on these examples.

**Key difference:**  
Zero-shot uses instructions without examples, while few-shot uses examples to demonstrate the expected behavior.

---

## 2. Chain-of-Thought Prompting

Chain-of-thought prompting encourages the model to approach a complex problem through intermediate reasoning steps instead of immediately producing the final answer.

It can be useful for tasks involving multiple steps, such as:

- Mathematics
- Debugging
- Planning
- Complex problem solving

For example, instead of only asking:

> Fix this error.

We can ask the model to:

1. Analyze the error.
2. Identify the cause.
3. Consider possible solutions.
4. Provide the final solution.

---

## 3. Generated Knowledge

Generated knowledge means asking the model to first generate useful facts or relevant knowledge about a topic and then use that information to solve the main task.

For example, before asking an AI Project Mentor to design a database for a college inventory system, we could first ask it to identify:

- Important entities
- Relationships
- Required data
- Business requirements

This is useful when a task requires background knowledge before reaching the final answer.

---

## 4. Least-to-Most Prompting

Least-to-most prompting breaks a difficult problem into smaller problems, solves them one at a time, and then uses those results to solve the larger problem.

For a college inventory management system, instead of asking the model to build everything at once:

1. Identify requirements.
2. Design the database.
3. Design backend APIs.
4. Design authentication.
5. Design the frontend.
6. Integrate the system.
7. Test the system.
8. Plan deployment.

This makes a large engineering problem easier to manage.

---

## 5. Self-Refine

Self-refine means asking the AI to generate an initial answer, critique that answer, and then improve it.

### Workflow

**Generate → Review → Identify problems → Improve → Final answer**

For code:

1. Generate the code.
2. Review the generated code.
3. Find bugs and edge cases.
4. Check security and reliability concerns.
5. Improve the implementation.
6. Return the improved version.
7. Explain the changes.
8. Test the improved code.

The important idea is that the initial model output should not automatically be treated as correct.

---

## 6. Maieutic Prompting

Maieutic prompting uses a process of breaking down reasoning into smaller explanations and examining those explanations to improve the final reasoning.

### Difference from Self-Refine

**Self-refine:**
- Generate an answer.
- Critique the answer.
- Improve the answer.

**Maieutic prompting:**
- Explore the reasoning behind an answer.
- Break reasoning into smaller explanations.
- Examine those explanations for inconsistencies.
- Improve the final reasoning.

---

## 7. Why the Same Prompt Can Produce Different Responses

LLMs generate responses based on probabilities rather than always selecting one fixed answer.

If multiple tokens or answers are reasonable, the model can produce different responses.

Output can be affected by factors such as:

- Temperature
- Sampling
- Context
- Small changes in the prompt

Therefore, the same prompt does not necessarily guarantee exactly the same response.

---

## 8. Temperature

Temperature controls how predictable or varied the model's output is.

- **Lower temperature** → more deterministic and consistent responses.
- **Higher temperature** → more varied and creative responses.

Examples:

- Code generation → generally lower temperature.
- SQL generation → generally lower temperature.
- Brainstorming ideas → higher temperature.
- Creative project names → higher temperature.

The general principle is:

> Lower temperature → deterministic and consistent  
> Higher temperature → varied and creative

---

# Practical Challenges

## Challenge 1 — Self-Refine

### Initial Prompt

```text
Create a Python function that validates an email address.
Self-Refine Prompt
Review the Python email-validation function generated above.

Perform the following steps:

1. Check whether the code works correctly.
2. Identify bugs or incorrect assumptions.
3. Identify important edge cases.
4. Check for security and reliability concerns.
5. Suggest improvements.
6. Rewrite the function with the improvements.
7. Explain what you changed and why.
8. Provide a few test cases to verify the improved function.

Do not assume the original implementation is correct.
If there is a limitation in the validation approach, clearly explain it.
Workflow

Generate code → Review → Find problems → Improve → Test

This demonstrates how self-refine can be applied to software engineering rather than blindly accepting generated code.

Challenge 2 — Least-to-Most Prompting
You are an experienced software architect helping me build a college inventory management system.

Do not try to design the entire system at once.

Break the project into these stages:

1. Requirements
2. Database
3. Backend API
4. Authentication and authorization
5. Frontend
6. Integration
7. Testing
8. Deployment

For each stage:

- Explain the goal.
- Identify the important components.
- Describe the decisions that need to be made.
- Identify dependencies on previous stages.
- Produce a clear output that can be used in the next stage.

Start with Stage 1: Requirements.

After completing each stage, summarize the decisions before moving to the next stage.

Do not invent requirements.
Clearly identify assumptions and ask for missing information when necessary.

At the end, provide an overall architecture showing how all stages connect.

This prevents the model from trying to solve a large engineering problem in one response.

Challenge 3 — Temperature
Task	Temperature	Reason
Generate SQL query	0.1–0.2	Predictable and precise output
Debug Python code	0.1–0.2	Consistent and technically focused output
Generate creative project names	0.8–1.0	More variation and creativity
Generate a fixed JSON response	0.0–0.1	Highly deterministic and consistent formatting
Brainstorm AI project ideas	0.8–1.0	Different ideas are valuable

The exact temperature values depend on the model and API.

The general principle is:

Lower temperature → deterministic and consistent

Higher temperature → varied and creative

Challenge 4 — Advanced AI Project Mentor

The following prompt combines:

Project context
Few-shot examples
Least-to-most decomposition
Self-refine
Output constraints
# AI Engineering Project Mentor

You are an experienced software architect and engineering mentor.

Your job is to analyze a student's project and technical problem, propose a solution, critically review that solution, improve it, and provide a final recommendation.

## 1. Project Context

Student Project:
{project_description}

Technology Stack:
{tech_stack}

Current Architecture:
{current_architecture}

Current Code:
{current_code}

Error / Problem:
{problem}

Error Message:
{error_message}

## 2. Few-Shot Examples

Example 1:

Problem:
The React application loses newly created records after refreshing the browser.

Good analysis:
The frontend may only be storing the records in component state or localStorage instead of persisting them in the backend database.

Good solution:
Send the record to a backend API and persist it in PostgreSQL. Retrieve the records from the API when the page loads.

Example 2:

Problem:
The API returns 500 when creating an inventory item.

Good analysis:
Check the backend logs, request body, database schema, validation, and SQL query.

Good solution:
Identify the exact failing layer before changing the code.

Use these examples as guidance, but do not assume that the student's problem is identical.

## 3. Least-to-Most Analysis

Analyze the problem in stages:

Stage 1 — Understand the requirements.

Stage 2 — Identify the affected components.

Stage 3 — Identify the likely root cause.

Stage 4 — Consider possible solutions.

Stage 5 — Select the most appropriate solution.

Stage 6 — Explain implementation steps.

## 4. Proposed Solution

Provide:

- Root cause
- Recommended solution
- Required code changes
- Dependencies
- Implementation steps

Do not invent files, APIs, functions, or project details that were not provided.

## 5. Self-Refine

Now critically review your proposed solution.

Check for:

- Incorrect assumptions
- Bugs
- Missing edge cases
- Security issues
- Scalability problems
- Reliability problems
- Compatibility issues
- Unnecessary complexity

Then improve the solution based on this review.

## 6. Final Recommendation

Return only the improved recommendation using this structure:

### Problem Analysis
Explain the problem.

### Root Cause
Explain the most likely cause and identify uncertainty if applicable.

### Recommended Solution
Explain the solution.

### Implementation
Provide the required code or configuration.

### Verification
Give clear steps for testing the solution.

### Risks and Limitations
Mention anything the student should be careful about.

### Confidence
State whether the recommendation is high, medium, or low confidence and explain why.

## Important Rules

- Do not invent information.
- If the provided information is insufficient, say what is missing.
- Prefer reliable evidence from the provided project context.
- Do not claim that code works without verification.
- Keep explanations understandable for an engineering student.
Lesson 5 — Key Takeaways

Advanced prompting is not only about writing longer prompts.

The main techniques learned are:

Zero-shot — solve a task without examples.
Few-shot — provide examples to guide the model.
Chain-of-thought — approach complex problems through intermediate reasoning.
Generated knowledge — generate relevant knowledge before solving the main task.
Least-to-most — decompose a large problem into smaller problems.
Self-refine — generate, review, improve, and test.
Maieutic prompting — examine and improve reasoning through smaller explanations.
Temperature — control the degree of deterministic versus varied output.
Engineering Principle

For software engineering tasks:

Do not blindly trust the first AI-generated answer. Generate → Review → Refine → Test.