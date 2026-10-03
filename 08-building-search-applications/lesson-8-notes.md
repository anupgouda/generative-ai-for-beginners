##Lesson 8 — Search Applications, Embeddings & RAG
Q1. What is a search application, and how is it different from a normal web search?

A search application helps users find relevant information from a specific collection of data, such as college documents, company files, or a private knowledge base.

A normal web search usually searches a large collection of publicly available web pages using search-engine indexing and ranking.

For example, a college search application could search only college regulations, notices, academic calendars, and student handbooks.

Q2. What are the main components of a search application?

The main components are:

Data/documents — The information that we want to search.
Document processing — Cleaning and splitting the information into useful chunks.
Embedding model — Converts text into numerical vectors.
Vector database — Stores the vectors and related document information.
Query processing — Processes the user's search query.
Similarity search — Finds documents that are semantically similar to the query.
Results/UI — Displays the relevant information to the user.
Q3. What is semantic search, and how is it different from keyword-based search?

Semantic search tries to understand the meaning or intent behind a query rather than only matching exact words.

For example, if a college document says:

"Students must maintain the minimum required attendance to be eligible for the end-semester examination."

A student searching:

"How much attendance do I need to write the exam?"

could still find the document even though the exact words are different.

Keyword search mainly looks for matching words, while semantic search looks at the meaning of the query and document.

Q4. What are embeddings, and why are they useful for semantic search?

Embeddings are numerical representations of text that capture information about its meaning.

Similar pieces of text tend to have embeddings that are close to each other in vector space.

For example:

"Minimum attendance required for exams"

and

"Attendance eligibility for semester examination"

have different words but similar meanings, so their embeddings can be close.

This allows a search system to find relevant information even when the user's wording doesn't exactly match the document.

Q5. What is a vector database, and what does it store?

A vector database is a database designed to store and search vector embeddings efficiently.

It can store:

Text embeddings
The original text or document chunks
Document IDs
Metadata such as document type, department, date, or access permissions

The system can then perform similarity searches to find the vectors most similar to a user's query.

Q6. Explain the basic flow of a search application using embeddings.

The basic flow is:

User Query
    ↓
Query Embedding
    ↓
Similarity Search
    ↓
Relevant Documents
    ↓
Results
Step 1 — User Query

The student enters a question.

Step 2 — Query Embedding

The embedding model converts the question into a numerical vector.

Step 3 — Similarity Search

The vector database compares the query vector with stored document vectors.

Step 4 — Relevant Documents

The system retrieves the documents or chunks that are most similar to the query.

Step 5 — Results

The relevant information is displayed to the user or passed to an LLM for generating a natural-language answer.

Q7. What is the purpose of chunking documents before creating embeddings?

Large documents are divided into smaller pieces called chunks before creating embeddings.

This is useful because:

Smaller chunks can represent more focused information.
Search can retrieve only the relevant section instead of an entire document.
It helps the system stay within the LLM's context limits.
It can improve retrieval accuracy.

For example, instead of embedding an entire 100-page student handbook as one piece, I could divide it into smaller sections such as attendance, examinations, grading, leave, and disciplinary rules.

Q8. What is RAG, and how does search fit into a RAG application?

RAG stands for Retrieval-Augmented Generation.

RAG combines information retrieval with an LLM.

The basic process is:

User Question
      ↓
Create Query Embedding
      ↓
Search Knowledge Base
      ↓
Retrieve Relevant Information
      ↓
Give Information + Question to LLM
      ↓
Generated Answer

Search is therefore the retrieval part of RAG. It finds relevant information, and the LLM uses that information to generate the final response.

Q9. What factors should you consider when evaluating a search system?

I would consider at least these factors:

Relevance — Are the retrieved documents actually related to the query?
Precision — How many retrieved results are relevant?
Recall — How much of the relevant information does the system successfully retrieve?
Ranking quality — Are the most useful results appearing near the top?
Response time — How quickly does the system return results?
Coverage — Does the system contain the information users are looking for?
User satisfaction — Do users find the results useful?
Access control — Does the system prevent users from retrieving information they are not authorized to see?
Q10. What problems can occur in a search application, and how could you improve the system?

Several problems can occur.

Irrelevant results

The system may retrieve documents that are related to the words in the query but not actually useful.

Improvement: Improve chunking, embeddings, metadata filtering, and ranking.

Missing information

The relevant document may not be retrieved.

Improvement: Improve document processing, chunk overlap, embedding models, and retrieval strategies.

Incorrect ranking

A relevant result might appear below less useful results.

Improvement: Use better similarity methods or a reranking model.

Outdated information

The search system might return an old college notice even though a newer policy exists.

Improvement: Store document dates and metadata and prioritize current/official documents.

Access-control problems

A user might retrieve documents they shouldn't be able to access.

Improvement: Apply authorization and metadata-based access filtering before returning results.

🚀 Challenge 1 — College Semantic Search

I would build a semantic search system for questions such as:

"How many days of attendance do I need to write the semester exam?"

Documents I would store

I would store trusted college documents such as:

Student handbook
Academic regulations
Examination regulations
Attendance policy
College circulars
Department notices
Academic calendar
Official examination guidelines

I would prioritize official and current documents.

How I would chunk them

I would divide documents into meaningful sections rather than arbitrary large pieces.

For example:

Attendance Policy
    ↓
Eligibility Requirements
    ↓
Minimum Attendance
    ↓
Condonation Rules
    ↓
Examination Eligibility

I would include some overlap between chunks when necessary so that important context isn't lost at chunk boundaries.

How I would create embeddings

I would send each document chunk through an embedding model.

The model converts each chunk into a numerical vector representing its semantic meaning.

Where I would store the embeddings

I would store the vectors in a vector database, along with the original chunk and metadata such as:

document_id
department
document_type
date
source
access_level
How the query would be searched

When the student asks:

"How many days of attendance do I need to write the semester exam?"

the system would:

Convert the question into an embedding.
Search the vector database.
Find semantically similar attendance-related chunks.
Apply access and metadata filters.
Rank the results.
Return the most relevant official information.
How I would return the answer

For a simple search application, I could display the relevant document sections and sources directly.

For a RAG version, I would provide the retrieved information to an LLM and ask it to generate a clear answer.

The answer should also provide the source document, so the student can verify the information.

Challenge 2 — Search vs RAG vs Normal LLM
Approach	What it does	Practical use case
Normal LLM	Generates an answer using the knowledge and context available to it	Explaining what Python functions are
Semantic Search	Finds relevant information from a knowledge base based on meaning	Finding the college's attendance policy
RAG	Retrieves relevant information and gives it to an LLM to generate a grounded answer	Answering a student's question using current college regulations
Simple difference

Normal LLM → Generate

Semantic Search → Find

RAG → Find + Generate

For example, if a student asks:

"What is the attendance requirement?"

Semantic search could return the relevant attendance policy.

RAG could take that policy and generate:

"According to the attendance policy, students must meet the required attendance threshold to be eligible for the examination."

with the source attached.

Challenge 3 — Engineering Architecture
Student
   ↓
Search UI
   ↓
Backend API
   ↓
Query Processing
   ↓
Embedding Model
   ↓
Vector Database
   ↓
Similarity Search
   ↓
Relevant Documents
   ↓
(Optional) LLM
   ↓
Final Response
1. What each layer does
Student

The student enters a natural-language question.

Search UI

Provides the interface for entering queries and displaying search results or generated answers.

Backend API

Receives the request and handles authentication, query processing, retrieval, and communication with other services.

Query Processing

Cleans and prepares the user's query and applies any required filters, such as department or document type.

Embedding Model

Converts the user's query into a numerical vector.

Vector Database

Stores document embeddings and metadata and performs similarity searches.

Similarity Search

Finds document chunks whose embeddings are most similar to the query embedding.

Relevant Documents

The system returns the highest-ranked relevant chunks.

Optional LLM

The retrieved information can be passed to an LLM to generate a natural-language response.

Final Response

The user receives the relevant documents, generated answer, or both.

2. Where would I add authentication?

Authentication should happen around the Backend API before the system processes protected requests.

Student
   ↓
Search UI
   ↓
Authentication
   ↓
Backend API

This ensures that the system knows who is making the request.

3. Where would I add logging?

I would add logging primarily in the backend.

I could log:

Request timestamp
Response time
Query ID
Retrieval performance
Errors
System status

I would avoid logging sensitive information unnecessarily.

4. Where would I add access control?

Access control should be applied before returning retrieved documents.

For example, if some documents are only available to faculty or a particular department, the search system should filter them based on the authenticated user's permissions.

A user's access level should also be stored as metadata where appropriate.

5. Where would I add evaluation?

I would evaluate both the retrieval system and the final RAG response.

For retrieval:

Precision
Recall
Ranking quality
Retrieval latency

For the final answer:

Factual accuracy
Relevance
Grounding in retrieved documents
Hallucination rate
User satisfaction

I would create a test set of real college questions with known relevant documents and expected answers.

6. Where would I add security and privacy?

Security should be applied throughout the architecture.

I would use:

Authentication
Authorization
HTTPS
Secure API keys/secrets
Encryption for sensitive data
Access-controlled vector databases
Data minimization
Appropriate data retention
Protection against prompt injection and malicious documents
Careful logging that avoids exposing private information
Lesson 8 Takeaway

The main concept I learned is that search and LLMs solve different parts of an AI application.

A search system helps find the right information, while an LLM helps understand and generate language. RAG combines both:

Query → Retrieve relevant information → Give it to the LLM → Generate a grounded answer