---
tags: [requirements, specs]
---
# Functional Specs — WaCRM

## Core Features

### Contact Management
- Add contacts: name, phone (WA number), tags (unlimited paid / 3 free), notes
- Contact card: history log, last interaction, status, WhatsApp link
- Search and filter contacts by tag, status, last contact date

### Follow-Up Reminders
- Set reminder per contact: "follow up in X days" or specific date/time
- Notification: in-app + email when reminder fires
- Most valuable single feature: prevents sales loss from forgetting

### Customer History
- Per-contact log: notes, interaction timestamps, status changes
- All manual at MVP (no WhatsApp message sync)

### Team Features (Team + Business tiers)
- Shared contact pool
- Assign contacts to team members
- Conversation handoff with notes
- Activity feed

### Pipeline (Business tier)
- Custom pipeline stages (e.g., Lead → Contacted → Proposal → Closed)
- Kanban view of contacts per stage
- Analytics: response time, conversion rate, CSV export

## User Flow — Core
1. Customer messages on WhatsApp → owner opens WaCRM
2. Find or create contact → view history
3. Interact on WhatsApp → come back to WaCRM → log note + update status
4. Set follow-up reminder → close app
5. Reminder fires → owner opens WhatsApp → contacts customer

## Key Design Principle
WaCRM is never shown during the WhatsApp conversation — it's the before (context) and after (logging) layer. The chat happens in WhatsApp.
