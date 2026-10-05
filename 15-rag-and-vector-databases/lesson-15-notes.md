🚀 Lesson 15 — RAG

The most important idea to remember is:

RAG gives an LLM access to relevant external information at query time instead of expecting the model to know everything from its training.

For your College Inventory AI, an important distinction is:

RAG → documents/knowledge
Function calling → live data/actions
PostgreSQL → system of record
Q1. What is RAG?

RAG stands for Retrieval Augmented Generation.

It is a technique where an application first retrieves relevant information from an external knowledge source and then gives that information to the LLM so it can generate a grounded answer.

Without RAG:

User
 ↓
LLM
 ↓
Answer from model knowledge

With RAG:

User
 ↓
Search knowledge base
 ↓
Relevant information
 ↓
LLM
 ↓
Grounded answer
Why is RAG useful?

An LLM's foundational training data may not contain:

Your company's internal documents
College policies
Recent documents
Private organizational information
Specialized knowledge

For example, your college could upload:

"College Procurement Policy 2026.pdf"

The LLM doesn't automatically know the contents of your private PDF.

RAG retrieves the relevant section and provides it to the model.

Q2. Complete RAG workflow

The basic workflow is:

Knowledge Base
      ↓
User Query
      ↓
Retrieval
      ↓
Augmented Generation

Let's break it down.

1. Knowledge Base

First, documents are collected.

For your college:

Procurement Policy
Asset Management Manual
Inventory Guidelines
College Rules
Vendor Procedure

These documents are processed and stored in a searchable form.

2. User Query

The student or employee asks:

"What is the procedure for purchasing stationery?"

The application converts the question into a form suitable for searching.

3. Retrieval

The system searches the knowledge base for the most relevant chunks.

For example:

User Query
   ↓
Embedding
   ↓
Vector Search
   ↓
Top relevant chunks

It might retrieve:

"Stationery purchases above ₹50,000 require HOD approval..."

4. Augmented Generation

The retrieved information is added to the LLM's context.

Conceptually:

User Question
+
Retrieved Information
        ↓
       LLM
        ↓
Final Answer

The LLM can then answer:

"According to the procurement policy, stationery purchases above ₹50,000 require HOD approval."

Q3. Why do we need a knowledge base?

We can't simply put every document directly into every prompt because:

There may be thousands of documents.
The prompt would become extremely large.
It would increase cost.
It would increase latency.
The model has a finite context window.
Most documents aren't relevant to every question.

Instead, we process the documents and retrieve only the relevant pieces.

Document preprocessing

Documents may come in formats such as:

PDF
DOCX
TXT
HTML

They are first converted into usable text.

Chunking

Large documents are divided into smaller pieces called chunks.

For example:

100-page document
       ↓
500 smaller chunks

A chunk might contain one paragraph or a few related paragraphs.

Why?

When the user asks a question, we don't need to retrieve the entire 100-page document.

We only need the relevant sections.

Embeddings

Each chunk is converted into a numerical representation called an embedding.

Conceptually:

"Stationery requires HOD approval"
              ↓
      [0.12, -0.43, 0.81, ...]

The embedding represents the semantic meaning of the text.

Database storage

The chunks and their embeddings are stored in a searchable database, often a vector database.

Then:

Question
   ↓
Query embedding
   ↓
Vector database
   ↓
Similar chunks
Q4. What is a vector database?

A vector database stores and searches numerical vector representations of data, usually embeddings.

1. What does it store?

It can store:

Document chunk
+
Embedding
+
Metadata

For example:

Chunk:
"Stationery purchases require HOD approval."

Embedding:
[0.21, 0.43, -0.12, ...]

Metadata:
Document = Procurement Policy
Page = 12
2. What is an embedding?

An embedding is a numerical representation of the semantic meaning of data.

For example:

"Purchase stationery"

and

"Buy office supplies"

have different words but similar meanings.

Their embeddings should therefore be relatively close in vector space.

3. Why are embeddings useful?

They allow applications to search by meaning, rather than only exact words.

For example, a user asks:

"How do I buy office supplies?"

A document says:

"Stationery procurement procedure..."

Keyword search may struggle because the exact words differ.

Vector search can recognize their semantic similarity.

4. Why is chunking important?

If you embed an entire 500-page document as one giant piece, retrieval becomes less precise.

Chunking allows the system to retrieve:

the specific section relevant to the question.

Good chunking therefore improves retrieval quality.

Three vector databases

Examples include:

Pinecone
Weaviate
Milvus

Other technologies can also support vector search, including PostgreSQL with the appropriate vector extension.

Q5. Keyword Search vs Vector Search vs Hybrid Search
Search	How does it work?	Main advantage
Keyword	Searches for matching words/terms	Simple and effective for exact terms
Vector	Searches using embedding similarity	Understands semantic meaning
Hybrid	Combines keyword + vector search	Benefits from both approaches
Keyword search

Suppose the document contains:

"A4 Xerox paper procurement"

Search:

"A4 Xerox paper"

It can find exact matching terms.

Good for:

IDs
Product names
Exact phrases
Technical terms
Vector search

Search:

"How do I purchase office supplies?"

It can retrieve a document containing:

"Stationery procurement procedure"

even though the wording is different.

Hybrid search

Hybrid search combines both approaches.

Keyword Search
      +
Vector Search
      ↓
Better retrieval

This can be especially useful when both exact matching and semantic understanding matter.

Q6. What is vector similarity?

Vector similarity measures how close two vector representations are.

If two pieces of text have similar meanings, their embeddings should generally be close in vector space.

For example:

"Buy stationery"
        ↘
         Similar semantic area
        ↗
"Purchase office supplies"
1. Cosine similarity

Cosine similarity measures the angle/directional similarity between vectors.

A common interpretation is:

1    → very similar
0    → unrelated
-1   → opposite direction

The exact behavior depends on the embedding space and implementation.

Cosine similarity is widely used in semantic search.

2. Euclidean distance

Euclidean distance measures the straight-line distance between two points.

Small distance → more similar
Large distance → less similar

Think of two locations on a map.

3. Dot product

Dot product calculates the sum of corresponding vector components multiplied together.

Depending on how embeddings are normalized, a larger dot product can indicate greater similarity.

Simple memory trick
Cosine       → angle
Euclidean    → distance
Dot product  → vector multiplication/similarity
Q7. What is reranking?

Suppose retrieval returns 10 potentially relevant documents:

Query
 ↓
Retriever
 ↓
10 results

The first retrieval stage may be fast but not perfectly precise.

A reranking model examines the retrieved candidates more carefully and reorganizes them according to relevance.

10 retrieved chunks
       ↓
Reranker
       ↓
Best 3 chunks
       ↓
LLM
Why rerank?

Because the most semantically similar result isn't always the most useful result.

Reranking can improve:

Precision
Relevance
Answer quality
Grounding
Q8. Why is RAG useful?
1. Information richness / up-to-date information

RAG lets the application retrieve information from an external knowledge base.

For example:

College policy updated
       ↓
Update knowledge base
       ↓
RAG retrieves new information

You don't necessarily need to retrain the entire model just because a policy document changed.

2. Reducing fabrication

Without grounding, the model may generate an answer based on its learned patterns.

With RAG:

Question
 ↓
Relevant evidence
 ↓
LLM
 ↓
Answer

This can reduce hallucinations/fabrication, although RAG does not guarantee zero hallucinations.

The quality of the retrieved information matters.

3. Cost effectiveness compared with fine-tuning

If your goal is to provide the model with changing organizational knowledge, RAG can be more practical than retraining/fine-tuning a model every time the information changes.

For example:

Policy changes
 ↓
Update documents/index
 ↓
RAG retrieves new policy

instead of repeatedly modifying the model.

Q9. How would you evaluate a RAG application?

The four metrics are:

1. Quality

Does the application provide a useful and correct answer?

Example:

Question:

"What approval is required for stationery purchases?"

A good response should actually answer the question.

2. Groundedness

Is the answer supported by the retrieved information?

Suppose the retrieved document says:

"HOD approval is required."

But the AI says:

"Principal approval is required."

The answer is poorly grounded.

3. Relevance

Are the retrieved documents actually relevant to the question?

Suppose the user asks about procurement.

The system retrieves:

Student Hostel Rules
Sports Guidelines
Library Policy

Those are not relevant.

4. Fluency

Is the generated answer clear, readable, and natural?

For example:

Bad:

"Procurement stationery HOD approval yes policy."

Better:

"According to the procurement policy, stationery purchases require HOD approval."

Example evaluation

Suppose we have 100 test questions.

We could manually or automatically evaluate:

Quality:       90/100
Groundedness:  94/100
Relevance:     92/100
Fluency:       97/100

This gives us a much better picture than simply asking:

"Does the chatbot seem good?"

🔥 Q10. RAG vs Function Calling
Question 1

"How many A4 Xerox papers are currently available?"

Answer: B. Function calling → PostgreSQL

Because this is live, changing data.

The correct architecture is:

User
 ↓
LLM
 ↓
get_stock()
 ↓
Node.js API
 ↓
PostgreSQL
 ↓
100 units
 ↓
LLM
 ↓
Final answer

Why not RAG?

Because a vector database may contain an older document saying:

"A4 paper: 500 units."

The actual database could now say:

100 units.

For current inventory quantities, your database should be the source of truth.

Question 2

"What is the procurement policy for purchasing stationery?"

Answer: RAG

Because this is relatively stable document-based knowledge.

Architecture:

Question
 ↓
RAG
 ↓
Procurement Policy
 ↓
Relevant section
 ↓
LLM
 ↓
Answer
Easy rule

Current state → Function calling/database

Document knowledge → RAG

🚀 Challenge 1 — Design RAG for College Inventory

For relatively stable college documents:

Procurement Policies
Asset Management Guidelines
Inventory Procedures
College Rules

I would design:

Documents
   ↓
Document Preprocessing
   ↓
Chunking
   ↓
Embeddings
   ↓
Vector Database
   ↓
User Query
   ↓
Query Embedding
   ↓
Similarity Search
   ↓
Relevant Chunks
   ↓
LLM
   ↓
Answer + Sources

Let's break it down.

Step 1 — Documents

Collect:

Procurement Policy.pdf
Asset Management Manual.pdf
Inventory Procedure.pdf
College Rules.pdf
Step 2 — Preprocessing

Extract text from the documents.

Clean unnecessary formatting and prepare the content.

Step 3 — Chunking

Break documents into meaningful sections.

For example:

Procurement Policy
      ↓
Chunk 1 → General procurement
Chunk 2 → Purchase approval
Chunk 3 → Vendor selection
Chunk 4 → Stationery procedure
Step 4 — Embeddings

Generate embeddings for each chunk.

Chunk
 ↓
Embedding model
 ↓
Vector
Step 5 — Vector database

Store:

Chunk
+
Embedding
+
Metadata

Metadata might contain:

Document name
Page number
Department
Document version
Date
Step 6 — User query

User:

"What approval is needed for stationery purchases?"

Convert the question to an embedding.

Step 7 — Retrieval

Search the vector database for relevant chunks.

Step 8 — LLM

Provide:

Question
+
Retrieved chunks

to the LLM.

Step 9 — Answer + source

The AI responds:

"According to the procurement policy, stationery purchases above the specified threshold require HOD approval."

Then display:

Source:
Procurement Policy 2026
Page 12
🚀 Challenge 2 — Inventory Knowledge Architecture

Here's how I would classify your six examples:

Information	RAG	PostgreSQL	Function/Tool
Current stock quantity	❌	✅	✅
Procurement policy	✅	❌	❌
Current PO status	❌	✅	✅
Asset management manual	✅	❌	❌
Current ticket status	❌	✅	✅
Vendor registration procedure	✅	❌	❌
Why?
Current stock

Stock changes frequently.

PostgreSQL = source of truth
Function = controlled access
Procurement policy

It's document knowledge.

Policy document
 ↓
RAG
Current PO status

Purchase orders change over time.

PostgreSQL
 ↓
Function
Asset management manual

A relatively stable document.

Manual
 ↓
RAG
Current ticket status

Dynamic application data.

PostgreSQL
 ↓
Function
Vendor registration procedure

This is procedural/document knowledge.

Vendor Procedure
 ↓
RAG
🚀 Challenge 3 — RAG Failure

The vector database contains:

A4 paper: 500 units

But PostgreSQL says:

A4 paper: 100 units.

1. What went wrong?

The RAG knowledge base contains stale dynamic information.

The vector database retrieved an old document instead of current inventory data.

2. Why is RAG inappropriate here?

Because current inventory quantity is transactional and frequently changing data.

A document/vector index is not the ideal source of truth for something like:

Current stock
Current PO status
Current ticket status
Current balance
3. What should the AI use?

It should use:

Function calling → backend API → PostgreSQL

User
 ↓
LLM
 ↓
get_stock()
 ↓
Node.js
 ↓
PostgreSQL
 ↓
100
4. How prevent this architecturally?

Define clear data ownership.

STATIC / DOCUMENT KNOWLEDGE
        ↓
       RAG

DYNAMIC / TRANSACTIONAL DATA
        ↓
    PostgreSQL

ACTIONS
        ↓
Function Calling

Also make the tool description explicit:

"Use this tool whenever the user asks for current inventory quantity."

This reduces the chance of the model answering from stale knowledge.

🚀 Challenge 4 — RAG Evaluation

You have:

100 test questions

Relevant answers: 82
Grounded answers: 91
Fluent answers: 88
What does this tell us?
Relevance = 82%

The retrieval/answer system is relevant for about 82% of the test cases.

That suggests retrieval may need improvement.

Groundedness = 91%

91 answers were supported by the retrieved information.

That's better than the relevance score.

Fluency = 88%

Most responses are readable and natural, but there is still room for improvement.

User complaint

"The AI gives answers that sound good but aren't supported by our documents."

The first metric I would investigate is:

Groundedness

Because the problem is specifically that the answers aren't supported by the source material.

I would also investigate relevance, because poor retrieval can lead to poor grounding.

So:

First → Groundedness
       ↓
Then → Retrieval/Relevance
🔥 Bonus Interview Question — RAG vs Fine-Tuning
RAG

RAG gives the model access to external information at inference/query time.

Question
 ↓
Retrieve information
 ↓
LLM
 ↓
Answer
Fine-tuning

Fine-tuning modifies a model's learned behavior using additional training examples.

Base Model
 ↓
Training Examples
 ↓
Fine-tuned Model
Comparison
Factor	RAG	Fine-tuning
Knowledge	Retrieves external knowledge	Knowledge/behavior influenced during training
Freshness	Easy to update knowledge source	New training may be needed for changing knowledge
Model modification	Doesn't fundamentally retrain the base model	Changes model behavior through additional training
Cost	Often practical for changing knowledge	Can require additional training resources
Best for	Private/current documents and factual grounding	Specialized behavior, style, or task patterns
Example

Suppose your college changes its procurement policy.

With RAG
New Policy PDF
 ↓
Process + Embed
 ↓
Update Vector DB
 ↓
AI retrieves new policy

You don't need to retrain the model simply because the document changed.

With fine-tuning

You would generally use training examples to teach the model a desired behavior or specialization.

Fine-tuning is not the ideal mechanism for constantly changing factual information.

🎯 When would I choose each?
Choose RAG when:
You have private documents.
Information changes regularly.
You need citations/sources.
You want answers grounded in organizational knowledge.
You don't want to retrain the model for every document update.
Choose fine-tuning when:
You need specialized behavior.
You need a particular response style/format.
You have a suitable high-quality training dataset.
Prompting/RAG alone isn't enough for the desired behavior.

And they can also be combined:

Fine-tuned Model
       +
      RAG
       +
Function Calling
       ↓
Powerful AI Application
🔥 The Architecture I Would Use for Your College Inventory AI

This is the key takeaway from Lesson 15:

                         USER
                           │
                           ▼
                    ┌──────────────┐
                    │  React Chat  │
                    └──────┬───────┘
                           │
                           ▼
                       ┌───────┐
                       │  LLM  │
                       └───┬───┘
                           │
             ┌─────────────┼──────────────┐
             │             │              │
             ▼             ▼              ▼
           RAG        Function Calls    Normal
             │             │           Response
             ▼             ▼
      Vector Database   Node.js API
             │             │
             │             ▼
             │        PostgreSQL
             │
      College Documents
The decision logic is:
User asks about...
        │
        ├── Current stock?
        │       ↓
        │   Function → PostgreSQL
        │
        ├── Current PO?
        │       ↓
        │   Function → PostgreSQL
        │
        ├── Current ticket?
        │       ↓
        │   Function → PostgreSQL
        │
        ├── Procurement policy?
        │       ↓
        │      RAG
        │
        ├── Asset management manual?
        │       ↓
        │      RAG
        │
        └── General conversation?
                ↓
               LLM
One-line interview answer

RAG grounds an LLM with relevant external knowledge at query time, while function calling lets an LLM access live systems and perform controlled actions; for an enterprise AI application, using the right mechanism for each type of information is more reliable than trying to make the LLM answer everything from its own knowledge.