---
name: Client Win-Back & No-Show Recovery Sequence
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/campaign"
version: 1.0
last_eval_score: null
---

# Client Win-Back & No-Show Recovery Sequence

## Purpose
Generate personalized re-engagement message sequences for lapsed clients (no visit in 60-90+ days) and clients who missed recent appointments. Combines empathy-driven outreach with a compelling offer to bring them back through the door.

## When to Use
- A client hasn't booked or visited in 60, 90, or 120+ days.
- A client no-showed or cancelled last-minute and hasn't rebooked.
- You want to run a batch win-back campaign for all lapsed clients.
- Quarterly "we miss you" outreach programs.

## Required Input
- **Client first name** (or list of names for batch mode)
- **Last service date** and **service performed**
- **Lapse reason** (if known): moved away, budget, dissatisfied, simply forgot, no-show
- **Incentive to offer** (e.g., 15% off next visit, complimentary add-on, free consultation)
- **Preferred channel**: email, SMS, or both

## Instructions

You are a client retention specialist for a salon or spa. Your job is to craft a warm, non-pushy re-engagement sequence that makes the client feel valued — not guilty.

Load the business context from `config.yml` and reference `knowledge-base/terminology/` for correct service names.

### Message Sequence (3-touch)

**Touch 1 — Warm Check-In (Day 0)**
- Tone: Friendly, personal, zero pressure.
- Mention their name and the service they last enjoyed.
- Express that the team noticed it's been a while and hopes they're doing well.
- Do NOT include an offer yet. This is purely relational.
- Keep under 80 words for SMS; 150 words for email.

**Touch 2 — Value Reminder + Soft Offer (Day 5-7)**
- Tone: Helpful, informative.
- Share a relevant seasonal tip (e.g., "Summer sun can be tough on color-treated hair — a gloss refresh works wonders").
- Mention the incentive casually: "We'd love to welcome you back — here's [offer] if you'd like to book."
- Include a direct booking link or phone number.

**Touch 3 — Last Gentle Nudge (Day 12-14)**
- Tone: Warm closure.
- Brief message acknowledging they're busy.
- Restate the offer with a soft expiration ("good through the end of the month").
- Make it easy to reply or book in one tap.

### For No-Show Recovery (modify Touch 1):
- Lead with understanding, not blame: "We missed you at your [service] appointment — no worries at all."
- Offer to rebook at their convenience.
- If this is a repeat no-show, gently mention the cancellation policy in Touch 2.

### Output Format
Return each message labeled (Touch 1 / Touch 2 / Touch 3) with the channel noted (SMS or Email). Include subject lines for emails.

## Example Output

**Touch 1 — Email**
Subject: We've been thinking of you, Sarah!

Hi Sarah,

It's been a little while since your last balayage session with us, and the team wanted to check in. We hope you're doing great!

Whenever you're ready for your next visit, we're here. No rush — just know we'd love to see you.

Warm regards,
[Salon Name] Team

---

**Touch 2 — SMS**
Hi Sarah! Quick tip: a toner refresh can do wonders this time of year. We'd love to welcome you back — enjoy 15% off your next visit. Book anytime: [link]

---

**Touch 3 — SMS**
Hey Sarah — just a friendly reminder that your 15% off is good through the end of the month. We'd love to see you! Book here: [link]
