# Instrumentation Cheatsheet (PM Lens)

## 1) Core Principle

Instrument for decisions, not for dashboard volume.

Every tracked event should answer one of three questions:

1. Did users reach value?
2. Where does the flow break?
3. Which lever should we pull next?

## 2) Event Design Standards

### Event naming

Use consistent `object_action` naming.

Examples:
- `resume_uploaded`
- `match_score_viewed`
- `outreach_draft_generated`
- `outreach_message_sent`

### Required event properties

- `user_id` (or anonymous equivalent)
- `session_id`
- `timestamp`
- `platform`
- `feature_version`
- Context-specific properties (e.g., `job_id`, `match_score_band`, `draft_variant`)

## 3) PM Metric Stack

### Activation

- Time to first value
- % reaching first value moment
- Step conversion through onboarding/value funnel

### Quality

- User-rated output quality
- Edit/accept rates on generated content
- Error/fallback rates

### Retention

- Weekly/monthly active users in core workflow
- Repeat completion of key loop
- Cohort retention by onboarding profile

### Business

- Conversion to paid/qualified state
- Revenue or pipeline influence
- CAC payback (where applicable)

## 4) Funnel Mapping Template

1. Entry event:
2. Setup completion event:
3. First value event:
4. Repeat value event:
5. Conversion event:

For each step, define:
- Conversion rate
- Median time to next step
- Top drop-off segments

## 5) Segmentation Defaults

Always segment by:

- Acquisition source
- User intent / declared goal
- Lifecycle stage (new, activated, retained)
- Plan type or customer tier
- Platform/device

Without segmentation, averages hide product reality.

## 6) Guardrail Metrics

Guardrails prevent local optimization from harming system health.

Common guardrails:
- Latency
- Error rates
- Complaint/support contact rate
- Unsubscribe/churn indicators

## 7) Pre-Launch Telemetry Checklist

- [ ] Event schema reviewed by PM + Eng + Data
- [ ] Tracking plan mapped to PRD goals
- [ ] QA with realistic user paths completed
- [ ] Dashboard/query validated with backfill sanity checks
- [ ] Alert thresholds configured for critical failures

## 8) Post-Launch Review Cadence

- **Day 1:** Data integrity + critical funnel checks
- **Week 1:** Activation and quality diagnostics
- **Week 2-4:** Segment performance + iteration priorities
- **Monthly:** Cohort retention and business impact review
