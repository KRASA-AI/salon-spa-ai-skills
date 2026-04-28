# Salons & Spas AI Skills

**Free, open-source AI prompts and workflows built for salons & spas professionals.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for salons & spas. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| AI Consent & Compliance Guardrails for Client Communications | Generate the four consent, disclosure, and authorization artifacts that a salon, spa, or med spa needs when AI is drafting, routing, or analyzing client communications: (1) an AI-disclosure clause for intake forms, (2) a HIPAA-compliant marketing authorization separating clinical records from marketing outreach, (3) a before/after photo authorization scoped per platform and revocable, and (4) a short booking-confirmation notice that meets EU AI Act Article 50 transparency for any salon that has EU clients. | ~90 min/policy update |
| Client Consultation Notes | Produce structured, searchable consultation notes that capture formulas, processing times, tool settings, client preferences, contraindications, and service history in a format any provider can pick up and deliver a consistent service from. | ~8 min/client |
| Compensation Model Calculator | Help a salon owner, day-spa owner, or independent stylist / therapist / esthetician model the financial outcome of switching between (or running side-by-side) the four canonical compensation structures used in the industry: **commission split**, **hourly + commission**, **booth / chair rental**, and **suite rental**. | ~90 min/decision |
| Staff Training Guide Builder | Create structured, step-by-step training guides for salon and spa staff covering new techniques, product knowledge, service protocols, safety procedures, and client interaction standards. | ~30 min/guide |
| Waitlist Gap-Fill Outreach Generator | Turn a last-minute cancellation, no-show, or unbooked stretch into a filled chair by generating short, time-sensitive outreach to the right waitlisted clients. | ~10 min per gap |
| Weekly KPI Owner Briefing | Turn a raw weekly KPI export from the salon, spa, or med-spa management platform (Zenoti, Mangomint, Boulevard, Meevo, Vagaro, Mindbody, Booksy, Fresha, etc.) into a 1-page owner briefing the user can read in under five minutes Monday morning. | ~45 min/week |
| Retail Product Recommender | Generate personalized take-home product recommendations based on the service just performed, the client's hair or skin type, their lifestyle, and their budget comfort level. | ~5 min/client |
| SMS & WhatsApp Marketing Campaign Builder | Create targeted, concise SMS or WhatsApp marketing messages for salon and spa promotions. | ~20 min/campaign |
| Salon & Spa Loyalty Program Builder | Design a loyalty program tailored to a salon or spa's specific client base, service mix, and retention goals. | ~45 min/program design |
| Salon & Spa Referral Program Builder | Design a two-sided referral program that converts existing clients into brand advocates and new-client acquisition channels. | ~30 min/program design |
| Social Media Caption Writer | Generate platform-optimized, on-brand captions for salon and spa social media posts — including before/after transformations, promotions, educational content, team spotlights, and seasonal campaigns. | ~10 min/post |
| Social Media Content Calendar & Strategy Planner | Build a complete monthly social media content calendar for a salon or spa — mapping content pillars, post types, platform assignments, cadence, and seasonal hooks into a ready-to-execute schedule. | ~60 min/monthly calendar |
| Booking Confirmation Sequence | Produce the full multi-touch messaging sequence that turns a new booking into a confirmed, well-prepared, on-time client: the instant confirmation, the prep instructions, the reminder, and the day-of welcome. | ~10 min/booking |
| Client Win-Back & No-Show Recovery Sequence | Generate personalized re-engagement message sequences for lapsed clients (no visit in 60–120+ days) and clients who missed recent appointments. | ~15 min/campaign |
| Med Spa Treatment-Cadence Rebooking Reminders | Generate treatment-specific rebooking reminders timed to the clinical cadence of each aesthetic service — Botox at 10–14 weeks, filler at 6–12 months, chemical peel series at 3–6 weeks, laser hair removal at 4–6 weeks, HydraFacial at 4 weeks, CoolSculpting follow-up at 8–12 weeks. | ~20 min/cadence campaign |
| New Client Welcome & First-90-Day Retention Journey | Design the full multi-touch journey that turns a first-time booker into a rebooked, retained, and referring client across their first 90 days. | ~45 min/new-client cohort |
| Review Response Writer | Craft public-facing responses to Google, Yelp, Facebook, Fresha, and booking-platform reviews that (1) thank and reinforce happy clients, (2) de-escalate negative reviews without being defensive or legally risky, (3) pull future readers toward the brand, and (4) prompt rebooking where appropriate. | ~8 min/review |
| Service Recovery Writer (Direct Complaint & Refund Response Drafter) | Draft the private, direct response a salon, day spa, or med spa sends when a client complains, requests a refund, or signals they are unhappy through a non-public channel — email, DM, post-visit reply, voicemail follow-up, or a 1-star private message that has not yet become a public review. | ~25 min/incident |
| Virtual Consultation Intake Builder | Generate a structured pre-visit digital consultation questionnaire and produce a stylist/therapist prep summary from the client's responses. | ~10 min/client |
| Quick Review Reply (Salon & Spa) | Generate a single, on-brand reply to an online review in under 30 seconds. | ~5 min/reply |
| Salon & Spa Email Drafter | Turn rough notes into a polished, salon-or-spa-appropriate email that matches the business's voice, respects the client relationship, and is ready to send. | ~15 min/email |
| Salon & Spa Meeting Summarizer | Turn raw meeting notes, transcript, or bullet points from a salon, day-spa, or med-spa meeting into a structured summary that a busy owner or lead actually reads — separating decisions from action items from open questions, and surfacing the few metrics that matter for this meeting type. | ~20 min/meeting |

**Total time saved per use: ~616+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/salon-spa-ai-skills.git
cd salon-spa-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
salon-spa-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Salons & Spas AI guide**: [krasa.ai/industries/salons-spas](https://krasa.ai/industries/salons-spas)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
