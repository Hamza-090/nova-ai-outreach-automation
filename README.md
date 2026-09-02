
---

LinkedIn Lead Scraper & Outreach Automation

This one should be a little stronger because it is one of your more complex portfolio projects.

```markdown
# LinkedIn Lead Scraper & Outreach Automation

An end-to-end AI-powered lead generation and outreach automation platform built using **n8n, ScrapeGraphAI, Apify, Gemini, Gmail API, Google Sheets API, Webhooks, and Lovable**.

The system automates the process of finding LinkedIn leads, enriching them with email information, generating personalized outreach messages, creating Gmail drafts, and tracking the complete workflow in Google Sheets.

---

## Overview

The platform connects lead discovery, data enrichment, AI-generated outreach, email drafting, and human review into a single automated workflow.

### Main Workflow

```text
Lovable Frontend
      ↓
Search Leads Webhook
      ↓
Prepare Search Filters
      ↓
LinkedIn Lead Search
      ↓
Parse Lead Results
      ↓
Filter / Validate Leads
      ↓
Email Enrichment
      ↓
 ┌───────────────┐
 │ Email Found?  │
 └───────┬───────┘
      Yes│       │No
         ↓       ↓
  Generate AI   Mark Email
     Email      Not Found
         ↓       ↓
      Gmail Draft
         ↓
      Google Sheets
         ↓
    Lead Management
