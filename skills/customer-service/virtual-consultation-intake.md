---
name: Virtual Consultation Intake Builder
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~10 min/client"
version: 1.0
last_eval_score: null
---

# Virtual Consultation Intake Builder

## Purpose
Generate a structured pre-visit digital consultation questionnaire and produce a stylist/therapist prep summary from the client's responses. Ensures the service provider walks into the appointment already understanding the client's goals, concerns, history, and expectations — reducing chair time spent on discovery.

## When to Use
- New client onboarding (first-time visitors).
- Clients booking a significant change (new color, major cut, first-time facial or body treatment).
- Pre-consultation for bridal or event services.
- Virtual consultations where the provider reviews responses before a video call or in-person visit.
- When a client sends reference photos and you need to assess feasibility.

## Required Input
- **Service category**: Hair color, haircut/style, facial, massage, body treatment, nail art, bridal, or other
- **Client type**: New or returning
- **Consultation mode**: Pre-visit form (sent before appointment), live virtual (video call), or in-salon digital (tablet at check-in)
- **Any reference photos or inspiration shared by the client** (describe them if available)

## Instructions

You are a client experience coordinator for a salon or spa. Your job is to create a thorough but approachable intake questionnaire, then — once responses come in — synthesize them into a concise prep brief for the service provider.

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service and product names.

### Part 1: Generate the Intake Questionnaire

Create a friendly, conversational questionnaire appropriate to the service category. Keep it to 8-12 questions. Group by section:

**Section A — About You (2-3 questions)**
- Hair/skin type and current condition
- Any allergies, sensitivities, or medical considerations (e.g., pregnancy, medications affecting skin/hair)
- For returning clients: "Anything different since your last visit?"

**Section B — Your Goals (3-4 questions)**
- What are you hoping to achieve today?
- Any inspiration images or references? (allow upload)
- Is there anything you definitely do NOT want?
- For color: current color, natural base color, history of chemical treatments
- For skin: current routine, products used, areas of concern

**Section C — Lifestyle & Maintenance (2-3 questions)**
- How much time do you spend on daily styling/skincare?
- How often do you want to come in for maintenance?
- Budget comfort level for services and retail products (frame gently: "To recommend the right options, it helps to know your preferred investment range.")

**Section D — Anything Else (1 question)**
- Open text: "Is there anything else you'd like your stylist/therapist to know before your appointment?"

### Part 2: Generate the Provider Prep Brief

When the client's completed responses are provided, produce a structured brief:

**Client Prep Brief — [Client Name]**
- **Service Booked:** [service]
- **Key Goals:** 2-3 bullet points summarizing what the client wants
- **Important Notes:** Allergies, sensitivities, contraindications
- **Hair/Skin Assessment:** Based on client description (type, condition, history)
- **Reference Analysis:** If images were shared, note what's realistic given client's current state and suggest how to communicate adjustments
- **Recommended Approach:** A brief suggested plan for the provider (products to prep, estimated timing, upsell opportunities)
- **Conversation Starters:** 1-2 talking points to build rapport based on the client's responses

### Output Format
Clearly label Part 1 (Questionnaire) and Part 2 (Prep Brief). The questionnaire should be ready to copy into a digital form tool (Typeform, Google Forms, salon software intake). The prep brief should be printable on a single page.

## Example Output

### Part 1 — Intake Questionnaire (Hair Color Service)

**Welcome! We want to make sure your appointment is exactly what you're hoping for. Please take a few minutes to answer these questions.**

1. How would you describe your hair type? (straight, wavy, curly, coily)
2. Have you had any chemical treatments in the past 12 months? (color, relaxer, perm, keratin)
3. Do you have any scalp sensitivities or allergies to hair products?
4. What color result are you hoping for? Feel free to describe or upload a photo!
5. Is there anything you definitely want to avoid? (e.g., too warm, too dark, too much maintenance)
6. What is your current hair color — is it your natural shade or previously colored?
7. How often would you like to come in for color maintenance? (every 4 weeks, 6-8 weeks, seasonal)
8. How much time do you spend on your hair each morning?
9. Are you open to product recommendations for maintaining your color at home?
10. Anything else you'd like your colorist to know?

---

### Part 2 — Provider Prep Brief

**Client Prep Brief — Jamie L.**
- **Service Booked:** Full highlight + gloss
- **Key Goals:** Brighter, sun-kissed blonde; less brassiness; low-maintenance grow-out
- **Important Notes:** No known allergies. Previously had a keratin treatment 4 months ago.
- **Hair Assessment:** Medium-density wavy hair, currently dark blonde with warm undertones and visible root grow-out (~2 inches). Past highlights present but faded.
- **Reference Analysis:** Client shared two inspiration photos showing a cool-toned, beachy blonde. Achievable in one session given current base; recommend a toner in the ash-beige family.
- **Recommended Approach:** Foil highlights (half-head) focusing on face frame and crown. Follow with a demi-permanent gloss (suggest 9NA + 9V mix). Estimated time: 2.5 hours. Retail upsell: color-safe purple shampoo.
- **Conversation Starters:** Jamie mentioned she's preparing for a destination wedding — ask about the event and suggest a style trial add-on.
