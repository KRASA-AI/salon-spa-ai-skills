# Salon & Spa AI Tools Ecosystem — Snapshot (May 2026)

This file is a working snapshot of where AI tooling sits in the salon, spa, and med-spa industry as of May 2026. It is not a recommendation list — it's a map of categories and representative players so the skills in this repo can stay grounded in what owners actually run.

## Category map

**Practice management with built-in AI (booking, scheduling, client records).** This is where most owners encounter AI first, because their existing software adds it. Representative platforms: Zenoti, Meevo, Booksy, Pabau, Insight, GlossGenius, Mindbody. Common AI features now bundled: predictive no-show flagging, smart waitlist that auto-texts when a slot opens, AI-generated review responses, AI daily briefs / digests for the owner.

**Standalone AI booking concierges.** Web widgets or phone agents that sit in front of the booking system and handle inbound questions, quote services, and book appointments 24/7. Pricing typically starts in the low-to-mid two figures per month for chat-only and climbs for voice. Common in med spas where after-hours inbound is heavy.

**Review and reputation automation.** Tools that watch Google Business Profile (and Yelp / Facebook) for new reviews, draft a response in the brand's voice, and either auto-publish positives or queue negatives for human review. The strong consensus across multiple sources is that negative review responses should never be auto-sent — the cost of one tone-deaf reply outweighs the time saved.

**Pre-appointment intake and consultation.** Form builders and chatbots (e.g., Jotform's beauty salon agent templates) that gather hair / skin / treatment history before the appointment so the provider walks in informed. Increasingly paired with AI image preview tools that render a proposed style or color on a client photo with a face-preservation prompt.

**Marketing content generation.** Caption generators, blog drafters, and email campaign tools tuned for the beauty vertical. Quality is uneven — these tools work best when fed the salon's own voice samples and service list rather than generic prompts.

**Upstream AI (formulation, safety assessment, virtual try-on).** Mostly relevant to product brands and large chains, not independent salons. Worth tracking because it shapes what clients walk in expecting ("I tried this color on the app, can you do it?").

## Themes worth designing around

The biggest near-term operational wins reported across the industry trade press are no-show reduction (reported reductions from ~25% down to under 5% when reminder cadence is tuned to client risk) and recovery of owner admin time (reports of 9 hours per week of admin work compressed to around 2 hours when daily briefs, automated confirmations, and waitlist fills are stacked together).

Personalization is moving from "use the client's first name" to "match cadence and channel to the individual client's history". A repeat weekly client and a first-time injectable booking should not get the same reminder sequence.

Trust and tone remain the constraint. Owners who have rolled out review-response automation report that 5-star and 4-star responses are safe to automate; anything lower needs a human hand. The same logic applies to deposit chasing and cancellation-fee outreach.

## Implications for this repo

The skills in `skills/operations/` and `skills/customer-service/` should assume the owner already has a practice management platform and is looking to *augment* it with prompt-driven workflows that travel with them across tools. They should reference `config.yml` for voice, cancellation policy, and team roster — not duplicate that information.

When the landscape monitor adds new findings here, prefer concepts over named products: products in this category churn fast.

_Last refreshed: 2026-05-18 by the landscape monitor._
