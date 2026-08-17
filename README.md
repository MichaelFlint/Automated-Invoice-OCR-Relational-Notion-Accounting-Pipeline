# 📄 Automated Invoice OCR & Relational Notion Accounting Pipeline

A production-grade, AI-driven automation workflow built in **n8n** that intercepts incoming email attachments, performs structured OCR data extraction using **OpenAI (GPT-4o)**, and maps invoice headers and line items into a relational **Notion** accounting database. Equipped with defensive data validation and a global error handling sub-workflow.

---

## 🌟 Key Features

* **Automated Ingestion:** Listens for inbound PDF invoice attachments via Gmail and securely archives them on Google Drive.
* **Structured AI Extraction (LLM):** Enforces strict JSON Schema outputs using OpenAI to extract invoice metadata (vendor, NIP/tax ID, dates, totals, currency) and line items without regex limitations.
* **Relational Notion Data Model:** Creates a parent invoice record in Notion, then splits and loops through individual line items (`Split Out` & `Loop Over Items`) to populate child line-item records.
* **Mathematical Guardrails:** Validates financial integrity ($Net + VAT = Gross$) and normalizes currency codes (PLN, EUR, USD) prior to database insertion.
* **Production-Grade Error Resilience:**
  * Node-level **Exponential Backoff / Retry-on-Fail** policy across external APIs (OpenAI, Google Drive, Notion) to absorb transient network issues and rate limits.
  * Dedicated **Global Error Trigger Sub-Workflow** that intercepts runtime failures and sends formatted HTML alert emails with direct n8n execution log links.

---

## 🏗️ Workflow Architecture



<img width="2071" height="490" alt="image" src="https://github.com/user-attachments/assets/908c0d95-c6e5-4eb2-b057-e7b0d964e348" />




## 🛡️ Reliability & Error Handling Sub-System

To prevent pipeline silent failures and data corruption, the workflow implements a dual-layer fault-tolerance model:

1. **Rate Limit & Transient Error Mitigation:**
   * **Notion API:** Retries 3 times with 2000ms delay during batch line-item loops to prevent 429 Rate Limit errors.
   * **OpenAI API:** Configured with exponential backoff against API degradation or timeout spikes.
2. **Global Failure Alerting:**
   * Linked to a decoupled **Error Trigger Sub-Workflow**.
   * Unhandled runtime exceptions trigger an automated HTML email notification displaying the failing node name, timestamp, stack trace/error payload, and a direct URL to inspect the execution logs in n8n.

---

## 🛠️ Tech Stack & Integrations

* **Orchestration:** n8n (Sub-workflows, Loop nodes, Error Trigger, Binary Stream Handling)
* **AI & Extraction:** OpenAI API (`gpt-4o-mini`, Structured Outputs / JSON Schema)
* **Database & CRM:** Notion API (Relational Database Properties, Batch Entries)
* **Storage & Messaging:** Gmail API, Google Drive API, HTML/SMTP Alerting
* **Logic & Parsing:** Custom JavaScript (`Code Node` data sanitization, type-casting, math checks)



🚀 Setup & Installation
Import Workflow: Load the workflow.json into your n8n instance.

Configure Credentials:

Gmail OAuth2

Google Drive OAuth2

OpenAI API Key

Notion API Integration Token

Set Up Notion Database:

Create a parent Invoices database (Title, Number, Vendor, Total Amount, Currency).

Create a child Line Items database linked via Relation to the parent database.

Attach Error Handler: Set the Error Workflow in Workflow Settings to point to your deployed Error Trigger pipeline.
