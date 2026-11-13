# AI Recruitment Automation (n8n)

An end-to-end recruitment system built with n8n. Candidates apply with a PDF CV, an AI scores each application (with Gemini OCR for scanned PDFs and Groq models for scoring), and the right email is sent automatically. A separate workflow monitors failures.

**Live site:** https://safiamohmed.github.io/AI-CV-Screening/

## Workflows

| File | Trigger | What it does |
|---|---|---|
| `intake.json` | Form | Validates the application, blocks duplicates, saves the CV to Google Drive and logs it to a Google Sheet |
| `ai-scan.json` | Every hour + manual | Reads new applications, extracts the CV text (OCR fallback), builds a candidate profile, scores it, validates the result, then invites, flags for HR, or declines. Invalid AI output goes to manual review |
| `error-handler.json` | Error + schedules | Logs failures, emails alerts with throttling, sends a daily digest, and checks that the queue is not stuck |
| `hr-policy-rag-assistant.json` | Chat trigger | Public RAG chat that answers HR questions only from the policy documents (English and Arabic) |
| `policy-loader.json` | n8n start, daily, manual | Loads the policy Google Docs into the assistant's knowledge base |
| `status-ping.json` | Webhook (GET) | Tiny endpoint the website uses to show Online / Offline |

## Services

n8n (Docker), ngrok (static domain), Google Sheets, Drive and Gmail, Groq API, Google Gemini API (OCR and embeddings). All have free tiers.

## Import steps

1. Run n8n with a public address (`WEBHOOK_URL` set to your ngrok domain).
2. Import each JSON file: Workflows, Create workflow, then Import from File.
3. Create credentials in n8n: Google Sheets, Drive and Gmail OAuth2, Groq, Gemini. Open every node with a warning triangle and select the credential.
4. Replace every `YOUR_ID` with your own Sheet or Drive file, and every `you@example.com` / `YOUR_SENDTO` with your HR address.
5. In Recruitment 1 and 2, set **Settings, Error workflow** to Recruitment 3.
6. In `status-ping`, set **Allowed Origins** to `https://safiamohmed.github.io`.
7. RAG: put your policies as Google Docs in one Drive folder, set that folder in `policy-loader`, run **Reload Now** once, and in `hr-policy-rag-assistant` set the Chat Trigger **Allowed Origins** to your GitHub Pages domain. Keep the memory key `hr_policies` identical in both workflows.
8. Activate the workflows (error handler first), then copy the Production URLs of the apply form and the chat into `CONFIG` in `index.html`.

No keys, IDs or personal email addresses are included in this repository.
