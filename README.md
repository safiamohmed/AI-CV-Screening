# AI CV Screening

An n8n automation that reads job applications, reads each CV (PDF or scanned, with OCR), scores it with AI against the job requirements, and emails the candidate and HR. Built by Safia.

**Live site:** https://safiamohmed.github.io/AI-CV-Screening/ (works only while the owner's n8n and ngrok are running)

## Workflows

| File | Trigger | What it does |
|---|---|---|
| `workflows/cv-intake-workflow.json` | Form | Validates the application, saves the CV to Google Drive and logs it to a Google Sheet |
| `workflows/cv-screening-workflow.json` | Every hour / manual | Reads new applications, scores each CV with AI, sends an invite, a review flag or a decline, and saves the result |

## Services needed

Google Sheets, Google Drive and Gmail (OAuth), an LLM provider (Groq or Gemini), Gemini for OCR, Docker and ngrok to run n8n publicly.

## How to import

1. In n8n choose **Import from File** for each JSON in `workflows/`.
2. Create your own credentials (Google, Groq, Gemini) and select them on every node.
3. Pick your own Google Sheet and Drive folder in the Sheets and Drive nodes.
4. Sheet columns: `name, email, position, motivation, cv_file_id, status, score, decision, notes`.
5. Replace the HR email address in the Gmail nodes.
6. Publish both workflows.

