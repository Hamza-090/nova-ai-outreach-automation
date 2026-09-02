# Nova AI Lead Outreach Automation

An AI-powered lead discovery and outreach platform that automates
prospect discovery, profile enrichment, email generation, Gmail drafting,
and human-reviewed outreach.

## Architecture

Lead Search
↓
ScrapeGraphAI
↓
Lead Filtering
↓
Apify Email Enrichment
↓
Gemini Personalization
↓
Gmail Draft
↓
Google Sheets
↓
Human Review
├── Edit
├── Send
└── Cancel

## Tech Stack

- n8n
- Gemini
- ScrapeGraphAI
- Apify
- Gmail API
- Google Sheets API
- Webhooks
- React
- JavaScript
- Lovable

## Key Features

- LinkedIn lead discovery
- Email enrichment
- AI-generated personalized outreach
- Email-found / email-not-found handling
- Gmail draft creation
- Human approval workflow
- Draft editing
- Draft cancellation
- Draft sending
- Google Sheets status tracking
- Frontend/backend webhook integration

## Project Structure

- `src/` - Lovable/React frontend
- `n8n/` - sanitized n8n automation workflow
- `docs/` - architecture diagrams and screenshots
