# Invoice Automation AI Agent

## 1. Problem

This project was built for **AI Legends 2026 (AI agents automation)**.  
The goal is to create an AI agent that can process invoice images and PDF files, extract financial information, validate the invoice against structured master data, detect risks, classify the invoice, and make a final business decision.

The system is not only an OCR pipeline. It combines:

- multimodal invoice understanding
- structured database validation
- rule-based financial checks
- risk flagging
- final business routing
- aggregate Q&A over processed invoices

## 2. Solution Overview

The agent processes invoice files in the following flow:

```text
PDF / JPG / PNG Invoice
        ↓
Vision-based Field Extraction
        ↓
Structured JSON Parsing
        ↓
Vendor / Bank / Historical Invoice Validation
        ↓
Amount / Date / Duplicate Checks
        ↓
Category Classification
        ↓
Final Decision Logic
        ↓
CSV Export + Aggregate Q&A
```

## 3. Agent Workflow

### Step 1 — Input Loading

The notebook loads:

- invoice image files (`.jpg`, `.jpeg`, `.png`)
- invoice PDF files (`.pdf`)
- master invoice database (`master_invoices_database.db`)

### Step 2 — Vision Extraction

A vision-language model extracts structured fields from each invoice.

Main extracted fields:

```text
invoice_number
invoice_date
due_date
vendor_name
bank_name
account_number
item_description
quantity
unit_price
subtotal
tax
total_amount
currency
```

### Step 3 — Data Normalization

The extracted data is normalized before validation:

- text normalization
- number cleanup
- date parsing
- vendor name correction
- item/category matching

### Step 4 — Validation

The system checks each invoice against business rules and historical data.

Detected risk types:

```text
AMOUNT_MISMATCH
UNREGISTERED_VENDOR
INVALID_DATE
BANK_ACCOUNT_MISMATCH
DUPLICATE
MISSING_REQUIRED_FIELD
LOW_CONFIDENCE_EXTRACTION
```

### Step 5 — Final Decision

Each invoice receives one of three final decisions:

| Decision | Meaning |
|---|---|
| `AUTO_POST` | The invoice is valid and can be posted automatically |
| `HUMAN_APPROVAL` | The invoice is new or uncertain and needs manual review |
| `DENY` | The invoice has serious risk or rule violations |

## 4. Decision Logic

```text
DENY:
- duplicate invoice
- amount mismatch
- bank account mismatch
- invalid date

HUMAN_APPROVAL:
- unregistered vendor
- missing required fields
- low-confidence extraction

AUTO_POST:
- registered vendor
- valid amount
- valid date
- valid bank account
- not duplicate
```

## 5. Output Files

The notebook exports:

```text
final_invoice_results.csv
suspicious_invoice_results.csv
clean_invoice_results.csv
failed_invoice_files.csv
```

Important output columns:

```text
source_file
invoice_number
vendor_name
invoice_date
due_date
total_amount
category
risk_flags
final_decision
explanation
processing_status
```

## 6. Aggregate Q&A

The notebook answers summary questions such as:

- How many invoices were denied?
- How many invoices required human approval?
- How many invoices were duplicates?
- How many invoices had unregistered vendors?
- Which category appeared most often?

## 7. How to Run

### On Kaggle

1. Open the public Kaggle notebook.
2. Add the competition dataset.
3. Add your Groq API key using Kaggle Secrets.
4. Use the secret name:

```text
GROQ_API_KEY
```

5. Run all cells.
6. Download the exported CSV files from `/kaggle/working`.

### Local Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Then update the dataset path in the notebook.

## 8. Requirements

```text
pandas
pymupdf
pillow
groq
python-dateutil
```

## 9. Limitations

- Extraction quality depends on invoice image clarity.
- Some fields may be difficult to extract from low-resolution scans.
- Rule-based validation may need adjustment for real ERP systems.
- The current pipeline is optimized for the competition dataset structure.

## 10. Future Improvements

- Add confidence scoring per extracted field.
- Add stronger duplicate detection using embeddings.
- Add a web dashboard for invoice review.
- Add human-in-the-loop approval UI.
- Add more advanced category classification using historical examples.

## 11. Competition Submission Assets

This repository is part of the final submission package:

- Public Kaggle Notebook
- Kaggle Writeup
- Public YouTube Demo Video
- Public GitHub Project Link
