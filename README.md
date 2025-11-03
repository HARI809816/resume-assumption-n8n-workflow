# resume-assumption-n8n-workflow

Resume Parser Automation Workflow (n8n)
This n8n workflow automatically processes incoming resumes sent via email, extracts key information using Google Gemini AI, and logs the parsed details into a Google Sheet while archiving the original file in Google Drive.

📌 Overview
Trigger: Monitors your Gmail INBOX for new emails every minute.
Filter: Only processes emails that contain at least one attachment with a .pdf, .docx, or .doc extension.
Processing:
Downloads the email and its attachments.
Uploads the original resume file to a Google Drive folder.
Extracts text from the resume:
PDFs → parsed via built-in PDF extractor.
DOC/DOCX → parsed via plain-text extractor.
Sends the extracted text to Google Gemini (models/gemini-2.5-flash) with a structured prompt to extract:
Full Name
Email Address
Phone Number (formatted as (+91) XXXXXXXXXX)
Skills (as a comma-separated list)
Output: Parsed data is appended or updated in a Google Sheet, using Email as the unique identifier to avoid duplicates.
🔧 Requirements
Credentials (configured in n8n):
Gmail OAuth2 – to read incoming emails and download attachments.
Google Drive OAuth2 – to upload and store resume files.
Google Sheets OAuth2 – to write parsed data.
Google Gemini (PaLM) API – for AI-powered resume parsing.
External Resources:
Google Sheet:
ID: 1WUyyc29MUyO-YswMQCYzN_EnxGd4oNSEqQGymgMHWtA
Tab: Sheet1
Columns: Name, Email, Number, Skills
Google Drive Folder:
ID: 1v5cNVzonziPZFoGqWL-OGSgsCf0LW0N4
(Named "Resume")
💡 Ensure all credentials and external resources are accessible to the n8n instance. 

🔄 Workflow Flow
graph LR
A[Gmail Trigger] --> B[Get a message]
B --> C{Has .pdf/.doc/.docx?}
C -->|Yes| D{Is PDF?}
D -->|Yes| E[Extract from File (PDF)]
D -->|No| F[Extract from File (DOC/DOCX)]
E --> G[Upload to Drive]
F --> G
E --> H[Message a model (Gemini)]
F --> H
H --> I[Edit Fields]
I --> J[Append/Update Google Sheet]
⚠️ Notes & Assumptions
Phone Formatting: Assumes Indian numbers; strips all non-digits and prefixes with (+91), keeping the last 10 digits.
Filename Handling: Only processes the first attachment (attachment_0). Additional attachments are ignored.
AI Output: Relies on Gemini returning valid JSON. Invalid responses may cause errors in the Edit Fields node.
Error Handling: Not explicitly implemented—consider adding error workflows for production use.
🛠️ Customization
To adapt this workflow:

Change the Gmail label or polling frequency in the Gmail Trigger.
Modify the Gemini prompt to extract different fields.
Adjust phone formatting logic for other countries.
Add duplicate detection beyond email (e.g., name + phone).
✅ Use Case
Ideal for HR teams or recruiters who receive resumes via email and want to:

Automatically archive submissions.
Build a structured candidate database in Google Sheets.
Reduce manual data entry.
