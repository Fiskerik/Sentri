# Review Aggregator for Single-Location SMBs
### Product Brief — Sweden First, Global Ready
*Last updated: March 2026*

---

## Table of Contents
1. [The Idea in One Paragraph](#the-idea)
2. [The Market Gap](#the-market-gap)
3. [Sweden-First Strategy](#sweden-first-strategy)
4. [Target Customer](#target-customer)
5. [Review Platforms to Aggregate](#review-platforms-to-aggregate)
6. [Core Features (MVP)](#core-features-mvp)
7. [Features to Save for Later](#features-to-save-for-later)
8. [Validation Roadmap](#validation-roadmap)
9. [Tech Stack](#tech-stack)
10. [Pricing Model](#pricing-model)
11. [MRR Growth Model](#mrr-growth-model)
12. [Go-to-Market Plan](#go-to-market-plan)
13. [Competitive Landscape](#competitive-landscape)
14. [Your Swedish Moat](#your-swedish-moat)
15. [GDPR & Compliance Notes](#gdpr--compliance-notes)
16. [Next Steps — This Week](#next-steps--this-week)

---

## The Idea

A simple SaaS tool that pulls reviews from Google, Facebook, Reco.se, and Trustpilot into one dashboard for single-location small business owners. It shows them everything in one place, drafts AI-powered replies in Swedish or English, and sends a weekly digest email on Monday morning so they never miss a new review again.

**Pricing:** 199–399 SEK/month (~$19–39).
**Target:** Cafés, salons, gyms, dental clinics, restaurants, tradespeople — any SMB with an online presence across 2+ review platforms.
**The gap:** Enterprise tools (Birdeye, Yext) cost 3,000–30,000 SEK/month and are built for chains. Nothing affordable exists for the solo business owner.

---

## The Market Gap

Every existing tool falls into one of two traps:

| Tool | Price | Problem |
|---|---|---|
| Birdeye | ~3,000 SEK/mo | Built for enterprise chains |
| Yext | ~1,500 SEK/mo | Multi-location, requires sales call |
| BrightLocal | ~420 SEK/mo | Built for SEO agencies, not SMB owners |
| Synup | ~800 SEK/mo | Agency-reseller model, overkill |
| Google Dashboard | Free | Only shows Google reviews |
| Reco.se Dashboard | Free | Only shows Reco reviews |

**The 199–399 SEK/mo slot for a single-location owner who wants everything in one tab is completely empty.**

Incumbents won't go downmarket — it breaks their unit economics. They need annual contract values above 10,000 SEK to justify their sales team. You don't need a sales team to sell a 199 SEK/mo product. You need a product that works, a landing page, and 30 cold DMs a week.

---

## Sweden-First Strategy

### Why Sweden is the right starting market

- You speak the language — cold outreach, customer calls, and support are 10x easier
- You understand the business culture and how Swedish SMB owners communicate
- Sweden has a unique review ecosystem (Reco.se, Prisjakt) that no US-built tool has properly integrated
- Swedish SMBs are tech-comfortable and willing to pay for software (high SaaS adoption per capita)
- Smaller market means faster word-of-mouth — one happy customer in Eskilstuna talks to three others

### The Swedish review landscape

Swedish consumers spread reviews across more platforms than US consumers. A café in Stockholm may have reviews on:

- **Google** — dominant, used by all age groups for local search
- **Reco.se** — the Swedish-native review platform, BankID-verified reviews, very trusted locally
- **Trustpilot** — widely used in Sweden, especially for service businesses
- **Facebook** — still active for local recommendations and business pages
- **TripAdvisor** — hospitality, restaurants, anything tourist-adjacent

This is actually your competitive advantage over US-first tools: **Reco.se and Trustpilot integration makes your product immediately more valuable to a Swedish SMB than anything built in San Francisco.**

### Naming and language

- Website: English (for global accessibility and SEO)
- UI: bilingual from day one — Swedish and English toggle
- Marketing copy: Swedish for outreach, landing page in both
- Domain: consider `.se` for Swedish market trust + `.com` for global
- Suggested working names: **Reviewly**, **Samla** (Swedish for "collect"), **Omdömio**, **Flexa Reviews**

### Global-readiness from day one

Even while selling to Swedish SMBs first, the architecture should be globally ready:

- Build multi-currency billing in Stripe from day one (SEK, EUR, USD)
- Use `i18n` (internationalization) in your frontend — add languages without rewrites
- Store all timestamps in UTC, display in user's local timezone
- Don't hardcode Swedish platform names into your data model — make platform integrations pluggable
- English as the default language in code, database, and internal comms

---

## Target Customer

### Primary persona: "Sofia the salon owner"

- Runs a 1–3 person hair or beauty salon in a Swedish city (Stockholm, Gothenburg, Malmö, Eskilstuna, Uppsala, etc.)
- Has 80–200 reviews spread across Google, Reco.se, and Facebook
- Currently checks each platform separately — or doesn't check at all
- Gets stressed when a bad review comes in and doesn't know how to respond
- Has no marketing person; she does everything herself
- Uses Bokadirekt or Timma for booking, Swish and Stripe for payments
- Will pay 199–299 SEK/month without needing approval from anyone

### Secondary personas

- **Restaurant owner** — high review volume, TripAdvisor matters a lot
- **Dental / physio clinic** — professional tone matters, AI reply drafts are especially valued
- **Handyman / hantverkare** — Reco.se is huge for this segment, Hantverksdata integration potential
- **Gym / PT studio** — Google reviews drive new member signups directly

### Who is NOT your customer (yet)

- Multi-location chains (they need Birdeye or Yext)
- E-commerce businesses (no local reviews)
- Businesses with under 10 reviews (they don't feel the pain yet)

---

## Review Platforms to Aggregate

### MVP (build these first)

| Platform | API | Auth method | Notes |
|---|---|---|---|
| **Google Business Profile** | Google My Business API (free) | OAuth with Google account | Also gives you access to Google Maps data |
| **Facebook** | Facebook Graph API (free) | OAuth with FB business page | Covers page reviews and recommendations |
| **Reco.se** | No public API — scraping or partnership | Business claims their profile | Critical for Swedish market. Start with web scraping; pursue partnership later |
| **Trustpilot** | Trustpilot Business API (free tier available) | API key + business account | Important in Sweden, easy integration |

### V2 (after 50 customers)

| Platform | Why wait |
|---|---|
| TripAdvisor | API is restricted; requires partnership approval |
| Prisjakt | Relevant for retail, not all SMBs |
| Hittaproffs | Niche but relevant for Swedish tradespeople |
| Yelp | Less used in Sweden than US; add for global expansion |

### Note on Reco.se

Reco.se does not have a public API. Your options:
1. **Web scraping** — legally grey in Sweden but commonly done; check their ToS. Build a scraper with Playwright.
2. **Business partnership** — email their partnership team. A growing tool that sends them traffic is in their interest.
3. **Manual import** — user copies their Reco URL, you scrape on their behalf.

Start with option 3 (manual URL + scraping). Clean it up once you have traction.

---

## Core Features (MVP)

These are the only features you build before charging money. Nothing else.

### 1. Unified review feed
- All reviews from all connected platforms in one scrollable list
- Sort by: date, star rating, platform, unanswered
- Filter by: platform, star rating (1-star alerts), date range
- Each review shows: platform logo, stars, reviewer name, date, review text, reply status

### 2. AI reply drafts
- Click any review → AI generates a reply draft in the business's tone
- Two modes: **Swedish** and **English** (auto-detected from review language)
- Editable before copying
- V1: copy-paste to platform manually. V2: one-click publish to Google.
- Uses GPT-4o mini (cost: ~0.05 SEK per draft at scale)

### 3. Weekly email digest
- Sent Monday at 08:00 in user's timezone
- Contains: new reviews this week, average rating this week vs last week, number of unanswered reviews, 2 reviews that need attention (lowest rated, most recent)
- Built with Resend + React Email
- This becomes the most-loved feature — it creates a weekly habit

### 4. Onboarding flow
- Connect Google (OAuth), Facebook (OAuth), Trustpilot (API key), Reco.se (URL)
- Takes under 5 minutes
- Shows a preview of their reviews immediately after connecting first platform — the "aha moment"

### 5. Stripe billing
- 14-day free trial, no credit card required
- 199 SEK/mo (Pro) and 399 SEK/mo (Growth) plans
- Customer portal for self-serve cancellation (reduces churn friction AND support load)

---

## Features to Save for Later

Do not build these until you have 50 paying customers. Resist the temptation.

- **Review request campaigns** (SMS/email to customers asking for reviews) — your $39/mo upsell
- **One-click reply publishing** to Google (requires extra API scope, more complex)
- **Competitor monitoring** — track a competitor's review score over time
- **Team accounts** — multiple users per business
- **White-label** — for agencies to resell to their clients
- **Mobile app** — overkill until you understand usage patterns
- **TripAdvisor integration** — complicated API access
- **Sentiment analytics** — nice to have, not needed for first 100 customers
- **Review widgets** — embed your best reviews on your website

---

## Validation Roadmap

**Rule: do not write a single line of backend code until 5 people give you a credit card number.**

### Week 1: Reddit research (4 hours)

Post in r/smallbusiness and Swedish Facebook groups ("Småföretagare Sverige", "Frisörer Sverige", etc.):

> *"För er som har reviews på Google och Reco.se — har ni något ställe där ni ser allt samlat, eller kollar ni varje plattform för sig?"*

Collect 20+ replies. These replies become your landing page copy, verbatim.

Also post in English on r/smallbusiness, r/entrepreneur — validate internationally in parallel.

### Week 2: Fake-door landing page (1 day)

Build a landing page on **Framer**, **Carrd**, or **Webflow** — no backend.

- Headline: "Alla dina recensioner. En flik. 199 kr/mån." (Swedish version)
- Sub: "Google, Reco.se, Facebook och Trustpilot — samlat på ett ställe med AI-svar."
- Screenshot of a mockup dashboard (design it in Figma or just use a screenshot template)
- "Starta gratis provperiod" button → collects name, email, business name
- Add Hotjar — see where people drop off

Goal: 50 email signups before you build anything.

### Week 3–4: Manual concierge (the most important phase)

DM 30 Swedish business owners via Instagram or LinkedIn:

> *"Hej [Name]! Jag bygger ett verktyg för att samla alla recensioner på ett ställe. Jag skulle vilja ge dig en gratis rapport med dina reviews från Google, Reco och Facebook — tar 15 minuter att läsa. Är det något du skulle ha nytta av?"*

Manually pull their reviews from each platform. Put them in a Google Sheet or a simple PDF. Email it to them Monday morning.

Do this for 10 businesses for 2 weeks. This costs you zero money and teaches you:
- Which platforms they actually care about
- What numbers they look at in the report
- What they say when they read it ("oh I didn't know I had that 1-star review")

### Week 4–5: First payment

Email your 10 manual customers:

> *"Jag bygger det här till ett riktigt verktyg. Beta-priset är 199 kr/mån, och du får 3 månader gratis om du registrerar dig nu."*

Set up a **Stripe payment link** — takes 10 minutes. If 3 of 10 pay: green light to build. If 0 pay: go back and ask why. (Price? Format? Didn't actually solve the pain?)

### Week 6+: Build the MVP

Only now do you open your code editor.

---

## Tech Stack

### Philosophy: boring tech, fast to ship, zero DevOps

Everything below has a free tier that gets you to 5,000 SEK MRR. Switch only when you outgrow it.

| Layer | Tool | Why | Cost at 0 users |
|---|---|---|---|
| **Frontend** | Next.js 15 + Tailwind | App Router, server components, one repo for everything | Free |
| **Hosting** | Vercel | Deploy on git push, preview URLs, cron jobs built in | Free (Hobby) |
| **Database** | Supabase (Postgres) | Managed DB + auth + Row Level Security built in | Free to 500MB |
| **Auth** | Supabase Auth + Google OAuth | Google login = Google Business API access in one OAuth flow | Included |
| **Review APIs** | Google My Business API, Facebook Graph, Trustpilot API, Playwright (Reco) | All free within rate limits | Free |
| **AI replies** | OpenAI GPT-4o mini | ~0.05 SEK per reply draft at scale | $5/mo at 100 users |
| **Email** | Resend + React Email | Best DX, 3,000 free emails/month, great for transactional | Free to 3k/mo |
| **Background jobs** | Vercel Cron | Review sync every 4h, weekly digests on Monday | Free on Pro plan |
| **Billing** | Stripe | Handles subscriptions, trials, failed payments, customer portal | 2.9% + 1.80 SEK per charge |
| **Error tracking** | Sentry | Know when things break before customers tell you | Free tier |
| **Analytics** | Plausible | GDPR-compliant (important for EU), lightweight, 9 EUR/mo | 9 EUR/mo |

### Total infrastructure cost

- **0 customers:** ~0 SEK/month
- **50 customers:** ~200–400 SEK/month
- **100 customers:** ~400–700 SEK/month
- **At 100 × 199 SEK = 19,900 SEK MRR, your infra is ~2–3% of revenue**

### Database schema (simplified)

```sql
users (id, email, name, created_at)
businesses (id, user_id, name, country, language, plan, stripe_customer_id)
platform_connections (id, business_id, platform, credentials_encrypted, connected_at)
reviews (id, business_id, platform, external_id, rating, author, body, replied, review_date, fetched_at)
reply_drafts (id, review_id, draft_text, edited_text, status, created_at)
digest_sends (id, business_id, sent_at, stats_json)
```

---

## Pricing Model

### Tiers

**Free (lead magnet)**
- Google reviews only
- Last 10 reviews
- No AI replies, no digest
- 1 platform — enough to show the value, not enough to keep

**Pro — 199 SEK/month**
- Google + Facebook + Reco.se + Trustpilot
- Unlimited reviews
- AI reply drafts (Swedish + English)
- Weekly Monday email digest
- Sentiment trend (simple chart)
- 14-day free trial

**Growth — 399 SEK/month**
- Everything in Pro
- Review request campaigns (SMS/email to customers)
- Competitor rating tracker (1 competitor)
- One-click Google reply publishing
- Monthly PDF report
- Priority support

### Annual pricing

Offer 2 months free on annual plans (1,990 SEK/yr for Pro). This improves cash flow significantly and reduces churn.

### Why 199 SEK specifically

- Sounds cheap vs "thousands per month" for enterprise tools
- Below the psychological 200 SEK barrier
- Roughly equivalent to $19 — easy to expand globally with same pricing logic
- Still delivers 96%+ gross margin at scale

---

## MRR Growth Model

| Milestone | Users | MRR (SEK) | MRR (USD approx.) | Timeline |
|---|---|---|---|---|
| Validation done | 10 | 1,990 | ~190 | Month 2 |
| Early traction | 50 | 9,950 | ~950 | Month 4 |
| Real product | 100 | 19,900 | ~1,900 | Month 6 |
| Ramen profitable | 200 | 39,800 | ~3,800 | Month 9 |
| Growth phase | 300 | 59,700 | ~5,700 | Month 12 |

Assumptions: 80% on Pro (199 SEK), 20% on Growth (399 SEK), 4% monthly churn, no annual plan uplift factored in.

At 300 users, infrastructure cost is ~1,500 SEK/month. Gross margin: ~97%.

---

## Go-to-Market Plan

### Phase 1: Manual outreach — month 1–2 (0 → 10 customers)

**Channel:** Instagram DMs + LinkedIn cold outreach to Swedish SMB owners

**Who to target:**
- Businesses on Google Maps with 50–300 reviews and a 3.5–4.5 star average (they clearly care about reviews but aren't perfect)
- Instagram business accounts with visible contact info
- Salons, cafés, gyms, clinics in Swedish cities

**DM script (Swedish):**
> *"Hej [Namn]! Jag såg att ni har reviews spridda på Google och Reco — kollar ni dem separat eller har ni något verktyg för det? Jag bygger något som samlar allt på ett ställe, skulle kunna ge er en gratisversion att testa. Intresserade?"*

**Daily target:** 10 DMs/day, 5 days/week = 50 per week. Expect 10–15% reply rate, 3–5% conversion.

### Phase 2: Communities — month 2–4 (10 → 50 customers)

**Swedish Facebook Groups:**
- Småföretagare Sverige (50k+ members)
- Frisörer och Stylistkollegor i Sverige
- Restaurangägare Sverige
- Företagare i [your city]

**Post approach:** Don't sell. Share value. Example:
> *"Vi analyserade 500 recensioner från svenska småföretag — här är de 3 sakerna som skiljer dem med 4.8 stjärnor från dem med 3.9. [Short insight list]. Vi bygger ett verktyg för att göra detta enklare — gratis att testa de första 14 dagarna om ni är nyfikna."*

**Reddit:** r/smallbusiness, r/entrepreneur — post in English for international validation

### Phase 3: Affiliate partners — month 4–6 (50 → 100 customers)

**Target:** Swedish freelance digital marketers and local SEO consultants on LinkedIn. They each have 5–20 SMB clients.

**Offer:** 30% recurring commission, tracked via Rewardful (integrates with Stripe).

One affiliate with 10 clients doubles your MRR overnight.

**Where to find them:** LinkedIn search "digital marknadsföring konsult", Swedish freelancer Facebook groups, Upwork (search Swedish digital marketing freelancers).

### Phase 4: SEO + ProductHunt — month 6+ (100 → 300 customers)

**SEO content (English, for global reach):**
- "BrightLocal alternatives for small businesses"
- "How to manage Google and Reco.se reviews in one place"
- "Best review management tools for single-location businesses 2026"

**ProductHunt launch:**
- Schedule for a Tuesday or Wednesday
- Prepare 5 "hunters" (friends who upvote on launch day)
- Write a maker comment in both Swedish and English
- This alone can add 50–100 signups in a week

**Google Ads:**
- Target: "review management small business", "samla recensioner", "google reviews tool"
- CPC is low in this niche (SEK 5–15 for Swedish terms, $1.50–3 for English)
- Start with 2,000 SEK/month budget, pause if CAC exceeds 6 months of revenue

---

## Competitive Landscape

### Global competitors (not focused on SE)

| Competitor | Strength | Weakness | Your edge |
|---|---|---|---|
| Birdeye | Enterprise features, big brand | 3,000+ SEK/mo, overkill | Price, simplicity, Reco integration |
| BrightLocal | Affordable, good SEO tools | Agency-first, clunky for SMB owners | Purpose-built for SMB, local platforms |
| Podium | Great SMS features | No Yelp integration, US-centric | Swedish platforms, simpler pricing |
| Semrush Local | Powerful analytics | Expensive add-on to a huge platform | Standalone simplicity |
| Chatmeter | Multi-location mastery | Way too complex for solo owners | Single-location focus |

### Swedish competitors

No direct Swedish competitor exists in this exact niche as of March 2026. Reco.se itself doesn't offer aggregation. No Swedish startup is offering a "all your reviews in one place for 199 SEK/mo" product.

**This is your window.** It won't stay open forever — move fast.

---

## Your Swedish Moat

The features that make you hard to displace once established in Sweden:

1. **Reco.se integration** — US competitors won't build this. It requires Swedish market knowledge, scraping or a partnership, and an understanding of why it matters. You have all three.

2. **BankID context** — Reco reviews are BankID-verified. You can surface this trust signal in your dashboard ("This review is verified with BankID") — a feature that has zero meaning outside Sweden but is hugely credible to Swedish business owners.

3. **Swedish AI replies** — GPT-4o replies in Swedish are good but generic. Fine-tune your prompts with Swedish business culture context (more formal, less hyperbolic than US English). Over time, train on your own accepted/edited replies for better output.

4. **Swedish customer support** — you answer in Swedish, same timezone, and understand the culture. Enterprise tools offer English-only support from US time zones. This matters enormously for SMB owners who aren't confident in English.

5. **Word of mouth in a small market** — Sweden has 550,000 registered small businesses. If you get 300 of them (0.05%), you're at a strong MRR. That's achievable. Word travels faster in a smaller country — one happy salon owner in Eskilstuna talks to five other salon owners.

---

## GDPR & Compliance Notes

Operating in Sweden means operating under GDPR. This is not optional — and it's also a marketing advantage if you handle it well.

**Must-do from day one:**
- Privacy policy covering what data you collect and why (user emails, business names, review data)
- Cookie consent banner on the landing page (use Cookiebot or similar, under 500 SEK/mo)
- Data Processing Agreement (DPA) in place with Supabase, OpenAI, Resend, Stripe
- Do not store review content longer than necessary — define a retention policy (e.g. 24 months)
- Provide a clear "delete my account and all data" flow for users

**Review data specifically:**
- Review text and reviewer names you pull from Google/Facebook/Reco are technically third-party data
- You are processing it on behalf of the business owner — structure your ToS accordingly
- Do not use customer review data to train your own models without explicit consent

**Use GDPR as a selling point:**
> *"Vi lagrar all data på EU-servrar och följer GDPR fullt ut."*
> "All data stored on EU servers, fully GDPR compliant."

Use **Plausible Analytics** instead of Google Analytics — it's GDPR-compliant by design, built in the EU, and you can say so proudly.

---

## Next Steps — This Week

This is your exact action plan for the next 7 days. Do these in order. Do not skip ahead.

### Day 1 (today)
- [ ] Post in r/smallbusiness: "For those managing reviews on Google, Yelp, Facebook — do you check them separately or have one tool?" — just to gather language
- [ ] Post in Swedish: "Småföretagare — hur hanterar ni era recensioner på Google och Reco? Kollar ni varje ställe separat?" in at least one Swedish Facebook group
- [ ] Read every reply and save the exact language people use

### Day 2
- [ ] Create a Figma account (free)
- [ ] Sketch a simple 3-screen mockup: dashboard with reviews, single review with AI reply draft, weekly digest email
- [ ] This becomes your landing page screenshot — you need a visual before you can validate

### Day 3
- [ ] Register a domain (Namecheap or Loopia.se for .se)
- [ ] Build a landing page on Framer (free tier) or Carrd (19 USD/yr)
- [ ] Headline, 3 bullets, one mockup screenshot, email capture form
- [ ] Connect the form to a free Mailchimp or ConvertKit account

### Day 4–5
- [ ] Find 30 Swedish businesses on Google Maps with 50+ reviews in a niche you understand (e.g. salons, cafés, gyms in your city)
- [ ] Write and send your first 15 Instagram DMs using the script from this document
- [ ] Track replies in a simple spreadsheet: name, sent date, reply yes/no, meeting booked

### Day 6–7
- [ ] For every business that replies: offer to manually pull their reviews from Google, Reco.se, and Facebook for free
- [ ] Do this for whoever says yes — put the results in a clean Google Sheet or PDF
- [ ] Email it to them with a note: "Here's your weekly review report. Happy to turn this into an automated tool — would love 15 minutes to show you what I'm building."

### After week 1
- [ ] Schedule 5 feedback calls with business owners who received your manual report
- [ ] Ask: What surprised you? What would you check first every week? Would you pay 199 SEK/month for this automatically?
- [ ] After 5 calls: if 3 of them say "yes I would pay for this" — start building
- [ ] Set up a Stripe account and create a payment link for 199 SEK/month at stripe.com/payment-links (takes 10 minutes, no code)

---

## Summary

You have a clear gap, a validated pain point, a Swedish-first advantage that US competitors won't replicate, and a realistic path to 5,000+ SEK MRR in 12 months. The tech is straightforward. The hard part is the first 10 customers — which is why the manual concierge approach matters more than any line of code right now.

The goal for month 1 is not to build software. The goal is to prove that Swedish business owners will pay 199 SEK/month to have someone else deal with their reviews.

Once you know that's true — the building is the easy part.

---

*This document was created as a product brief and is intended to be a living document. Update it as you learn from customers.*
