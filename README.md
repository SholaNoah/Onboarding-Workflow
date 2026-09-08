# New Starter Onboarding Bot

An n8n workflow that simulates how an MSP (Managed Service Provider) handles onboarding a new employee for a client — from initial request through to IT notification and kit-confirmation reminders. Built as a hands-on automation project during my IT placement at ExpressIT, using made-up sample data throughout to avoid any real client/employee privacy issues.

## What it does

**Onboarding flow:**
1. **Form Trigger** — collects new starter details: Name, Company Name, Role, Start Date, and Access Needed (multi-select: Email, VPN, Adobe CC, License, Hardware Installation, Software Development/Management, Cyber Security)
2. **Code node** — validates the submission, generates a `first.last` username and email address using a slugified version of the company name as the domain (e.g. "Webcom Networks" → `webcomnetworks.com`), and parses the start date
3. **Copy file** (Google Drive) — duplicates a branded onboarding checklist template
4. **Update a document** (Google Docs) — merges the new starter's details into the copied template using find-and-replace
5. **Download file** (Google Drive) — exports the merged document as a PDF
6. **Send a message** (Gmail, x2) — sends a welcome email to the new starter and a separate IT notification, both with the onboarding PDF attached
7. **Append row in sheet** (Google Sheets) — logs the new starter into an Onboarding Records tracking sheet

**Reminder flow (runs on a schedule):**
1. **Schedule Trigger** — runs periodically to check on upcoming starters
2. **Get row(s) in sheet** (Google Sheets) — reads the Onboarding Records sheet
3. **Code node** — filters for starters whose kit hasn't been confirmed and whose start date falls within a set reminder window (also catches anything overdue)
4. **Send a message** (Gmail) — fires a reminder email if any starters match

Kit confirmation is a deliberate manual step — someone ticks a "Kit Confirmed" checkbox directly in the tracking sheet, simulating the human-in-the-loop check that a real IT team would do before a reminder is needed.

## Tech / nodes used

n8n · Google Drive · Google Docs · Google Sheets · Gmail · Form Trigger · Schedule Trigger · Code (JavaScript)

## Setup notes

This is a portfolio export of a working workflow, not a plug-and-play template. To run it yourself:

- Import `onboarding-workflow.json` into your own n8n instance
- Reconnect the Google Drive, Google Docs, Google Sheets, and Gmail credentials to your own accounts (the original credential references have been stripped)
- Replace the placeholder document/sheet IDs (`YOUR_DOC_TEMPLATE_ID`, `YOUR_SHEET_ID`) with your own Google Doc template and tracking sheet
- Your Google Doc template should include merge fields matching the ones the workflow replaces: full name, role, company, start date, username, email, and access needed

## Why this project

Most of my other projects (ticket triage bot, phishing simulator) are reactive — responding to something that's already happened. This one is proactive setup work instead, and it gave me hands-on experience combining a form trigger with document generation and multi-step notifications in the same workflow.
