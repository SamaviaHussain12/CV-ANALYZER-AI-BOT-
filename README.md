# AI CV Analyzer Bot

An automated CV/Resume analysis bot built with **n8n** and **OpenAI GPT-4o-mini**. Upload a CV through a simple web form, and receive a detailed AI-powered analysis report directly in your email.

## How It Works

```
Open Form  →  Upload CV (PDF)  →  AI Analyzes  →  Report Sent to Email
```

1. Open the n8n form URL in your browser
2. Enter candidate name, your email, and optionally a target job position
3. Upload the CV as a PDF
4. Submit — the AI analyzes the CV in ~30-60 seconds
5. A comprehensive analysis report arrives in your inbox

## What the AI Analyzes

| Category | Details |
|----------|---------|
| Candidate Overview | Name, contact info, location, LinkedIn |
| Skills Analysis | Technical skills, soft skills, tools, languages, certifications |
| Experience Summary | Years of experience, key roles, achievements, industries |
| Education | Degrees, institutions, certifications |
| Top 5 Strengths | With evidence from the CV |
| Areas for Improvement | With actionable suggestions |
| Best Fit Job Role | AI-suggested role with reasoning |
| Job Fit Scores | 1-10 ratings across 7 career paths |
| Overall CV Score | Out of 100 |
| Hiring Recommendation | Strong Hire / Hire / Maybe / Pass |
| CV Improvement Tips | 5 specific suggestions |
| Executive Summary | Final professional assessment |

## Workflow Architecture

```
┌──────────────┐    ┌───────────────┐    ┌──────────────┐    ┌─────────────────────┐
│ CV Upload    │───>│  Normalize    │───>│ Extract PDF  │───>│ Prepare Analysis    │
│ Form         │    │  Upload       │    │ Text         │    │ Prompt              │
│ (Form Trigger│    │  (Code)       │    │ (PDF Parser) │    │ (Code)              │
└──────────────┘    └───────────────┘    └──────────────┘    └──────────┬──────────┘
                                                                        │
                                                                        v
┌──────────────┐    ┌───────────────┐    ┌──────────────────────────────────────────┐
│ Send via     │<───│  Format       │<───│ Analyze with OpenAI                      │
│ Gmail        │    │  Response     │    │ (HTTP Request → GPT-4o-mini)             │
│              │    │  (Code)       │    │                                          │
└──────────────┘    └───────────────┘    └──────────────────────────────────────────┘
```

**7 nodes** — fully pre-configured and ready to import.

## Tech Stack

- **n8n** — Workflow automation (self-hosted or cloud)
- **OpenAI GPT-4o-mini** — CV analysis engine
- **Gmail API** — Sends analysis results via email

## Quick Start

### Prerequisites

- An n8n instance ([cloud](https://n8n.io) or [self-hosted](https://docs.n8n.io/hosting/))
- An OpenAI API key ([get one here](https://platform.openai.com/api-keys))
- A Gmail account with OAuth2 set up via Google Cloud Console

### Setup

1. **Import** `cv-analyzer-workflow.json` into n8n (three-dot menu → Import from File)
2. **Add OpenAI credential** — Create a "Header Auth" credential in n8n:
   - Header Name: `Authorization`
   - Header Value: `Bearer sk-your-api-key-here`
3. **Add Gmail credential** — Create a "Gmail OAuth2 API" credential with your Google Cloud OAuth2 Client ID and Secret
4. **Connect credentials** to the "Analyze with OpenAI" and "Send via Gmail" nodes
5. **Activate** the workflow
6. **Open** `https://your-n8n/form/cv-analyzer` and start analyzing CVs!

> See [SETUP-GUIDE.md](SETUP-GUIDE.md) for detailed step-by-step instructions with screenshots descriptions.

## Project Files

| File | Description |
|------|-------------|
| `cv-analyzer-workflow.json` | Complete n8n workflow — import directly into n8n |
| `SETUP-GUIDE.md` | Detailed setup guide with step-by-step instructions |
| `README.md` | This file |

## Cost

| Component | Cost |
|-----------|------|
| n8n (free tier or self-hosted) | Free |
| OpenAI per CV analysis | ~$0.002–$0.005 |
| Gmail API | Free |
| **Total per CV** | **< $0.01** |

## Customization

- **AI Model** — Change `gpt-4o-mini` to `gpt-4o` or `gpt-3.5-turbo` in the HTTP Request node
- **Form Fields** — Add or remove fields in the Form Trigger node
- **Email Template** — Edit the "Format Response" code node
- **Fixed Recipient** — Hardcode an email in the Gmail node instead of using the form field

## License

This project is open source and free to use.
