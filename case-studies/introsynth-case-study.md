# IntroSynth Case Study

## One-Line Product Thesis

Career coaching is high impact but cost-prohibitive; IntroSynth productizes core coaching workflows so users can get tailored guidance and execution support at software economics.

## Problem Framing

### User pain

Job seekers face three compounding issues:

1. Generic advice not tied to specific roles.
2. Low response rates from cold outreach.
3. Slow feedback loops that make improvement difficult.

### Business opportunity

Traditional coaching is constrained by human time. A systemized product can deliver:

- lower cost per user,
- faster turnaround,
- and repeatable quality via structured data + model feedback.

## System Overview

IntroSynth combines deterministic pipeline components with AI generation steps:

1. **Resume ingest and normalization**
   - Parse user resume into canonical skill, experience, and impact objects.
2. **Job ingest and structuring**
   - Convert job postings into requirement vectors and role signals.
3. **Fit and gap matching**
   - Score alignment between user profile and target roles, identify high-leverage gaps.
4. **Referral discovery support**
   - Surface likely referral pathways based on company/role targeting and network cues.
5. **Outreach generation**
   - Generate personalized outreach drafts grounded in user specifics and role context.
6. **Feedback loop**
   - Capture outcomes (reply/no reply, interview, conversion) and use them to refine ranking and generation prompts.

## Growth Loops

### Loop 1: Better Inputs → Better Match Quality → Better Outcomes

- As users provide cleaner profile data and preferences, match precision improves.
- Better match precision increases response/interview rates.
- Positive outcomes increase trust and data completeness from users.

### Loop 2: Outreach Performance Learning Loop

- Outreach messages are tracked by variant and context.
- Top-performing patterns are reused and adapted.
- Message quality improves over time, increasing user-perceived value.

### Loop 3: Activation-to-Retention Loop

- Early “first useful output” (fit score + actionable gap plan) drives activation.
- Activated users engage in repeated cycles (refine target roles, regenerate outreach, track responses).
- Repeated usage creates compounding personalization, improving retention.

## Key Product Decisions

1. **System of record first, generation second**
   - Decision: prioritize structured profile + job schemas before investing heavily in prompt complexity.
   - Why: model quality degrades when inputs are inconsistent.

2. **Actionability over advice volume**
   - Decision: optimize for a short list of next actions instead of long-form coaching narratives.
   - Why: execution friction, not awareness, is usually the user bottleneck.

3. **Event instrumentation as launch-gate**
   - Decision: ship only flows with complete activation and quality telemetry.
   - Why: without decision-grade data, iteration velocity collapses.

4. **Human tone constraints in outreach generation**
   - Decision: enforce style and specificity checks before presenting drafts.
   - Why: generic AI language reduces trust and response rates.

## Metrics and Evaluation

### Activation metrics

- Time to first fit score
- Time to first outreach draft
- % of users completing first end-to-end application cycle

### Quality metrics

- Match relevance score (user-rated + behavior-inferred)
- Outreach acceptance/edit rate
- Reply rate uplift vs. user baseline

### Retention metrics

- Weekly active users in active job-search mode
- Number of completed feedback cycles per user
- Cohort retention by target-role clarity at onboarding

## What I Learned

1. **PLG requires immediate utility, not comprehensive setup.**
   Users need a fast “I can use this now” moment; depth can unfold progressively.

2. **Data modeling is product strategy in AI systems.**
   Schema quality directly determines recommendation and generation quality.

3. **Outcome instrumentation should be tied to decisions, not dashboards.**
   Every tracked metric should correspond to a known lever the team can pull.

4. **Trust compounds when outputs are specific and editable.**
   Users engage more when content is personalized and transparently improvable.

## Why This Matters for PM Roles

IntroSynth demonstrates product leadership across discovery, system design, growth mechanics, instrumentation, and iteration. It reflects how I operate on AI-enabled products where value depends on both user experience and data loop quality.
