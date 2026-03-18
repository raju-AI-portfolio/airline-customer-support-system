# AI-Powered Airline Customer Support Agent

## Overview

Airlines manage thousands of daily customer support requests related to **flight status, delays, baggage policies, cancellations, and refunds**. Traditional support systems rely heavily on human agents, leading to high operational costs and slow response times.

This project demonstrates the design and implementation of an **AI-powered intelligent support agent** that automates airline customer queries using:

- Large Language Models (LLMs)
- Retrieval-Augmented Generation (RAG)
- Structured flight data from PostgreSQL
- Guardrails for AI safety
- Workflow orchestration using n8n

The solution showcases how **agentic AI systems** can integrate operational data, enterprise knowledge bases, and workflow automation to deliver accurate and scalable customer support.

---

# Product Vision

Create an **intelligent conversational support agent** capable of resolving common airline queries autonomously while ensuring **accuracy, safety, and scalability**.

The system integrates:

- Operational data (flight schedules)
- Knowledge documents (airline policies)
- LLM reasoning
- AI guardrails

This architecture represents a **practical enterprise implementation of AI copilots and intelligent agents**.

---

# Problem Statement

Customer service operations in the airline industry face several challenges:

- High volume of repetitive queries
- Slow response times
- High operational costs
- Inconsistent support experiences

### Examples of common customer questions

- “Is my flight delayed?”
- “What is the baggage allowance?”
- “How do I request a refund?”
- “What happens if my flight is cancelled?”

These questions require **both structured operational data and policy knowledge**, making them ideal candidates for **AI-powered support agents**.

---

# Product Objectives

## Business Goals

- Reduce customer support workload
- Improve response time
- Increase automation of repetitive queries
- Enhance customer satisfaction

## Technical Goals

- Build an **LLM-powered intelligent support agent**
- Integrate **structured and unstructured data sources**
- Implement **AI safety guardrails**
- Demonstrate **workflow orchestration**

---
  
<img width="6800" height="3878" alt="Flowchart" src="https://github.com/user-attachments/assets/4267e165-ae9d-48b8-9cec-6b0df6fca9f2" />



---

# Target Users

| User | Need |
|------|------|
| Passengers | Fast answers to flight queries |
| Customer support teams | Reduced workload |
| Airline operations | Consistent information delivery |

---

# Key Product Capabilities

## 1. Intelligent Query Understanding

The system uses an **LLM** to interpret user questions and determine the correct information source.

---

## 2. Structured Data Access

Flight-related queries are resolved using a **PostgreSQL flight database**.

---

## 3. Knowledge Retrieval (RAG)

Policy-related questions retrieve answers from airline documentation using **vector search**.

---

## 4. Workflow Automation

The entire AI pipeline is orchestrated using **n8n workflows**.

<img width="1600" height="603" alt="image" src="https://github.com/user-attachments/assets/871c4e13-642d-4eeb-b300-5b336c9385f5" />

---

## 5. AI Safety and Guardrails

Input and output guardrails prevent:

- Unsafe queries
- Prompt injection
- Hallucinated responses
- Unauthorized data access
## 🧠 Core AI Components

| Component | Purpose |
|-----------|---------|
| LLM | Understand and generate responses |
| PostgreSQL | Store flight data |
| Vector Database (Pinecone) | Store airline knowledge base |
| RAG | Retrieve relevant policy information |
| Guardrails | Ensure safe and compliant AI responses |
| n8n | Orchestrate workflow automation |

---

## 🧩 Technology Stack

| Layer | Technology |
|------|------------|
| LLM | OpenAI GPT / Llama |
| Workflow | n8n |
| Database | PostgreSQL |
| Vector Database | Pinecone |
| Backend | Python |
| Safety | LLM Guard |
| Automation | GitHub Codespaces |
# 📂 Project Repository Setup

## Step 1 — Create GitHub Repository

1. Go to **GitHub**
2. Create a new repository:

```
airline-customer-support-system
```

3. Add the following files:

- `README.md`
- `Python .gitignore`

4. Start **GitHub Codespaces**

---

# ⚙️ Step 2 — Install n8n

Run the following command in the Codespaces terminal:

```bash
npm install n8n -g
```

This installs the **n8n workflow automation tool**.

### Configure Port

Forward port:

```
5678
```

Set visibility:

```
Public
```

Copy the **Forwarded Address**.

---

# 🔐 Step 3 — Configure Environment Variables

Create a `.env` file:

```env
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=https
WEBHOOK_URL=ADD_YOUR_FORWARDED_ADDRESS
N8N_EDITOR_BASE_URL=ADD_YOUR_FORWARDED_ADDRESS
N8N_WEBHOOK_TUNNEL_URL=ADD_YOUR_FORWARDED_ADDRESS
```

Load environment variables:

```bash
source .env
```

---

# 🐍 Step 4 — Create Python Environment

Create a virtual environment:

```bash
python -m venv venv
```

Activate environment:

```bash
source venv/bin/activate
```

Install required packages:

```bash
pip install llm-guard pandas psycopg2
```

---

# 📦 Step 5 — Upload Project Files

Upload the following files to the repository:

```
README.md
guardrails.py
Flights_Schedule.csv
add_data_to_db.py
```

Commit files:

```bash
git add .
git commit -m "Project files added"
git push
```

---

# 🐘 Step 6 — Start PostgreSQL using Docker

Run PostgreSQL container:

```bash
docker run -it -d -p 5432:5432 \
-e POSTGRES_PASSWORD=mypassword \
--name=postgrescont postgres:latest
```

Enter the container:

```bash
docker exec -it postgrescont psql -U postgres
```

---

# 🗄️ Step 7 — Create Database

List databases:

```sql
SELECT datname FROM pg_database WHERE datistemplate = false;
```

Create database:

```sql
CREATE DATABASE airlinedb;
```

Switch database:

```sql
\c airlinedb
```

---

# 📊 Step 8 — Create Flights Table

```sql
CREATE TABLE IF NOT EXISTS flights (
    id BIGINT PRIMARY KEY,
    flight_no TEXT NOT NULL,
    airline_code TEXT NOT NULL,
    airline_name TEXT NOT NULL,
    origin TEXT NOT NULL,
    destination TEXT NOT NULL,
    departure_scheduled TIMESTAMP NOT NULL,
    arrival_scheduled TIMESTAMP NOT NULL,
    departure_date DATE GENERATED ALWAYS AS (departure_scheduled::date) STORED,
    departure_time TIME GENERATED ALWAYS AS (departure_scheduled::time) STORED,
    arrival_date DATE GENERATED ALWAYS AS (arrival_scheduled::date) STORED,
    arrival_time TIME GENERATED ALWAYS AS (arrival_scheduled::time) STORED,
    status TEXT CHECK (status IN ('On Time','Delayed','Cancelled')),
    delay_minutes INT DEFAULT 0,
    delay_reason TEXT DEFAULT '',
    terminal TEXT,
    gate TEXT,
    aircraft_type TEXT,
    seats_total INT,
    seats_booked INT,
    fare_inr INT
);
```

Check tables:

```sql
\dt
```

---

# 📥 Step 9 — Insert Flight Data

Run Python script:

```bash
python add_data_to_db.py
```

Verify records:

```sql
SELECT * FROM flights LIMIT 10;
```

---

# 🔄 Step 10 — Start n8n Workflow

Activate environment:

```bash
source .env
source venv/bin/activate
```

Start n8n:

```bash
n8n start
```

Open the forwarded URL to access the **n8n dashboard**.

---

# 🔁 Step 11 — Import Workflow

In **n8n**:

1. Click **Create Workflow**
2. Click **Import from File**
3. Upload:

```
N8N_Airline_Customer_Support_System_Workflow.json
```

---

# 🔀 Query Routing Logic

| Query Type | Route |
|------------|-------|
| Flight data query | SQL + PostgreSQL |
| Policy question | RAG Agent |
| General query | Direct LLM response |

---

# 🛡️ Guardrails Implementation

Guardrails ensure **safe AI responses**.

## Input Guardrails

Checks for:

- Toxic language
- Prompt injection
- Sensitive data
- Violence

## SQL Guardrails

Prevents:

- `DELETE` queries
- `UPDATE` queries
- Data manipulation

## Output Guardrails

Ensures:

- Policy compliance
- No hallucinations
- Grounded answers

---

# 📚 RAG Knowledge Base

The system retrieves airline information using **vector search**.

Sources include:

- Airline policies
- FAQ documents
- Passenger guidelines

Vector store:

```
Pinecone
```

---

# 📊 Product Metrics

| Metric | Target |
|------|------|
| Response time | < 3 seconds |
| Automation rate | > 70% |
| Answer accuracy | > 90% |
| Customer satisfaction | > 4.5 / 5 |
---

# Activate and Test the workflow
 - Save and Activate the workflow
 - Access the N8N chat window and start asking questions like:
   
- ○ Are there any flights from Delhi to Nagpur on 11th Nov
- ○ What is the status of flight 6E477 on 10 Nov 2025?
- ○ How much free baggage is allowed for domestic flights?
- ○ Do you allow musical instruments in flight?

- Check the workflow executing the correct nodes and following the correct path to get the response.
- Debug if issue persists
---

# 💰 ROI Impact

For a mid-size airline:

| Metric | Impact |
|------|------|
| Support cost | ↓ 40% |
| Response time | ↓ 80% |
| Agent workload | ↓ 60% |

---

**n8n Workflow architecture:**

<img width="1382" height="523" alt="image" src="https://github.com/user-attachments/assets/d41efdc3-fc1b-4d89-bfdd-e80e9763dd01" />



