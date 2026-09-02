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
```
Key Features
Searches for LinkedIn prospects based on user-defined filters.
Supports job title, industry, location, keywords, and lead count.
Supports broad geographic searches including worldwide searches.
Uses ScrapeGraphAI to discover relevant LinkedIn profiles.
Validates and filters returned lead data.
Uses Apify for profile enrichment and email lookup.
Generates personalized outreach emails using Gemini.
Creates Gmail drafts instead of automatically sending messages.
Stores lead and outreach information in Google Sheets.
Supports leads where an email address cannot be found.
Includes a human-in-the-loop review system.
Supports Edit, Send, and Cancel actions for Gmail drafts.
Keeps Gmail and Google Sheets status synchronized.
Provides a frontend interface for searching leads and managing drafts.
Technologies Used
n8n – Workflow orchestration
Lovable – Frontend application
ScrapeGraphAI – Lead discovery / search
Apify – LinkedIn profile and email enrichment
Gemini – AI email generation
Gmail API – Draft creation, editing, sending, and deletion
Google Sheets API – Lead and workflow tracking
Webhooks – Frontend-to-n8n communication
JavaScript – Data transformation and workflow logic
Lead Search Workflow
1. Search Request

The frontend sends a search request to the n8n webhook.

Example fields include:

Job Title
Industry
Location
Keywords
Number of Leads

The frontend does not run the search until the required inputs are provided.

2. Search Filter Preparation

n8n converts the frontend request into a structured search configuration.

The filters are then used to construct the LinkedIn search query.

3. Lead Discovery

ScrapeGraphAI is used to discover relevant LinkedIn profiles.

The workflow searches for LinkedIn profile pages based on the supplied criteria.

Returned information can include:

Name
Job title
Company
Location
About
LinkedIn URL
Public profile information
4. Lead Parsing

The ScrapeGraphAI response is converted into structured lead records.

This makes the output easier to process in later nodes.

5. Lead Filtering

The workflow performs an additional validation step to remove incompatible results.

The filtering layer helps ensure that returned profiles match the requested search criteria without relying entirely on the scraper's output.

Broad location searches such as Worldwide are treated as global searches rather than requiring the word "Worldwide" to appear in a profile.

Email Enrichment

After valid leads are identified, their LinkedIn URLs are passed to the email enrichment stage.

Apify is used to retrieve profile information and search for available email addresses.

Each lead follows one of two paths:

Lead
 ↓
Email Found?
 ├── Yes → AI Outreach Generation
 └── No  → Email Not Found

This prevents the workflow from failing when an email address cannot be discovered.

AI Email Generation

For leads with available email addresses, Gemini generates the outreach email.

The AI receives the available lead information and generates:

Subject
Body

The prompt is designed to keep the generated content grounded in the available lead information and avoid inventing unsupported company or personal details.

Gmail Draft Creation

The generated message is added to Gmail as a draft rather than being automatically sent.

This provides a safety layer and allows the user to review the message before contacting the lead.

Google Sheets Tracking

Lead information and email workflow information are stored in Google Sheets.

Tracked fields include:

Name
Title
Email
Company
Location
LinkedIn URL
Subject
Body
Gmail Draft ID
Status
Drafted At
Sent At
Cancelled At

The Gmail Draft ID is used as the stable identifier for draft-related actions.

Human-in-the-Loop Workflow

The system does not force automatic email delivery.

Users can manage generated drafts through three actions:

Edit
Send
Cancel
Edit

The user modifies the subject or body.

The Gmail draft is updated and the corresponding Google Sheets row is marked:

Edited
Send

The Gmail draft is sent through the Gmail API.

The corresponding Google Sheets record is updated to:

Sent

and the send timestamp is recorded.

Cancel

The Gmail draft is deleted.

The Google Sheets record is updated to:

Cancelled

and the cancellation timestamp is recorded.

Frontend Integration

The frontend is built using Lovable and communicates with n8n through webhooks.

The interface provides:

Lead search
Search filters
Lead results
Draft management
Email editing
Send action
Cancel action
Workflow status

The frontend refreshes data after successful draft actions so that the displayed state remains synchronized with Gmail and Google Sheets.

Workflow Architecture
                    ┌──────────────────┐
                    │  Lovable Frontend │
                    └────────┬─────────┘
                             │
                             ↓
                    ┌──────────────────┐
                    │   n8n Webhook    │
                    └────────┬─────────┘
                             │
                             ↓
                    ┌──────────────────┐
                    │ ScrapeGraphAI    │
                    │ Lead Discovery   │
                    └────────┬─────────┘
                             │
                             ↓
                    ┌──────────────────┐
                    │ Lead Filtering   │
                    └────────┬─────────┘
                             │
                             ↓
                    ┌──────────────────┐
                    │ Apify Enrichment │
                    └────────┬─────────┘
                             │
                    ┌────────┴─────────┐
                    │                  │
                 Email Found        No Email
                    │                  │
                    ↓                  ↓
             ┌──────────────┐   ┌───────────────┐
             │    Gemini    │   │ Email Missing │
             │ AI Email Gen │   │   Handling     │
             └──────┬───────┘   └───────┬───────┘
                    │                   │
                    ↓                   ↓
             ┌──────────────┐    ┌──────────────┐
             │ Gmail Draft  │    │ Google Sheet │
             └──────┬───────┘    └──────────────┘
                    │
                    ↓
             ┌──────────────┐
             │ Google Sheet │
             └──────┬───────┘
                    │
                    ↓
             Human Review
              /    |    \
           Edit   Send  Cancel
Draft Management Workflow

Draft actions are handled through a separate n8n webhook.

Frontend
   ↓
Draft Action Webhook
   ↓
Find Draft
   ↓
Check Action
   ├── Edit
   ├── Send
   └── Cancel

Each action updates both Gmail and Google Sheets so the two systems remain synchronized.

Important Design Decisions
Draft Instead of Automatic Sending

Emails are created as Gmail drafts first.

This prevents accidental outreach and introduces a human approval layer before communication is sent.

Stable Draft Identification

The Gmail Draft ID is used to identify a specific draft across:

Gmail
Google Sheets
Frontend actions
Edit / Send / Cancel operations
Separate Email / No-Email Paths

Leads without an available email are not discarded silently.

They are recorded separately with an appropriate status.

Merge of Search Results

Both email-found and email-not-found leads are merged before the final webhook response so the frontend receives one consistent result set.

Setup
Required Services

The workflow requires accounts / credentials for the services used in the project:

n8n
ScrapeGraphAI
Apify
Gemini
Gmail
Google Sheets
Lovable
Importing the Workflow
Open n8n.
Import the provided .json workflow.
Configure the required credentials.
Verify API endpoints and webhook URLs.
Test the workflow with a small lead count.
Connect the frontend to the production webhook URL.
Activate the workflow.
Production Notes

For production use, the frontend should call the active n8n webhook endpoint rather than the temporary test webhook.

Example:

/webhook/search-leads

instead of:

/webhook-test/search-leads
Security

Never commit live credentials to GitHub.

Before publishing this workflow:

Remove API keys from exported JSON.
Remove bearer tokens.
Remove OAuth secrets.
Use n8n credentials for authentication.
Rotate any credentials that were previously exposed.

The workflow JSON stored in this repository should be a sanitized export.

Project Goal

This project demonstrates an end-to-end AI automation system that combines:

Web-based lead discovery
Profile enrichment
Generative AI
Email automation
Google APIs
Workflow orchestration
Human-in-the-loop review
Frontend and backend integration
