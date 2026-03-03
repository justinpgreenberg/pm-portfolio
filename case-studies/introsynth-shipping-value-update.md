# IntroSynth — Shipping Value Update (Week of 2026-03-02)

## 1) What shipped (facts only)
- Implemented the job scoring v3 model end-to-end (DB schema updates, Edge Function scoring logic, frontend integration, and tests), with a referral-first scoring framework and rollout/calibration documentation.
- Added score transparency UX in Jobs: subscore explainer tooltips and improved relevance scoring by using selected/default resume evidence.
- Shipped Jobs workflow upgrades: real posted dates, inline archive confirmation, add-job modal alignment fixes, tier quota UI updates, and plan rename from Enterprise to Surge.
- Improved reliability and security baseline: stabilized auth session handling, enforced server-side session checks, tightened job-write RLS policy scope, locked ingestion monitoring RPCs to authenticated callers, and restored ingestion-health query indexes.
- Added operational visibility work: dashboard ingestion health card and ingest-task provider hardening to enum constraints.

## 2) Why this mattered (user + business)
- User problem addressed:
  - Job prioritization previously lacked a clear, confidence-building scoring system and explanation layer.
  - Session/auth instability and weak backend guardrails could create failed actions or trust erosion.
- Expected user impact:
  - Better shortlist quality through clearer Opportunity Rank decomposition (relevance, comp/scope, referral opportunity).
  - Higher trust from transparent score explanations and more stable authenticated flows.
  - Smoother tracker workflow from posted-date accuracy and archive/add-job UX cleanup.
- Expected business impact:
  - Increased activation and repeat usage of the Jobs workspace via clearer decision support.
  - Lower operational risk from stronger auth/RLS and monitoring-path hardening.
  - Better foundation for future calibration/experimentation of scoring quality.

## 3) Instrumentation added/updated
- New events:
  - No new client analytics event schema shipped this week; instrumentation emphasis was backend observability and score-data persistence.
- Updated properties:
  - Expanded job score persistence and scoring outputs in support of v3 model fields/versioning (including relevance/comp/referral-oriented scoring components and related metadata for calibration).
- Funnel step(s) affected:
  - Jobs discovery -> scoring -> tracker decision step, where users evaluate priority and actionability.
- Guardrail metrics monitored:
  - Ingestion health monitoring path improved via dashboard health visibility, authenticated RPC restrictions, and restored indexes for monitoring queries.

## 4) Results so far (leading indicators)
- Activation:
  - Too early for stable quantitative readout this week; major scoring + UX changes just landed.
- Quality:
  - Product quality improved qualitatively via clearer score explainability and better resume-evidence relevance handling.
- Retention:
  - No reliable retention signal yet attributable to these changes within the same week.
- Any regressions:
  - No known critical regressions from this batch; auth/session edge cases were explicitly addressed in this cycle.

## 5) PM decisions made
- Decision:
  - Adopted v3 scoring direction that separates role relevance, comp/scope value, and referral opportunity, then combines into a final opportunity rank.
- Options considered:
  - Keep a simpler opaque composite score vs. ship a decomposed, more explainable framework.
  - Keep “effort modifier” as a prominent user-facing score vs. de-emphasize in UI and retain internally.
- Trade-off accepted:
  - Increased implementation complexity and calibration surface area in exchange for stronger explainability and better decision quality.
- Why this path was chosen:
  - It aligns with IntroSynth’s core value proposition: helping users prioritize where to apply and where networking/referral leverage can improve outcomes.

## 6) Engineering + product learnings
- What worked:
  - Shipping scoring logic across migration + Edge Function + frontend in one coherent slice reduced integration drift.
  - UI explainers improved clarity without requiring users to tune scoring knobs.
  - Security hardening (RLS/RPC/session checks) paired well with feature velocity.
- What didn’t:
  - Measurement still lags feature delivery; we improved system observability but did not introduce a richer client analytics event layer in the same week.
- Unknowns remaining:
  - Calibration quality of v3 scoring once larger real-user samples accumulate.
  - Exact contribution of referral-opportunity weighting to downstream conversion/interview outcomes.

## 7) Next 1–2 experiments
- Hypothesis:
  - More explicit referral-first ranking cues and tighter evidence presentation will increase add-to-tracker conversion and shortlist confidence.
- Test design:
  - Compare current scoring presentation against a variant that emphasizes referral leverage at the decision moment and refines evidence snippets.
- Success threshold:
  - Positive lift in score-to-tracker conversion and no increase in archive/reversal behavior; maintain auth and monitoring guardrails.
- Owner + timeline:
  - Product + Engineering (Jobs): run in the next 1–2 weekly cycles after baseline metric stabilization.

## 8) Risks / blockers / asks
- Risk:
  - Limited near-term quantitative signal may cause premature scoring-weight changes.
- Mitigation:
  - Hold v3 weights stable briefly, collect baseline, then calibrate using structured rollout + guardrails.
- Help needed:
  - Alignment on the minimum analytics layer to add next (event naming + ownership) so PM decisions can be backed by faster experiment readouts.
