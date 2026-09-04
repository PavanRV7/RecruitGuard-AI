# RecruitGuard AI

RecruitGuard AI is a full-stack web app for job-scam detection, application tracking, and interview preparation.

## Project Structure

```text
RecruitGuard AI/
├── client/                 # Frontend (React + Vite)
│   └── src/
├── server/                 # Backend API (Express + TypeScript)
├── shared/                 # Shared route + schema contracts
├── docs/
│   └── requirements.md     # Product and feature requirements
├── script/                 # Utility scripts (build, llm checks, pdf extraction helper)
├── docker-compose.yml      # Local Postgres service
├── package.json
└── README.md
```

## Prerequisites

- Node.js 20+
- npm
- Optional: Docker Desktop (for PostgreSQL)
- Optional for advanced PDF extraction helper: Python 3 + pip packages

## Setup From Zip (Start to End)

1. Extract the zip file.
2. Open terminal in project root:
   `Grok-Recruit-Guard`
3. Install dependencies:
   `npm install`
4. Create environment file:
   copy `.env.example` to `.env`
5. Add required values in `.env`:
   `DATABASE_URL=...`
   `SESSION_SECRET=...`
   `GROQ_API_KEY=...`
   `GEMINI_API_KEY=...`
   `GROK_API_KEY=...`
6. Start PostgreSQL (optional but recommended):
   `docker compose up -d`
7. Push DB schema:
   `npm run db:push`
8. Start app:
   `npm run dev`
9. Open:
   `http://localhost:5000`

## Optional Python PDF Helper

If you want to run the standalone PDF extractor helper in `script/extract_pdf_text.py`:

1. Install Python dependencies:
   `pip install pypdf pillow pdf2image pytesseract`
2. Run:
   `python script/extract_pdf_text.py --file path/to/file.pdf`

## Health Checks

- Type check: `npm run check`
- LLM connectivity check: `npm run check:llm`

## HTTPS Deployment (Docker + Caddy)

This repo now includes:
- `Dockerfile` for the app image
- `docker-compose.prod.yml` for app + Caddy
- `Caddyfile` for automatic HTTPS (Let's Encrypt)
- `.env.production.example` template

### Requirements

- A VPS/server with Docker and Docker Compose
- A domain name pointing to your server public IP
- Ports `80` and `443` open in firewall/security group

### Steps

1. Copy production env template:
   `cp .env.production.example .env.production`
2. Edit `.env.production` and set:
   - `DOMAIN` to your real domain
   - `DATABASE_URL`, `SESSION_SECRET`, and API keys
3. Start stack:
   `docker compose --env-file .env.production -f docker-compose.prod.yml up -d --build`
4. Verify:
   - `https://your-domain.com`
   - SSL certificate is issued automatically by Caddy

### Notes

- In production, Express now trusts the proxy so secure session cookies work correctly behind HTTPS.
- If DNS is not pointed correctly or ports are blocked, certificate issuance will fail.
