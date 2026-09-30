# AI Sales Agent - README

## 1. Project Overview

AI Sales Agent is a sales analytics solution for **Madar Electronics**.
It combines a simple sales system, a Supabase database, n8n automation,
an AI Agent, and a RAG knowledge base.

The solution is designed to answer management questions in Arabic using
two sources:

-   **Live sales data** for numerical KPIs and analysis.
-   **Sales Knowledge Base (RAG)** for policies, KPI definitions,
    business context, and guidance.

Currency used in the project: **SAR**.

Sample sales period: **January 2026 - March 2026**.

------------------------------------------------------------------------

## 2. Main Components

### Sales System / Interface

The interface provides the core sales-system functions required by the
assignment, including:

-   Customers
-   Products
-   Orders
-   Order details
-   Adding a new order

The system is connected to Supabase.

### Supabase Database

Main sales objects:

-   `customers`
-   `products`
-   `orders`
-   `order_items`
-   `sales_analytics` view

AI/RAG objects:

-   `sales_knowledge`
-   `documents`
-   `n8n_chat_histories`
-   `match_sales_knowledge`
-   `match_documents`

The `sales_analytics` view combines orders, customers, products, and
order items and exposes fields used by the AI workflows, including Gross
Sales, Net Sales, discounts, returns, quantity, branch, city, sales
channel, product, customer, order date, and status.

### n8n - Main Sales Agent Workflow

The main workflow:

1.  Receives a question from the chat or webhook.
2.  Reads the current sales data from the Supabase `sales_analytics`
    view.
3.  Calculates KPIs and analytical summaries using JavaScript.
4.  Sends the calculated results to the AI Agent.
5.  Uses the knowledge-base tool when the question requires RAG
    information.
6.  Returns the final Arabic answer to the user.

The workflow calculates and supports analysis such as:

-   Gross Sales
-   Net Sales
-   Discounts and discount percentage
-   Returns and return percentage
-   Orders count
-   Customers count
-   Average Order Value
-   Sales per customer
-   Sales by branch
-   Sales by product
-   Sales by category
-   Sales by city
-   Sales by channel
-   Monthly sales
-   Monthly growth
-   Weekly sales
-   Top customers
-   Return analysis
-   Latest-month comparisons

### n8n - RAG / Knowledge Workflow

The knowledge workflow creates embeddings and stores knowledge content
in the Supabase Vector Store.

The project knowledge base includes at least:

1.  Company and product information
2.  Sales and return policy
3.  KPI / metrics dictionary

The main AI Agent can retrieve this knowledge through the Supabase
Vector Store when required.

------------------------------------------------------------------------

## 3. AI Agent Rules

The AI Agent is designed to:

-   Answer in Arabic.
-   Use SAR for monetary values.
-   Use live calculated sales data for factual numerical answers.
-   Use the Vector Store for knowledge, definitions, policies,
    benchmarks, and business context.
-   Avoid inventing unavailable numbers.
-   Distinguish between facts, hypotheses, and recommendations.
-   Use the latest available month in the dataset instead of assuming
    the current calendar month.
-   Preserve conversational context where applicable.

### Read-Only Safety

The analytical AI Agent is read-only.

It must not execute or claim to execute:

-   `DELETE`
-   `UPDATE`
-   `INSERT`
-   `DROP`
-   `TRUNCATE`

Database modifications such as adding a new order are performed through
the sales system/interface, not through the analytical Agent.

------------------------------------------------------------------------

## 4. Required Credentials

Credentials are **not included** in the submitted workflow files.

After importing the n8n workflows, reconnect the required credentials:

-   Supabase credential
-   OpenAI credential

Do not place API keys or secret tokens directly inside the submitted
JSON files.

------------------------------------------------------------------------

## 5. Setup Instructions

### Step 1 - Create the Supabase Database

Open Supabase SQL Editor and run:

`AI_Sales_Agent_Final_Database.sql`

This creates the sales tables, relationships, sample data, analytics
view, Vector/RAG objects, search functions, and chat-history table.

### Step 2 - Configure Supabase

Confirm that the following objects exist:

-   `customers`
-   `products`
-   `orders`
-   `order_items`
-   `sales_analytics`
-   `sales_knowledge`

### Step 3 - Import n8n Workflows

Import the project workflow JSON files into n8n.

Reconnect the Supabase and OpenAI credentials after import.

### Step 4 - Load the Knowledge Base

Run the RAG/knowledge workflow to create embeddings and index the
project knowledge documents in `sales_knowledge`.

### Step 5 - Activate the Main Workflow

Activate the main Sales Agent workflow and confirm that its webhook is
available.

### Step 6 - Connect the Interface

Configure the project interface to use the production n8n webhook for AI
questions and Supabase for the sales-system data.

------------------------------------------------------------------------

## 6. Verification

The final verification workbook is:

`AI_Sales_Agent_Verification_FINAL.xlsx`

The workbook documents the required questions and additional test cases.

Verified core scenarios include:

-   Monthly Gross Sales and Net Sales
-   Latest two-month comparison
-   Weekly trend
-   Top 5 products
-   Best sales channel and contribution
-   Orders count and Average Order Value
-   Top 5 customers
-   Returns and return-policy retrieval through RAG
-   Follow-up question using conversational context
-   Unsupported period
-   Unsupported metric
-   Connection-failure behavior
-   Read-only / destructive-operation protection

The verification file should be submitted as evidence alongside
screenshots where required.

------------------------------------------------------------------------

## 7. Reference Results

The verified project dataset includes these reference KPIs:

-   Net Sales: **1,439,684.65 SAR**
-   Orders: **360**
-   Customers: **50**
-   Average Order Value: **3,999.12 SAR**
-   Top Branch: **Dammam - 690,016.05 SAR (47.93%)**
-   Top Product: **Gaming Laptop G5 - 169,747.35 SAR (11.79%)**
-   Best Month: **January 2026 - 507,217.45 SAR**
-   March 2026 Net Sales: **484,812.70 SAR**
-   February 2026 Net Sales: **447,654.50 SAR**
-   Latest MoM Growth: **+8.30%**
-   Total Returns: **90,966 SAR**

These values are reference results for the supplied sample dataset.

------------------------------------------------------------------------

## 8. Suggested Submission Structure

``` text
AI_Sales_Agent/
|
|-- README.md
|-- SQL/
|   `-- AI_Sales_Agent_Final_Database.sql
|
|-- n8n/
|   |-- sales.json
|   `-- sales_knowledge.json
|
|-- RAG_Documents/
|   |-- Company_and_Products
|   |-- Sales_and_Return_Policy
|   `-- KPI_Metrics_Dictionary
|
|-- Verification/
|   `-- AI_Sales_Agent_Verification_FINAL.xlsx
|
|-- Screenshots/
|   |-- System_Interface
|   |-- Supabase_Tables
|   |-- Database_Relationships
|   |-- n8n_Main_Workflow
|   `-- n8n_RAG_Workflow
|
`-- Video/
    `-- AI_Sales_Agent_Demo
```

------------------------------------------------------------------------

## 9. Demo Video - Recommended Flow

Recommended duration: **5-8 minutes**.

Suggested sequence:

1.  Briefly introduce the project and business goal.
2.  Show the sales interface.
3.  Show customers, products, orders, and order details.
4.  Demonstrate adding a new order through the system.
5.  Show the relevant Supabase tables and relationships.
6.  Show the main n8n Sales Agent workflow.
7.  Ask a numerical sales question.
8.  Ask a RAG/policy question.
9.  Show the RAG indexing workflow and knowledge source.
10. Briefly show the verification workbook and final project files.

------------------------------------------------------------------------

## 10. Submission Notes

-   Do not submit API keys, passwords, service-role keys, or secret
    tokens.
-   Credential connections must be recreated by the evaluator after
    importing the n8n workflows.
-   Use the production webhook for the final interface/demo.
-   Keep the analytical AI Agent read-only.
-   Include screenshots of the system and database relationship diagram
    as required by the assignment.
-   Keep the verification workbook with the final submission as evidence
    of testing.

------------------------------------------------------------------------

## Project Status

Core sales system: **Completed**

Supabase database and analytics view: **Completed**

AI Sales Agent: **Completed**

RAG / Vector Store: **Completed**

Knowledge documents: **Completed**

Verification workbook: **Completed**

Final documentation: **Completed**

Remaining submission activity: organize final screenshots/relationship
diagram and record the 5-8 minute demo video.
