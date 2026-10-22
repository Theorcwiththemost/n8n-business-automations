# n8n Business Automations

Production-style [n8n](https://n8n.io) workflows I built to run the back office of three service companies I operate — a commercial janitorial company (90+ staff, 1,000+ accounts), a remodeling contractor, and a commercial landscaping company. Each one replaces a task that used to be done by hand.

Import any `.json` from `/workflows` straight into n8n (**Workflows → Import from file**), add your credentials, set the environment variables below, and turn it on.

| # | Workflow | What it replaces | Stack |
|---|----------|------------------|-------|
| 01 | **Missed Call Text-Back + HubSpot** | Losing leads who hang up on voicemail. Texts them back in seconds, creates the contact, and queues a callback task. | Twilio, HubSpot |
| 02 | **Web Lead AI Qualifier** | Reading every form submission yourself. Claude scores 1–10, tags the service, and hot leads (7+) hit your phone by SMS; the rest go to an email digest. | Claude, HubSpot, Twilio, Gmail |
| 03 | **Overdue Invoice Chaser** | Chasing receivables by memory. Pulls open invoices daily, escalates friendly → second notice + SMS → owner task at 30 days. | QuickBooks, Gmail, Twilio, HubSpot |
| 04 | **Post-Job Google Review Request** | Forgetting to ask for reviews. 24h after job completion: thank-you + review link; one gentle follow-up 3 days later if nothing posted. | Twilio, Google Sheets |
| 05 | **Daily Government Bid Digest** | Scanning SAM.gov and BidNet every morning. Pulls both feeds, Claude ranks fit for your NAICS codes, emails a clean digest at 7am. | RSS, Claude, Gmail |

## Environment variables

Set these in n8n (**Settings → Variables**) or your `.env`:

```
BUSINESS_NAME=Eagle Janitorial
OWNER_PHONE=+1915…
OWNER_EMAIL=you@company.com
TWILIO_FROM=+1915…
GOOGLE_REVIEW_LINK=https://g.page/r/…/review
REVIEW_TRACKER_SHEET=<google sheet id>
SAM_RSS_URL=https://sam.gov/…
BIDNET_RSS_URL=https://www.bidnetdirect.com/…
```

## Credentials needed

- **Twilio** — Account SID + Auth Token
- **HubSpot** — Private app token (contacts + engagements scopes)
- **Anthropic** — API key (workflows 02, 05)
- **QuickBooks Online** — OAuth2 (workflow 03)
- **Gmail** — OAuth2 (03, 02, 05)
- **Google Sheets** — OAuth2 (04)

## Design notes

- **Fail-safe by default.** Every workflow that touches a customer has a time gate or a human task before anything aggressive happens (no SMS after hours, no collections without an owner task).
- **AI only where judgment is needed.** Claude scores leads and ranks bids — things a human would otherwise read. Everything deterministic stays deterministic.
- **Drop-in for any trade.** Swap `BUSINESS_NAME`, the NAICS codes in workflow 05, and the message copy; nothing else is company-specific.

## Adapting these for your business

I build and install these for small service businesses — usually live inside a week, including CRM and phone setup if you don't have one yet. Reach out via [Upwork](https://www.upwork.com/freelancers/~018988deca3d975193) or open an issue here.

---
*Built by Francisco Corral — operations lead across Eagle Janitorial, TMC Remodeling, and Little Man Landscaping. Bilingual EN/ES. Austin & El Paso, TX.*
