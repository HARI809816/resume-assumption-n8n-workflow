# Resume Assumption / Resume Parser Automation (n8n Workflow)

This n8n workflow automatically processes incoming resumes from Gmail, extracts important candidate details using Google Gemini AI, logs structured data into Google Sheets, and archives all resumes into Google Drive.

---

## Overview

### Trigger
- Checks Gmail INBOX every minute.
- Processes only emails that contain at least one attachment with:
  - .pdf
  - .docx
  - .doc

---

## Workflow Steps

### 1. Email Processing
- Downloads the email.
- Reads all attachments.
- Filters required formats.

### 2. File Handling
- Uploads resume file to Google Drive (Resume folder).
- Extracts text depending on file type:
  - PDF → PDF text extractor
  - DOC/DOCX → plain text extractor

### 3. AI Processing (Google Gemini)
Extracted text is sent to Gemini (models/gemini-2.5-flash) to extract:
- Full Name  
- Email Address  
- Phone Number formatted as (+91) XXXXXXXXXX  
- Skills (comma-separated)

### 4. Data Storage
- Parsed data is added or updated in a Google Sheet.
- Uses Email as the unique key to avoid duplicates.

---

## External Resources Used

### Google Sheet
- ID: 1WUyyc29MUyO-YswMQCYzN_EnxGd4oNSEqQGymgMHWtA  
- Tab: Sheet1  
- Columns: Name, Email, Number, Skills  

### Google Drive Folder
- ID: 1v5cNVzonziPZFoGqWL-OGSgsCf0LW0N4  
- Folder Name: Resume

---

## Required Credentials (configured in n8n)

- Gmail OAuth2 (read emails, download attachments)  
- Google Drive OAuth2 (upload files)  
- Google Sheets OAuth2 (write data)  
- Google Gemini API (AI extraction)

---

## Workflow Flow Diagram

```mermaid
graph LR
A[Gmail Trigger] --> B[Get a Message]
B --> C{Has .pdf/.doc/.docx?}
C -->|Yes| D{Is PDF?}
D -->|Yes| E[Extract from File (PDF)]
D -->|No| F[Extract from File (DOC/DOCX)]
E --> G[Upload to Google Drive]
F --> G
E --> H[Message a Model (Gemini)]
F --> H
H --> I[Edit Fields]
I --> J[Append/Update Google Sheet]
```

---

## Notes and Assumptions

- Phone numbers are formatted using Indian standard:
  - Remove non-digits
  - Keep last 10 digits
  - Prefix with (+91)
- Only the first attachment (attachment_0) is processed.
- Assumes Gemini returns valid JSON.
- No error-handling nodes included.

---

## Customization

- Change Gmail trigger label or frequency.
- Modify Gemini prompt to extract more fields.
- Adjust phone formatting for other countries.
- Add duplicate detection logic.
- Add error handling.

---

## Ideal Use Case

Useful for:
- HR teams  
- Recruiters  
- Automated hiring systems  

Helps to:
- Automatically archive resumes
- Build structured candidate databases
- Reduce manual work

---

If you need, I can also generate:
- Full n8n JSON workflow  
- Gemini prompt  
- Error-handling workflow  
