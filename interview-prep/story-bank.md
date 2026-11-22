# Story Bank — Master STAR+R Stories

This file accumulates your best interview stories over time. Each evaluation (Block F) adds new stories here. Instead of memorizing 100 answers, maintain 5-10 deep stories that you can bend to answer almost any behavioral question.

## How it works

1. Every time `/career-ops oferta` generates Block F (Interview Plan), new STAR+R stories get appended here
2. Before your next interview, review this file — your stories are already organized by theme
3. The "Big Three" questions can be answered with stories from this bank:
   - "Tell me about yourself" → combine 2-3 stories into a narrative
   - "Tell me about your most impactful project" → pick your highest-impact story
   - "Tell me about a conflict you resolved" → find a story with a Reflection

## Stories

<!-- Stories will be added here as you evaluate offers -->
<!-- Format:
### [Theme] Story Title
**Source:** Report #NNN — Company — Role
**S (Situation):** ...
**T (Task):** ...
**A (Action):** ...
**R (Result):** ...
**Reflection:** What I learned / what I'd do differently
**Best for questions about:** [list of question types this story answers]
-->

---

### [AI Design / Ambiguity] Testpert — Designing Without a Playbook
**Source:** Report #003 — Unknown Company — Product Designer
**S:** Bugasura needed an AI feature that auto-generated test cases from requirements — no existing UX playbook for this problem.
**T:** Design a self-service AI flow that QA engineers would actually trust and use.
**A:** Studied analogous AI UX patterns (code gen, writing assistants); ran concept tests with 5 QA users; iterated 3 prototype directions before locking approach.
**R:** Testpert shipped and became Bugasura's headline AI feature — saved QA teams 10 hours/week in manual test case writing.
**Reflection:** When the specific domain has no reference, adjacent domains are your research. I now always ask "what solved this in an adjacent field?" before designing from scratch.
**Best for questions about:** first-of-its-kind problems, AI UX design, ambiguity, product thinking, 0→1 features

---

### [AI Tools / Speed] Bugasura AI Workflow Redesign
**Source:** Report #003 — Unknown Company — Product Designer
**S:** Bug reporting was slow — 3–5 minutes per issue — causing under-reporting across teams.
**T:** Cut reporting time without removing the context quality that makes bugs actionable.
**A:** Designed AI auto-fill, description suggestions, duplicate detection, and auto-prioritisation — built the interaction patterns from scratch with no existing model.
**R:** Reporting time cut to under 10 seconds; 3x increase in reporting frequency.
**Reflection:** AI features fail when the output isn't trustworthy. I now always design an explicit verify/edit step in any AI-generated output — trust is earned, not assumed.
**Best for questions about:** AI tools, design speed, user trust, intelligent automation, productivity features

---

### [Product Thinking / Scope] TCM/RTM Scope Negotiation with PM
**Source:** Report #003 — Unknown Company — Product Designer
**S:** PM proposed a test management system with 12 distinct views — more than the team could build well.
**T:** Simplify scope without losing enterprise functionality that QA teams needed.
**A:** Ran competitive analysis of 5 QA tools; found 40% of proposed views had under 5% usage in competitors; presented data and negotiated scope to 7 core views.
**R:** Feature shipped on time; 25–40% QA efficiency improvement; no complaints about missing views.
**Reflection:** The best design contribution is often scope reduction, not feature addition — but you need data to win that argument, not just instinct.
**Best for questions about:** PM collaboration, challenging requirements, trade-offs, prioritisation, product thinking

---

### [Constraints / Growth] Onboarding Redesign — Frontend Only, 2 Sprints
**Source:** Report #003 — Unknown Company — Product Designer
**S:** Churn was high in the first 30 days; new users weren't reaching activation. No backend budget available.
**T:** Redesign onboarding in 2 sprints using frontend-only changes.
**A:** Ran 8 usability tests with technical users; identified 3 friction points solvable without backend; shipped phased rollout.
**R:** 20% churn reduction, 30% conversion increase in first 30 days, 28% drop-off reduction.
**Reflection:** Constraints force creative prioritisation. I now start every constraints conversation by asking: "What's the highest-leverage change that doesn't need a backend ticket?"
**Best for questions about:** constraints, growth design, onboarding, user research, shipping fast

---

### [Design Systems / Quality at Scale] Bugasura Design System — Sole Designer
**Source:** Report #003 — Unknown Company — Product Designer
**S:** Competing against well-funded QA tools with larger design teams as the sole designer.
**T:** Maintain premium visual quality across all surfaces without a team.
**A:** Built design system with reusable tokens; set visual standards across all surfaces; used Figma auto-layout for consistent fast shipping.
**R:** #1 Product of the Day on Product Hunt; G2 4.6/5, Capterra 4.6/5, SaaSworthy 4.7/5 sustained over years.
**Reflection:** Systems thinking beats raw execution speed. Investing in a design system at month 6 is what enabled consistent quality at month 36.
**Best for questions about:** design systems, visual quality, scaling design, solo/small team design, consistency
