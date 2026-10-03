# Lesson 10 — Low-Code Development & Microsoft Power Platform

This lesson explores how generative AI and low-code platforms can be used to build applications, automate workflows, process documents, and create AI agents.

## Topics Covered

- Low-code development
- Microsoft Power Platform
- Power Apps
- Power Automate
- Power BI
- Power Pages
- Microsoft Dataverse
- AI Builder
- Microsoft Copilot
- Microsoft Copilot Studio
- Generative answers
- Generative orchestration
- AI agents
- Autonomous agents
- Invoice processing
- Create Text with GPT

## Key Concepts

### Low-Code Development

Low-code platforms use visual interfaces, prebuilt components, connectors, templates, and AI assistance to reduce the amount of traditional programming required.

Low-code does not mean no-code. Developers can still add custom logic when required.

### Microsoft Power Platform

| Product | Purpose |
|---|---|
| Power Apps | Build business applications |
| Power Automate | Build workflows and automations |
| Power BI | Data analysis, dashboards and reporting |
| Power Pages | Build external-facing websites and portals |
| Copilot Studio | Build and customize AI agents |

### Dataverse

Microsoft Dataverse provides structured storage for business data used by Power Platform applications.

It supports:

- Tables
- Relationships
- Security
- Data validation
- Role-based access
- Integration with Power Platform

### AI Builder

AI Builder allows organizations to add AI capabilities to applications and workflows.

Examples:

- Invoice processing
- Receipt processing
- OCR/text recognition
- Sentiment analysis
- Text classification

AI Builder supports both prebuilt and custom AI capabilities.

### Copilot Studio

Copilot Studio can be used to create AI agents that can:

1. Understand user requests
2. Retrieve relevant information
3. Select appropriate tools or knowledge
4. Perform authorized actions
5. Return results

Agents can use knowledge sources such as documents, websites, organizational data, Dataverse, and connected systems.

## Practical Applications

### Student Assignment Tracker

Designed a college assignment management application using:

```text
Power Apps
     ↓
Dataverse
     ↓
Power Automate
     ↓
Email / Notifications

The application supports student and faculty workflows including assignments, deadlines, submissions, grading, and notifications.

AI Invoice Processing

Designed an invoice-processing workflow:

Vendor
  ↓
Invoice Email
  ↓
Power Automate
  ↓
AI Builder
  ↓
Invoice Extraction
  ↓
Database
  ↓
Approval
  ↓
Finance Notification

For an existing engineering backend, Power Platform can integrate through a REST API:

AI Builder
     ↓
Power Automate
     ↓
REST API
     ↓
Node.js Backend
     ↓
PostgreSQL
College AI Agent

Designed a college AI agent using Copilot Studio with:

Approved college documents
Academic regulations
Academic calendar
Assignment information
Dataverse
Power Automate actions
Authentication and authorization

A key requirement is preventing fabricated information:

Approved Knowledge
       ↓
College AI Agent
       ↓
Verified Response

The agent should not invent policies, dates, deadlines, or procedures.

Engineering Takeaway

Low-code platforms are not a replacement for traditional software engineering.

They provide another engineering option for:

Workflow automation
AI document processing
Business applications
Integrations
Rapid prototyping
AI agents

Traditional technologies such as React, Node.js, PostgreSQL, and Python remain valuable for custom APIs, complex business logic, database architecture, advanced AI models, and integrations requiring deeper control.

Traditional Engineering
        +
Low-Code / Automation
        +
AI
        ↓
More Implementation Options
Proof of Work

This lesson includes:

lesson-10-notes.md
Question and answer analysis
Student Assignment Tracker architecture
AI Invoice Processing architecture
College AI Agent architecture
Low-code vs traditional engineering analysis
