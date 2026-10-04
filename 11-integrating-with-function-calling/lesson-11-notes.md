# Lesson 11 — Function Calling in GenAI

## Q1. What is function calling, and why is it useful in a GenAI application?

Function calling is a mechanism that allows an LLM to request that an application execute a specific function or tool.

For example, a user asks:

> "What is the current stock of A4 paper?"

The LLM may recognize that it needs actual database information.

Instead of inventing an answer, it can request:

```json
{
  "name": "get_stock",
  "arguments": {
    "item_name": "A4 Xerox Paper"
  }
}

The application then executes the function and gets the real data.

Why is it useful?

It allows an LLM to interact with:

Databases
APIs
Search systems
Weather services
Payment systems
Inventory systems
Internal business applications

The important advantage is:

The LLM handles language and reasoning, while your application handles actual operations and data access.

Q2. What problem occurs when we simply ask an LLM to return JSON from an unstructured prompt?

If we simply tell an LLM:

"Return the answer as JSON."

The model may not always follow the exact structure we expect.

For example, we might expect:

{
  "item": "A4 Paper",
  "quantity": 100
}

But the model might return:

Sure! Here is the information:

{
  "item": "A4 Paper",
  "quantity": "100"
}

Or it could:

Add extra text
Change field names
Omit fields
Produce invalid JSON
Use the wrong data type
Return a different structure

This becomes a problem when our program needs to reliably parse the response.

Function calling/tool definitions provide a more structured way for the model to request an operation and supply arguments according to a defined schema.

Q3. Difference between LLM generating a response vs LLM requesting a function call
LLM generating a response

The model directly produces text for the user.

User
 ↓
LLM
 ↓
"Your stock is 100 units."

The LLM is generating the answer itself.

LLM requesting a function call

The model determines that it needs external information or an action.

User
 ↓
LLM
 ↓
Request tool/function
 ↓
Application executes function

For example:

{
  "name": "get_stock",
  "arguments": {
    "item_name": "A4 Xerox Paper"
  }
}

The LLM is not claiming to know the current stock. It is asking the application to retrieve it.

Q4. Does the LLM actually execute the function?

No.

This is one of the most important concepts.

The LLM only generates a tool/function call request.

Your application executes the actual function.

For example:

User
 ↓
LLM
 ↓
"Call get_stock with A4 Paper"
 ↓
Your Python/Node.js application
 ↓
get_stock()
 ↓
PostgreSQL

The LLM doesn't directly connect to PostgreSQL.

Your application controls the connection.

Therefore:

LLM = decides what tool is needed
Application = executes the tool
Q5. What are the three main steps involved in creating a function call?

The basic process can be understood in three stages.

1. Define the function/tool

Tell the LLM what function is available.

For example:

get_stock

with its description and parameters.

2. Let the LLM request the function

The user asks a question.

The LLM decides:

"I need to call get_stock."

It produces the function name and arguments.

3. Execute the function and provide the result back

Your application executes the function.

get_stock("A4 Xerox Paper")

It might return:

{
  "item": "A4 Xerox Paper",
  "quantity": 100,
  "location": "Main Store"
}

The result is then sent back to the LLM so it can generate the final natural-language response.

Q6. What are name, description, and parameters used for?

These are used to tell the LLM about the function.

name

Identifies the function.

"name": "get_stock"

It tells the model:

This is the name of the tool it can request.

description

Explains what the function does.

"description": "Get the current stock quantity of an inventory item."

This helps the LLM understand when the function should be used.

parameters

Defines the information the function needs.

For example:

{
  "item_name": {
    "type": "string",
    "description": "Name of the inventory item"
  }
}

So the model understands:

Function: get_stock

Needs:
item_name
Q7. What does tool_choice="auto" mean?

tool_choice="auto" means:

Allow the LLM to decide whether it should use one of the available tools/functions or simply answer normally.

For example:

Question 1

"What is function calling?"

The LLM probably doesn't need a tool.

LLM → Answer directly
Question 2

"What is the current stock of A4 paper?"

The LLM can determine that it needs the inventory tool.

LLM → get_stock()

So:

tool_choice="auto"
        ↓
LLM decides
   ↙       ↘
Answer     Tool call
directly
Q8. Explain the complete function-calling flow

The flow is:

User
 ↓
LLM
 ↓
Function Call
 ↓
Python Function
 ↓
External API
 ↓
Function Result
 ↓
LLM
 ↓
Final Answer

Let's use the college inventory system.

Step 1 — User

The user asks:

"Show me the current stock of A4 Xerox paper."

Step 2 — LLM

The LLM understands that it needs real inventory data.

It requests:

get_stock(item_name="A4 Xerox paper")
Step 3 — Function call

Your application receives the tool request.

Step 4 — Python function

Your Python function executes.

Conceptually:

get_stock("A4 Xerox paper")
Step 5 — External API/database

The function could call your backend:

Python
 ↓
GET /api/stock
 ↓
Node.js backend
 ↓
PostgreSQL

PostgreSQL might contain:

A4 Xerox Paper
Quantity: 100
Location: Main Store
Step 6 — Function result

The function returns structured data:

{
  "item": "A4 Xerox Paper",
  "quantity": 100,
  "location": "Main Store"
}
Step 7 — LLM

The result is given back to the LLM.

The LLM now has verified information.

Step 8 — Final answer

The LLM converts the structured result into natural language:

"There are currently 100 A4 Xerox paper units in the Main Store."

This is much safer than asking the LLM to guess the stock.

Q9. Three real-world use cases
1. College Inventory System
"How many laptops are available?"
        ↓
get_stock()
        ↓
Database
2. Banking Application
"What is my current account balance?"
        ↓
get_account_balance()
        ↓
Bank API
3. Weather Application
"What's the weather in Bangalore?"
        ↓
get_weather()
        ↓
Weather API

Other examples include:

Booking flights
Checking delivery status
Creating support tickets
Sending emails
Searching company databases
Making calendar events
Q10. Why is function calling useful for databases and external APIs?

An LLM's knowledge is not necessarily the current state of your system.

Suppose your database contains:

A4 Paper = 100

Tomorrow it becomes:

A4 Paper = 65

The LLM itself doesn't automatically know that your PostgreSQL database changed.

Function calling lets it retrieve the real-time information.

Without function calling
User
 ↓
LLM
 ↓
Possible hallucination
With function calling
User
 ↓
LLM
 ↓
Tool
 ↓
Database/API
 ↓
Real data
 ↓
LLM
 ↓
Answer

This is particularly important for an inventory system because quantities, purchase orders, tickets, vendors, and approvals are dynamic data.

🚀 Challenge — College Inventory System

The architecture could eventually look like:

                          ┌──────────────┐
                          │     User     │
                          └──────┬───────┘
                                 ↓
                          ┌──────────────┐
                          │  AI Chatbot  │
                          │     LLM      │
                          └──────┬───────┘
                                 ↓
                    ┌────────────┼────────────┐
                    ↓            ↓            ↓
               get_stock   get_pending   create_ticket
                            purchase_orders
                    ↓            ↓            ↓
                    └────────────┼────────────┘
                                 ↓
                          Backend API
                                 ↓
                           PostgreSQL

This is a practical use of function calling.

Function 1 — Get Current Stock

User:

"Show me the current stock of A4 Xerox paper."

1. Function

I would create:

get_stock

Its purpose:

Retrieve the current inventory quantity and details for an item.

2. Parameters

It could accept:

{
  "item_name": "A4 Xerox Paper"
}

Possible schema:

{
  "name": "get_stock",
  "description": "Get the current stock information for an inventory item.",
  "parameters": {
    "item_name": {
      "type": "string",
      "description": "Name of the inventory item"
    }
  }
}

It could later be expanded with:

item_name
department
storage_location

But initially, keep it simple.

3. What database/API would it call?

The existing architecture already has:

Frontend
    ↓
Node.js/Express Backend
    ↓
PostgreSQL

The function could call an API such as:

GET /api/stock

or preferably an item-specific endpoint:

GET /api/stock/search?item=A4%20Xerox%20Paper

Then the backend queries PostgreSQL.

Conceptually:

SELECT *
FROM stock
WHERE item_name ILIKE '%A4 Xerox Paper%';
4. What would the function return?

Suppose the database contains:

Item: A4 Xerox Paper
Quantity: 100
Unit Price: ₹5.50
Location: Main Store

The function should return structured information:

{
  "item": "A4 Xerox Paper",
  "quantity": 100,
  "unit_price": 5.50,
  "storage_location": "Main Store"
}
5. How does the LLM generate the final answer?

The result goes back to the LLM.

The LLM converts the structured data into:

"There are currently 100 A4 Xerox paper units available in the Main Store."

The important point:

The LLM didn't invent the 100. PostgreSQL provided it.

Function 2 — Show Pending Purchase Orders

User:

"Show me all pending purchase orders."

Function

Create:

get_pending_purchase_orders
Parameters

It may not require any parameters:

{
  "name": "get_pending_purchase_orders",
  "description": "Retrieve all purchase orders that are currently pending.",
  "parameters": {}
}

You could later add:

department
vendor
date_range
limit
Backend

The function could call:

GET /api/purchase-orders?status=pending

Your backend would query PostgreSQL:

SELECT *
FROM purchase_orders
WHERE status = 'Pending';
Function result

For example:

[
  {
    "po_number": "PO-001",
    "vendor": "ABC Computers",
    "amount": 125000,
    "status": "Pending"
  },
  {
    "po_number": "PO-002",
    "vendor": "XYZ Supplies",
    "amount": 25000,
    "status": "Pending"
  }
]

The LLM could respond:

"There are 2 pending purchase orders: PO-001 from ABC Computers for ₹1,25,000 and PO-002 from XYZ Supplies for ₹25,000."

Function 3 — Create Support Ticket

User:

"Create a support ticket for the printer in the CSE department."

This is different because we're not just reading data.

We're performing an action.

Function

Create:

create_support_ticket
Parameters

The function could require:

{
  "department": "CSE",
  "asset_type": "Printer",
  "issue_description": "Printer issue"
}

A better real-world design would ask the user for missing information if necessary.

For example:

"What problem is the printer having?"

The user responds:

"It isn't printing."

Then the LLM has:

{
  "department": "CSE",
  "asset_type": "Printer",
  "issue_description": "Printer isn't printing"
}
Backend

The function calls something like:

POST /api/tickets

Your Node.js backend receives:

{
  "department": "CSE",
  "asset_type": "Printer",
  "issue_description": "Printer isn't printing"
}

Then PostgreSQL stores the ticket.

Function result

The backend could return:

{
  "success": true,
  "ticket_id": "TKT-1042",
  "status": "Open"
}

The LLM then tells the user:

"I've created support ticket TKT-1042 for the CSE printer. The ticket is currently Open."

Again:

LLM → decides what action is needed
Backend → actually creates the ticket
PostgreSQL → stores it
LLM → explains the result
🔥 The Big Picture for the College Inventory Project

The college inventory chatbot can eventually become an AI interface for the entire inventory system.

For example:

User
 │
 ├── "How much A4 paper do we have?"
 │          ↓
 │      get_stock()
 │
 ├── "Show pending POs."
 │          ↓
 │      get_pending_purchase_orders()
 │
 ├── "Show CSE laptops."
 │          ↓
 │      search_assets()
 │
 ├── "Create a ticket for the printer."
 │          ↓
 │      create_support_ticket()
 │
 └── "Who approved PO-102?"
            ↓
        get_po_approval()

All of these can ultimately connect to:

                   AI Assistant
                       ↓
                  Function Calling
                       ↓
                    Backend
                       ↓
                   PostgreSQL
                       ↓
      ┌───────────┬─────────┼──────────┬──────────┐
      ↓           ↓         ↓          ↓          ↓
    Assets      Stock     Tickets      POs      Vendors
Interview Takeaway

Function calling allows an LLM to choose and request a predefined tool, while the application—not the LLM—executes that tool and returns the real result to the model for the final response.
