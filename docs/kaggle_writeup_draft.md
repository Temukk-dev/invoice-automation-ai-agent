# Invoice Automation AI Agent

## Subtitle

A multimodal financial document processing agent for invoice extraction, validation, risk detection, and business decision routing.

## 1. Solution Summary

This project develops an AI agent for automated invoice processing. The agent reads invoice images and PDF files, extracts structured financial fields, validates the extracted information against master data, detects suspicious invoice conditions, classifies invoice categories, and produces a final business decision.

The system is designed to go beyond simple OCR. It combines multimodal document understanding, structured database validation, business rule checks, and aggregate analysis. For each invoice, the final output includes extracted fields, category, risk flags, final decision, and a short explanation.

The final decisions are:

- `AUTO_POST`: valid invoice that can be posted automatically
- `HUMAN_APPROVAL`: invoice that needs manual review
- `DENY`: invoice with serious validation failure or suspicious risk

## 2. Model Architecture and Agent Workflow

The workflow contains seven main stages:

1. Load invoice files and master database.
2. Convert PDF invoices into image format when necessary.
3. Use a vision-language model to extract invoice fields into JSON.
4. Normalize text, numbers, dates, vendor names, and item descriptions.
5. Validate extracted fields against vendor, bank, item, and historical invoice data.
6. Detect risk flags and classify invoices into financial categories.
7. Generate final decisions and aggregate Q&A summaries.

The extraction layer uses a vision model through the Groq API. The validation layer uses deterministic Python rules so that decisions remain transparent and explainable.

## 3. Data Processing and Validation Strategy

The pipeline processes `.jpg`, `.jpeg`, `.png`, and `.pdf` invoice files. PDFs are converted to images before extraction. The expected extracted fields include invoice number, vendor name, invoice date, due date, bank account, item description, quantity, unit price, and total amount.

The validation layer checks the following risk types:

- `AMOUNT_MISMATCH`: quantity multiplied by unit price does not match the reported total
- `UNREGISTERED_VENDOR`: vendor is not found in the master vendor table
- `INVALID_DATE`: date is missing, invalid, or logically incorrect
- `BANK_ACCOUNT_MISMATCH`: extracted account does not match vendor master data
- `DUPLICATE`: invoice is similar to an existing invoice in the historical database

Each invoice receives a list of risk flags and an explanation. Serious risks such as duplicate invoices, bank account mismatch, invalid dates, and amount mismatch lead to `DENY`. New or uncertain invoices are routed to `HUMAN_APPROVAL`. Clean invoices are assigned `AUTO_POST`.

## 4. Evaluation Results

The notebook exports processed results into CSV files and prints summary statistics. The aggregate Q&A section answers questions such as how many invoices were denied, how many required human approval, how many had duplicate risk, and how many had unregistered vendors.

This makes the solution useful not only for invoice-level prediction, but also for business-level analysis over a batch of processed invoices.

## 5. Conclusions, Limitations, and Future Work

The solution demonstrates an end-to-end invoice automation agent that can read multimodal invoice files, validate them against structured data, detect financial risks, and make explainable routing decisions.

Current limitations include dependency on image quality and the need for stronger handling of unusual invoice layouts. Future improvements include confidence scoring, embedding-based duplicate detection, a human approval dashboard, and a more advanced category classifier trained on historical invoices.

## 6. Links

- Public Kaggle Notebook: ADD_LINK_HERE
- Public GitHub Repository: ADD_LINK_HERE
- Public YouTube Video: ADD_LINK_HERE
