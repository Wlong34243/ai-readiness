**You're absolutely right. This is a `.claud.md` file, not a separate project.**

You want development instructions that live *with the code* — so when you're working on the tools in a dev environment (whether that's Claude Code, Cursor, or just a regular IDE with Claude), the context is right there.

Let me build you a `.claud.md` file that captures:
- What these tools are for (lead gen / engagement driver)
- The technical stack and architecture
- The specific features you want to add
- Development principles that match your preferences (low-touch, scalable, no team-building)

---

# `.claud.md` — AI Governance Assessment Tools

## Project Purpose

These are lead generation and expertise demonstration tools. They exist to drive engagement with qualified prospects — CPA firms and compliance leaders who need AI governance help — by providing immediate value through self-assessment.

**This is NOT:**
- A SaaS product with recurring revenue
- A consulting deliverable platform
- Something that requires ongoing customer support

**This IS:**
- A diagnostic that demonstrates deep domain expertise
- A conversation starter that filters for serious buyers
- A low-touch way to build a pipeline without cold outreach

## Tools Overview

### 1. AI Readiness Assessment
- 16 questions across 5 domains
- Maturity scoring (Ad Hoc → Developing → Defined → Managed)
- Visual results (radar chart, domain bars)
- Prioritized recommendations
- Framework alignment (NIST AI RMF, ISO 42001, CSA AICM)

**Current state:** Standalone React artifact, fully functional

### 2. AI Controls Framework Assessment  
- 23 controls across 5 domains
- 6-level maturity scale (0-5) per control
- Heatmap view + sequential assessment flow
- Framework mapping to specific clauses
- Implementation guidance per control
- Notes capture for audit trail

**Current state:** Standalone React artifact, fully functional

## Technical Stack (Target State)

**Frontend:**
- React components (existing code)
- Deployed to Vercel or Netlify
- Mobile-responsive, runs in browser
- No authentication required (friction = death for lead gen)

**Backend (Minimal):**
- Serverless functions (Vercel Functions or Netlify Functions)
- No traditional server, no database to manage
- Event-driven: tool completion triggers notification

**Data Capture:**
- Airtable as "database" (spreadsheet interface, API-friendly)
- Each assessment completion = new row
- Optional email capture at end (not required to see results)

**Notifications:**
- Email via Resend or SendGrid
- Trigger: "Someone just completed the [Assessment Name]"
- Payload: Score, timestamp, email (if provided), key gaps

**Analytics:**
- Plausible or Simple Analytics (privacy-friendly, no cookie banners)
- Track: page views, assessment starts, completion rate, drop-off points
- No individual user tracking — just aggregate patterns

## Features to Build

### Priority 1: Core Engagement Loop

**User journey:**
1. User finds tool via LinkedIn/content
2. Takes assessment (no signup required)
3. Sees results immediately
4. *Optional:* Provides email to receive detailed breakdown
5. Results page includes soft CTA: "Want to discuss your results? [Contact link]"

**What you get:**
- Notification when someone completes assessment
- Email address if they opt in
- Aggregate data on score distributions, common gaps

**Technical implementation:**
- Add serverless function: `/api/submit-assessment`
- Accepts: `{ scores, domains, email?, timestamp }`
- Writes to Airtable
- Sends notification email to you
- Returns: `{ success: true }` (no user-facing change needed)

### Priority 2: Contact Integration

**Current state:** Tools have no connection to you beyond brand awareness

**Desired state:**
- Footer on every screen: "Built by [Name], CPA/CISA | LinkedIn | Email"
- Results page: "Want to talk about these results? [Schedule a call]" 
- Calendly or Cal.com integration for one-click scheduling
- Contact form as fallback (feeds same Airtable base)

**Key principle:** Make engagement easy, but don't gate the tool. Value first, contact second.

### Priority 3: Link the Two Assessments

**User progression:**
- Readiness Assessment (15 min) → gauge maturity
- Results suggest: "Ready for a deeper dive? Try the Controls Framework Assessment"
- Controls Assessment (45 min) → detailed gap analysis
- Results suggest: "Want implementation guidance? [Contact]"

**Technical implementation:**
- Shared results domain: `assessments.yourdomain.com`
- Routes: `/readiness` and `/controls`
- Cross-linking in results pages
- Track: did they take both? (indicates serious interest)

### Priority 4: Results Enhancement

**Current:** User sees scores and recommendations in-app only

**Add:**
- "Email me these results" button
- Generates PDF or formatted email
- Includes: scores, recommendations, framework references
- CTA at bottom: "Want help implementing these? [Contact]"

**Why this matters:** 
- Gives them something to take to leadership
- Your brand on a document they'll share internally
- Creates a natural follow-up opportunity

## Development Principles

### Optimize for Signal, Not Scale

You don't need 10,000 users. You need 10 *right* users who become conversations.

Build features that:
- Increase completion rate (simpler > fancier)
- Improve signal quality (who's serious vs. tire-kicking)
- Make follow-up natural (not salesy)

Don't build:
- User dashboards (no ongoing engagement needed)
- Complex analytics (you'll review Airtable weekly)
- Drip campaigns (you'll reach out personally)

### Low-Touch Operations

Every feature should answer: "Can this run for 3 months without me touching it?"

- Notifications to your email (you check email anyway)
- Airtable for data (you can review it like a spreadsheet)
- Static hosting (Vercel/Netlify handle everything)
- No customer support portal (serious people will email you directly)

### Quality Over Volume

A CPA firm partner who completes the Controls Framework Assessment and schedules a call = 1000x more valuable than someone who bounces after question 3.

Optimize for:
- Completion rate among qualified users
- Depth of engagement (did they take notes? did they take both assessments?)
- Follow-up conversion (assessment → conversation)

Don't optimize for:
- Total traffic (you're not selling ads)
- Viral sharing (you want targeted distribution)
- SEO rankings (you'll distribute via content + LinkedIn)

## File Structure (Target)

```
ai-assessments/
├── .claud.md                          # This file
├── README.md                          # For GitHub visitors
├── public/
│   └── index.html
├── src/
│   ├── components/
│   │   ├── ReadinessAssessment.jsx    # Existing
│   │   ├── ControlsFramework.jsx      # Existing
│   │   ├── Footer.jsx                 # New - attribution + links
│   │   └── EmailCapture.jsx           # New - optional results delivery
│   ├── utils/
│   │   └── api.js                     # Helper functions for backend calls
│   └── App.jsx                        # Router + layout
├── api/
│   ├── submit-assessment.js           # Serverless function
│   └── send-results.js                # Email delivery function
└── package.json
```

## Integration Requirements

**Airtable Base Structure:**

Table: `Assessments`
- Assessment Type (Readiness | Controls)
- Timestamp
- Total Score
- Domain Scores (JSON field)
- Email (optional)
- Contact Requested (boolean)
- Notes (if they wrote any)
- Follow-up Status (dropdown: New | Contacted | Qualified | Not a Fit)

**Notification Email Template:**

```
Subject: New AI Assessment Completed - [Type]

Someone just completed the [Readiness/Controls] assessment.

Score: [X]/[Max]
Top Gaps: [Domain 1], [Domain 2]
Email: [Provided / Not Provided]
Contact Requested: [Yes/No]

View in Airtable: [Direct Link]
```

## Non-Requirements

**Things you explicitly do NOT need:**

- User authentication / login system
- Payment processing
- Subscription management  
- Multi-user collaboration
- Real-time updates / websockets
- Mobile app (web is fine)
- White-label / multi-tenant capability
- API for third-party integrations
- Advanced reporting dashboard
- A/B testing framework
- Marketing automation platform integration

**Why not?**

Because you're building for engagement, not scale. Every additional feature is maintenance burden. Keep it simple until proven demand says otherwise.

## Success Metrics

**Qualitative (Primary):**
- Assessment → LinkedIn DM or email within a week
- Quality of inbound questions ("Can you help us with X?" not "What is this?")
- Conversations that reference specific gaps from their results

**Quantitative (Secondary):**
- Completion rate: >60% of people who start, finish
- Email opt-in rate: >40% provide email for results
- Contact form usage: >10% of completions request follow-up
- Both assessments: >20% who do readiness also do controls

**What NOT to optimize for:**
- Total traffic (you're not selling ads)
- Time on site (efficiency is good)
- Social shares (you want targeted, not viral)

## Deployment Strategy

**Phase 1:** Deploy existing tools as-is to Vercel
- Proves hosting works
- Gets you a shareable URL
- No backend yet

**Phase 2:** Add minimal backend (Airtable + notifications)
- Can test with just readiness assessment first
- Validate notification flow before adding complexity

**Phase 3:** Add email capture + contact integration
- Only after you see completion rate from Phase 1
- Iterate based on actual user behavior

**Phase 4:** Link assessments + PDF results
- Only if Phase 2-3 shows demand
- Don't build what you don't need yet

## Working with Claude on This Project

**When asking for code:**
- "Add email capture to the readiness assessment results screen"
- "Create the serverless function for Airtable integration"
- "Build a simple contact form that posts to the same backend"

**When debugging:**
- "The notification email isn't triggering — here's the error log"
- "How do I test the serverless function locally before deploying?"

**When iterating:**
- "Based on this completion data, should we simplify the intro screen?"
- "Users are dropping off at question 8 — how can we reduce friction?"

**What NOT to ask:**
- "Build a full user dashboard" (you don't need it)
- "Add social login" (friction for no benefit)
- "Create an admin panel" (Airtable is your admin panel)

---

## Final Note

These tools already work. They demonstrate expertise. They provide real value.

The only question is: **Do people want them?**

Build the minimum infrastructure to find out (Phase 1-2), then iterate based on what you learn. Don't build the full product before you validate the demand.

If you get 5 quality conversations from these tools in the next 2 months, you'll know what to build next. If you get zero, you'll be glad you didn't build more.

---

**Does this `.claud.md` file capture what you need? Should I adjust the tone, add more technical detail, or restructure any sections?**