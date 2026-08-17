# 📄 Automated Invoice OCR & Relational Notion Accounting System (n8n + OpenAI + Notion)

![n8n](https://img.shields.io/badge/n8n-FF6D5A?logo=n8n&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI-412991?logo=openai&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-000000?logo=notion&logoColor=white)

An automated end-to-end system designed to extract structured data from PDF invoices using AI (OpenAI) and record relational parent/child accounting entries directly into Notion.

## 🎯 Business Problem

Manual processing of incoming PDF invoices causes data entry bottlenecks, leads to human errors in amounts or VAT values, and fails to maintain structured line-item records for financial audits.

**The Solution:** This workflow automates the entire accounting intake pipeline in real-time:
1. **Ingest & Archive:** Automatically catches PDF attachments via Gmail and backs them up to Google Drive.
2. **AI Text Extraction:** Parses raw document text and extracts key metadata and line items using OpenAI (`gpt-4o-mini`).
3. **Parent Record Creation:** Generates a main invoice entry in the primary Notion Invoices database.
4. **Relational Line Items Loop:** Splits line items array and creates child records linked directly to the parent invoice.
5. **Error Handler Sub-Workflow:** Catches runtime exceptions and triggers a formatted HTML email alert with direct n8n log links.

---

## 🏗️ Workflow Architecture



<img width="2071" height="490" alt="image" src="https://github.com/user-attachments/assets/908c0d95-c6e5-4eb2-b057-e7b0d964e348" />


---

## 🛠 Tech Stack

* **Orchestration:** n8n (Gmail Trigger, Google Drive Node, Loop Node, Code Node, Sub-Workflow)
* **AI Engine:** OpenAI API (`gpt-4o-mini`) using Structured Outputs / JSON Schema
* **Database / Accounting:** Notion API (Relational Databases: Parent Invoices & Child Line Items)
* **Logic & Transformation:** JavaScript (`Code Node` in n8n for data sanitization & error parsing)

---

## ✨ Key Features & Production Readiness

* **Zero Hallucination Parsing:** Enforces strict OpenAI JSON Schemas to extract invoice names, vendors, totals, currencies, and item arrays reliably.
* **Relational Record Linking:** Dynamically connects individual line-item sub-records to their parent invoice entry inside Notion.
* **Data Sanitization & Math Integrity:** Verifies $Gross = Net + VAT$ calculations and normalizes currency codes (PLN, EUR, USD) prior to database insertion.
* **Global Error Handling:** Intercepts execution failures with a dedicated `Error Trigger` sub-workflow, dispatching styled HTML alert emails with stack traces.

---

## ⚙️ How to Run / Setup

### Prerequisites

* A running instance of n8n (Cloud or Self-Hosted).
* An active OpenAI API key.
* Connected Google Drive and Gmail accounts.
* A Notion integration token with access to parent and child databases.

### Setup Steps

1. Download the `workflow.json` and `error-subworkflow.json` files from this repository.
2. In your n8n canvas, navigate to **Workflows ➔ Import from File** and select the JSON files.
3. Configure your API Credentials:
   * **OpenAI API Key**
   * **Gmail OAuth2 & Google Drive OAuth2**
   * **Notion Integration Token**
4. Update the **Database ID** parameters in the Notion nodes with your database IDs.
5. In Main Workflow Settings, set **Error Workflow** to point to the imported Error Trigger Sub-Workflow.
6. Toggle the workflow to **Active** (top right corner).

---

## 👤 Author

**Michał Krzemiński**  
AI & Automation Developer

* **LinkedIn:** https://www.linkedin.com/in/micha%C5%82-krzemi%C5%84ski-2052b6428/
* **GitHub:** https://github.com/MichaelFlint
