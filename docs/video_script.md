# 3-Minute Demo Video Script

## 0:00–0:20 — Problem

Hello. This project is called Invoice Automation AI Agent.

The goal is to automate invoice processing for finance teams. Instead of only reading invoice text, the agent extracts information, validates it against master data, detects suspicious risks, and makes a final business decision.

## 0:20–0:50 — Agent Workflow

The workflow has several steps.

First, the system receives invoice files in PDF, JPG, or PNG format.  
Second, a vision-language model extracts invoice fields into structured JSON.  
Third, the extracted data is checked against vendor, bank, item, and historical invoice databases.  
Finally, the system generates risk flags, classifies the invoice, and chooses one of three decisions: AUTO_POST, HUMAN_APPROVAL, or DENY.

## 0:50–1:35 — Notebook Demo

Here is the Kaggle notebook.

The notebook loads the competition dataset and the master database.  
Then it processes invoice files one by one.  
PDF files are converted into images, and image files are sent directly to the extraction function.

The extracted fields include invoice number, vendor name, date, bank account, item description, quantity, unit price, and total amount.

## 1:35–2:15 — Risk Detection Demo

After extraction, the validation layer checks for risks.

The system can detect amount mismatch, unregistered vendor, invalid date, bank account mismatch, and duplicate invoice.

Each invoice receives a risk flag list and a short explanation.  
For example, if the vendor is not registered or the bank account does not match the database, the invoice is not posted automatically.

## 2:15–2:40 — Final Decision and Q&A

The final output contains the business decision.

AUTO_POST means the invoice is clean.  
HUMAN_APPROVAL means it needs manual review.  
DENY means it has serious risk.

The notebook also answers aggregate questions such as how many invoices were denied, how many were duplicates, and how many had unregistered vendors.

## 2:40–3:00 — Conclusion

This project shows an end-to-end AI agent for invoice automation.  
It combines document understanding, structured data validation, risk detection, decision logic, and summary Q&A.

The public notebook and GitHub repository are attached in the Kaggle writeup.
