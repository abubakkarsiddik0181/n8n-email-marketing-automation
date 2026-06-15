# Automated Email Marketing System with Follow-Up — n8n

A cold email outreach system built with n8n that sends personalized emails to leads from a Google Sheet and automatically follows up after 3 days.

---

## What It Does

- Reads leads (Name + Email) from a Google Sheet
- Sends personalized cold emails automatically
- Tracks which leads have been contacted (Status: Sent)
- After 3 days, sends a follow-up email to non-responding leads
- Updates the sheet with follow-up status (Follow Up: Done)
- Avoids duplicate emails using status filters

---

## Tech Stack

- **n8n** — Workflow automation
- **Gmail** — Sending emails via OAuth2
- **Google Sheets** — Lead database and status tracking
- **JavaScript (Code Node)** — Date calculation for follow-up timing

---

## Workflow Overview

### Part 1 — Initial Outreach (`part1_initial_outreach.json`)

```
Schedule Trigger → Google Sheets (Read) → Filter (not sent) → Loop → Gmail (Send) → Sheets (Update Status)
```

- Runs every 6 hours
- Filters out already-contacted leads
- Sends email and marks Status as "Sent"
- Records the sending timestamp

### Part 2 — Follow-Up (`part2_followup.json`)

```
Schedule Trigger → Date & Time → JS Code (subtract 3 days) → Google Sheets (Read Sent) → Filter (no follow-up + 3 days passed) → Loop → Gmail (Send) → Sheets (Update Follow Up: Done)
```

- Runs every 6 hours
- Only targets leads with Status "Sent" but Follow Up not "Done"
- Checks if 3 days have passed since the initial email
- Sends follow-up and marks "Follow Up: Done"

---

## Google Sheet Structure

| Name | Gmail | Status | Sending Time | Follow Up |
|------|-------|--------|--------------|-----------|
| John Doe | john@example.com | | | |

---

## Setup Instructions

### 1. Import Workflows
- Open your n8n instance
- Go to Workflows → Import
- Import `part1_initial_outreach.json`
- Import `part2_followup.json`

### 2. Connect Credentials
- Gmail OAuth2 — Connect your Gmail account in both workflows
- Google Sheets OAuth2 — Connect your Google account

### 3. Set Up Google Sheet
- Create a Google Sheet with these columns: `Name`, `Gmail`, `Status`, `Sending Time`, `Follow Up`
- Add your leads (Name + Gmail only, leave the rest blank)

### 4. Update Sheet ID
- In both workflows, open the Google Sheets nodes
- Replace `YOUR_GOOGLE_SHEET_ID` with your actual Sheet ID
- Found in the Sheet URL: `docs.google.com/spreadsheets/d/YOUR_ID_HERE`

### 5. Write Your Email
- In both workflows, open the "Send a message" Gmail node
- Write your Subject and Email Body

### 6. Activate
- Activate Part 1 first
- Activate Part 2 after Part 1 has run at least once

---

## Notes

- Part 2 only sends follow-up if 3 full days have passed since the initial email
- The system uses the `Sending Time` column to calculate the follow-up window
- Both workflows are safe to run simultaneously after initial setup

---

## Author

**Abu Bakkar Siddik**

