# Memo

## Thesis

Trial-to-paid drop-off for B2B workflow tools is primarily a time-to-value problem, not a pricing or feature problem. Users who don't reach a meaningful outcome in the first 3 days almost never convert. The highest-leverage interventions are upstream of day 3: define one activation metric, fix the blank state, and map onboarding to the job the user showed up to do — not to a feature tour.

## Context

For opt-in free trials, Lenny's free-to-paid survey of 1,000+ B2B products puts "good" conversion at 8–12% and "great" at 15–25% — far below what teams assume when they only watch signup volume. The gap is usually not feature parity; it is activation speed. Activation rate (users who reach a defined aha moment) is a stronger leading indicator of retention than raw signups.

The market has moved from "complete the checklist" onboarding to progressive disclosure — showing less, unlocking more as users accomplish steps. Intercom, Notion, and Linear are the reference implementations. Checklist-style onboarding still works for simple single-feature tools but creates anxiety in complex workflow products.

## Analysis

Three root causes for drop-off appear consistently across research:

**1. No defined activation metric.**  
Most early-stage B2B teams track "signed up" or "logged in" but not "reached first value moment." Without a precise activation event, onboarding has no target and product changes can't be measured. Top performers define one verb + object (e.g. "created first automated workflow," "invited a teammate," "connected first data source") and instrument it before optimizing the funnel.

**2. Blank state on first login.**  
First-login blank dashboards are a common high-friction empty state — users need templates, sample data, or a guided first action, not a feature dump (Intercom C.A.R.E.). Fix: ship a sample project, template, or guided first action before A/B testing anything else.

**3. JTBD mismatch in onboarding flow.**  
Onboarding flows are usually designed around feature completeness ("here is everything the product can do") rather than the specific job the user signed up to accomplish. This is visible in early support tickets — users phrase their frustration as "I can't figure out how to do X" where X is their original intent, not a feature the team built. Fixing this requires segmenting users at signup by intent and routing them to a narrower, job-specific first-run flow.

## Implications

- Activation metric definition is a prerequisite for all other onboarding work. No optimization is measurable without it.
- Blank state fix has the highest ROI per engineering day: typically 1–3 days of work, 2–4 week measurement cycle, often the fastest conversion win available.
- JTBD-aware onboarding is a 1–2 sprint project but has compounding effects — users who complete a job-relevant first session are more likely to invite teammates, increasing expansion revenue.

## Open questions

- What is the current activation metric, if any? Is it instrumented?
- What does a new user see on their first login today?
- What job/intent do users state in signup surveys or early support conversations?
