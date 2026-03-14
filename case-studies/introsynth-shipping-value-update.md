# IntroSynth â€” Shipping Value Update (Week of 2026-03-02)

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
  - Keep â€œeffort modifierâ€ as a prominent user-facing score vs. de-emphasize in UI and retain internally.
- Trade-off accepted:
  - Increased implementation complexity and calibration surface area in exchange for stronger explainability and better decision quality.
- Why this path was chosen:
  - It aligns with IntroSynthâ€™s core value proposition: helping users prioritize where to apply and where networking/referral leverage can improve outcomes.

## 6) Engineering + product learnings
- What worked:
  - Shipping scoring logic across migration + Edge Function + frontend in one coherent slice reduced integration drift.
  - UI explainers improved clarity without requiring users to tune scoring knobs.
  - Security hardening (RLS/RPC/session checks) paired well with feature velocity.
- What didnâ€™t:
  - Measurement still lags feature delivery; we improved system observability but did not introduce a richer client analytics event layer in the same week.
- Unknowns remaining:
  - Calibration quality of v3 scoring once larger real-user samples accumulate.
  - Exact contribution of referral-opportunity weighting to downstream conversion/interview outcomes.

## 7) Next 1â€“2 experiments
- Hypothesis:
  - More explicit referral-first ranking cues and tighter evidence presentation will increase add-to-tracker conversion and shortlist confidence.
- Test design:
  - Compare current scoring presentation against a variant that emphasizes referral leverage at the decision moment and refines evidence snippets.
- Success threshold:
  - Positive lift in score-to-tracker conversion and no increase in archive/reversal behavior; maintain auth and monitoring guardrails.
- Owner + timeline:
  - Product + Engineering (Jobs): run in the next 1â€“2 weekly cycles after baseline metric stabilization.

## 8) Risks / blockers / asks
- Risk:
  - Limited near-term quantitative signal may cause premature scoring-weight changes.
- Mitigation:
  - Hold v3 weights stable briefly, collect baseline, then calibrate using structured rollout + guardrails.
- Help needed:
  - Alignment on the minimum analytics layer to add next (event naming + ownership) so PM decisions can be backed by faster experiment readouts.

---

# IntroSynth - Shipping Value Update (Week of 2026-03-14)

## 1) What shipped
- Stabilized the Resume Tailor workflow around a DOCX-first editing model instead of continuing to treat PDF as the editable source of truth.
- Shipped a much richer resume optimization review flow: section approval, per-bullet approve/skip, recommended new bullets support, clearer source-vs-optimized previewing, and stronger guardrails against unsupported role-title overclaim.
- Integrated the live N8N resume parse/optimize contract more tightly with the app, including DOCX-aware request fields, stronger optimization prompt structure, and rewrite-strength support.
- Cleanly merged the Narratives and Interview Prep lane into the same working line, then merged that combined line into `main`.
- Standardized repo-local skill storage so canonical skills now live under `skills/` instead of being scattered between `skills/` and `.codex/skills/`.

## 2) Why this mattered
- User problem addressed:
  - Resume tailoring was getting trapped in low-fidelity PDF mutation behavior and confusing review UX.
  - Parallel work across resume + narratives/interview prep risked drift and merge confusion without a clean integration point.
- Expected user impact:
  - Higher trust in resume tailoring because users can review stronger, more structured optimizations against a DOCX-native source.
  - Better continuity across product surfaces now that resume, narratives, and interview prep work are merged into one current `main`.
- Expected business impact:
  - Resume tailoring is materially closer to a premium-worthy workflow rather than a prototype.
  - Cleaner merge discipline lowers future agent overhead and speeds parallel shipping.

## 3) Instrumentation updates
- Added stronger structured optimization payload handling in the app for:
  - role family
  - proof priorities
  - selected/rewritten bullets
  - suggested new bullets
  - rewrite strength
- Tightened product memory and roadmap docs so future work is now explicitly split across lanes instead of inferred ad hoc.

## 4) Results so far
- Quality:
  - Resume review UX is materially improved versus the original PDF-only mutation approach.
  - Local validation is green after merging the branches: `npm run typecheck` and `npm run build` both pass on `main`.
- Activation:
  - No quantified usage result yet; this was primarily a foundation and workflow-quality cycle.
- Regressions caught:
  - The branch merge exposed a Windows/casing issue around `ui/card.tsx` and `ui/badge.tsx`; that was fixed before leaving `main`.

## 5) PM decisions made
- Decision:
  - Treat DOCX as the canonical editable resume source and PDF as the final export/share artifact.
- Options considered:
  - continue pushing deeper into PDF mutation
  - pivot to DOCX-first editing and keep PDF as output
- Trade-off accepted:
  - DOCX-first adds document-format complexity up front, but removes a long tail of low-fidelity PDF editing bugs.
- Why this path was chosen:
  - It is the only path that gives IntroSynth a realistic shot at a high-trust resume editing experience without fabricating layout fidelity.

## 6) Engineering + product learnings
- What worked:
  - Moving to structured optimization contracts made prompt tuning and review UX much more tractable.
  - Merging branches before assigning the next work split was the right call; it exposed real conflicts early and forced cleanup.
- What did not:
  - The half-step preview-edit UX (click in preview, edit elsewhere) was not good enough to keep.
  - Repo-local skills had drifted into multiple locations, which invited ambiguity.
- Unknowns remaining:
  - How far direct-in-pane DOCX editing should go before it becomes too heavy relative to user value.
  - How aggressive optimization rewrites should be before they feel unsafe or inflated.

## 7) Next 1-2 experiments
- Hypothesis:
  - A DOCX-native, direct-in-pane editing experience paired with stronger rewrite controls will materially improve trust and completion of resume tailoring.
- Test design:
  - Keep the current guided review flow stable while prototyping true direct-in-pane editing on top of the DOCX preview.
  - In parallel, expand optimization strength controls and evaluate whether stronger role-family-aware rewrites improve approval rates.
- Success threshold:
  - Users can understand and adopt resume changes faster, with fewer â€œwhere did this come from?â€ moments and no loss of fidelity in the exported artifact.
- Owner + timeline:
  - Resume / AI lane next, immediately after the merge-and-cleanup cycle.

## 8) Risks / blockers / asks
- Risk:
  - The product can still accumulate hidden agent/process debt if worktree discipline and skill/document canonicalization drift again.
- Mitigation:
  - Keep `main` as the rebranch point, enforce lane split in `NEXT_UP.md`, and keep repo-local skills canonical under `skills/`.
- Help needed:
  - Agent B should now produce a matching PM-facing handoff update for Narratives / Interview Prep / Dashboard-shell work so both lanes are represented consistently in the PM repo.

---

# IntroSynth - Shipping Value Update (Week of 2026-03-14, Narratives + Interview Prep)

## 1) What shipped
- Reworked `Interview Prep` into a clearer rehearsal flow: one interview-mode entry point, camera/mic confidence cues, transcript capture, replay/export, and usable history.
- Shipped a transcript-to-critique pipeline backed by a Supabase Edge Function, with persisted score, hire signal, rubric breakdown, transcript, and critique text in session history.
- Simplified the critique rubric to three dimensions that held up in testing: `Structure`, `Substance`, and `Delivery`.
- Rebuilt `Narratives` around category-aware authoring for `Tell Me About Yourself` and `Behavioral`, including AI-assisted structure generation, review/build flow, and sectioned editing.
- Added the narrative-to-interview handoff so saved narratives can load into `Interview Prep` as concise cheat-sheet notes instead of raw section dumps.

## 2) Why this mattered
- User problem addressed:
  - Interview rehearsal was visually noisy, hard to trust, and not producing reusable feedback.
  - Narrative authoring was too heavy and too generic, especially for `Tell Me About Yourself`, where one-shot AI structuring caused section drift.
- Expected user impact:
  - Higher confidence during practice from visible camera/mic state, clearer save/history behavior, and critique that feels tied to the actual answer.
  - Lower effort to turn messy career stories into interview-ready material, then reuse those stories directly in rehearsal.
- Expected business impact:
  - Better activation on premium-looking workflows without adding high storage or ops burden.
  - Stronger linkage between core product surfaces instead of isolated point features.

## 3) Instrumentation updates
- Added persisted interview critique outputs in `interview_sessions`:
  - `transcript`
  - `ai_feedback`
  - `score`
  - `rubric_breakdown`
- Standardized critique state handling so the UI can distinguish:
  - transcript unavailable
  - critique pending
  - critique failed
  - critique ready
- No major new client analytics event layer was added in this lane; instrumentation was primarily persisted session/critique state.

## 4) Results so far
- Quality:
  - Browser rehearsal flow is materially more trustworthy than the initial version: camera works, mic meter works, save/history is understandable, and critiques now persist.
  - TMAY narrative generation improved after moving from hidden one-pass composition to extraction/review/build, which reduced section leakage and made the AI output controllable.
- Leading indicators:
  - Manual testing showed the final v1 rubric and critique output were “good enough for v1” after prompt tightening and category-specific evaluation rules.
  - Narrative cheat-sheet loading in interview mode now supports a more realistic rehearsal workflow.
- Regressions caught and fixed:
  - backend schema drift on `narratives` / `interview_sessions`
  - invalid OpenAI secret wiring for critique generation
  - category constraint mismatch on `interview_sessions.question_category`
  - multiple UX dead ends around save/history, replay, and delete confirmation

## 5) PM decisions made
- Decision:
  - Keep interview recording local-first by default and store derived artifacts (transcript, critique, score, notes) instead of making raw media hosting the default product behavior.
- Options considered:
  - always host raw audio/video
  - local-first recording with export and saved insights
- Trade-off accepted:
  - Less persistent replay from history in exchange for lower storage cost, lower privacy burden, and a cleaner v1 scope.
- Additional decisions:
  - Treat `Tell Me About Yourself` and `Behavioral` as the launch-ready narrative types.
  - Keep theoretical product questions in `Interview Prep`, but do not force them into `Narratives`, which should stay grounded in lived-experience stories.
  - Disable unreliable TMAY rewrite paths when they caused section drift rather than shipping them because they "kind of worked."

## 6) Engineering + product learnings
- What worked:
  - The rehearsal UX improved most when setup, performance, and review were separated cleanly.
  - The critique pipeline became much more legible once the rubric was reduced to three defensible dimensions.
  - TMAY AI support improved only after shifting from hidden composition to extraction -> user review -> section build.
- What did not:
  - Generic section “strengthen/optimize” behavior was too probabilistic for TMAY and had to be constrained heavily or disabled.
  - Native browser confirms and bottom-of-page notes/history placement made the product feel less polished than it was.
- Unknowns remaining:
  - How much additional critique tuning is needed before users trust scores without caveats.
  - Whether a saved cheat-sheet artifact is necessary post-launch, or whether runtime formatting is enough.

## 7) Next 1-2 experiments
- Hypothesis:
  - Making saved interview sessions easier to compare and revisit will improve repeat practice and critique usefulness.
- Test design:
  - Add richer history follow-up only after launch basics are stable: compare attempts, regenerate critique, and possibly persist prompt/version metadata.
- Success threshold:
  - Users can tell what improved between attempts and use saved notes/critique as a real coaching loop rather than a passive log.
- Owner + timeline:
  - Narratives / Interview Prep lane, after launch-critical design-system and onboarding work stabilizes.

## 8) Risks / blockers / asks
- Risk:
  - The AI-assisted writing flows can still feel “magical until they drift,” especially on nuanced TMAY content.
- Mitigation:
  - Keep the trustworthy human-in-the-loop review/build flow, and only expose AI rewrites where section boundaries are stable enough to defend.
- Help needed:
  - Once broader design-system work settles, validate these two surfaces against the refreshed shell so they do not regress visually or drift from the new onboarding/dashboard language.
