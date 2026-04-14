# Salons & Spas AI Skills

**Free, open-source AI prompts and workflows built for salons & spas professionals.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for salons & spas. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| Client Consultation Notes | Produce structured, searchable consultation notes that capture formulas, processing times, tool settings, client preferences, contraindications, and service history in a format any provider can pick up and deliver a consistent service from. | ~8 min/client |
| Staff Training Guide Builder | Create structured, step-by-step training guides for salon and spa staff covering new techniques, product knowledge, service protocols, safety procedures, and client interaction standards. | ~30 min/guide |
| Retail Product Recommender | Generate personalized take-home product recommendations based on the service just performed, the client's hair or skin type, their lifestyle, and their budget comfort level. | ~5 min/client |
| SMS & WhatsApp Marketing Campaign Builder | Create targeted, concise SMS or WhatsApp marketing messages for salon and spa promotions. | ~20 min/campaign |
| Salon & Spa Loyalty Program Builder | Design a loyalty program tailored to a salon or spa's specific client base, service mix, and retention goals. | ~45 min/program design |
| Salon & Spa Referral Program Builder | Design a two-sided referral program that converts existing clients into brand advocates and new-client acquisition channels. | ~30 min/program design |
| Social Media Caption Writer | Generate platform-optimized, on-brand captions for salon and spa social media posts — including before/after transformations, promotions, educational content, team spotlights, and seasonal campaigns. | ~10 min/post |
| Booking Confirmation Sequence | Produce the full multi-touch messaging sequence that turns a new booking into a confirmed, well-prepared, on-time client: the instant confirmation, the prep instructions, the reminder, and the day-of welcome. | ~10 min/booking |
| Client Win-Back & No-Show Recovery Sequence | Generate personalized re-engagement message sequences for lapsed clients (no visit in 60-90+ days) and clients who missed recent appointments. | ~15 min/campaign |
| Review Response Writer | Craft public-facing responses to Google, Yelp, Facebook, Fresha, and booking-platform reviews that (1) thank and reinforce happy clients, (2) de-escalate negative reviews without being defensive or legally risky, (3) pull future readers toward the brand, and (4) prompt rebooking where appropriate. | ~8 min/review |
| Virtual Consultation Intake Builder | Generate a structured pre-visit digital consultation questionnaire and produce a stylist/therapist prep summary from the client's responses. | ~10 min/client |
| Email Drafter | Turn rough notes into a professional email matching your company's voice and tone. | ~10 min/use |
| Meeting Summarizer | Summarize meeting notes into action items, decisions, and follow-ups. | ~10 min/use |
| Review Responder | Craft professional responses to online reviews — both positive and negative. | ~10 min/use |

**Total time saved per use: ~221+ minutes across all skills.**

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
