# PR-5 Scoring + UX Wrap-Up

Date: 2026-03-05  
Scope: Jobs scoring model UX, guardrails, performance controls, and reliability checks.

## What Shipped

1. Scoring guardrails + tests
- Added shared recommendation tier guardrail logic in `src/lib/job-scoring.ts`.
- Added unit coverage in `src/lib/job-scoring.test.ts`.

2. Referral action improvements
- Upgraded `Who can intro me?` to show richer, actionable context.

3. Decision-signal UI
- Added card-level Tier, Confidence, and Needs Verification badges.
- Added expandable `Why this score` details:
  - must-have evidence,
  - verification questions,
  - risk flags.

4. Expensive vs cheap scoring split
- Added instant weight-based reranking in Jobs from stored subscores only.
- Added explicit per-job `Recalculate Score` action for heavy recompute.
- Added background stale/missing score refresh with batching:
  - stale threshold: 7 days,
  - max background refresh batch: 10 jobs.

5. Production vs operator UX
- Added persistent `Advanced Mode` toggle in Jobs.
- Gates operator controls in production view:
  - Data Sync Health panel,
  - Ranking Weights panel,
  - global Refresh Scores,
  - per-job Recalculate Score button.

6. Backend parity
- Updated edge function tiering to align with rank-based guardrails:
  - `supabase/functions/job-score-calculator/index.ts`.

7. Contract + scrape regression tests
- Added scraper normalization tests:
  - `src/lib/job-autofill.test.ts`.
- Added score payload contract validator + tests:
  - `src/lib/job-score-contract.ts`,
  - `src/lib/job-score-contract.test.ts`.
- Wired score contract validation into Jobs load/reload paths.

## Why This Matters

1. Better trust and explainability
- Users can see not only rank, but why rank exists.

2. Better performance/cost control
- Weight changes are instant and local.
- Expensive recompute is explicit or background-batched.

3. Better production readiness
- Internal/debug controls are hidden by default via Production View.

## 5-Minute Verification Checklist

1. Install deps and build
```powershell
cd C:\IntroSynth\Introsynth
npm.cmd install
npm.cmd run build
```

2. Run focused tests
```powershell
npx vitest run src/lib/job-scoring.test.ts src/lib/job-autofill.test.ts src/lib/job-score-contract.test.ts
```

3. Redeploy scoring edge function
```powershell
npx supabase functions deploy job-score-calculator
```

4. Start app
```powershell
npm.cmd run dev:restart
```

5. Manual QA (Jobs page)
- Confirm `Production View` hides operator controls.
- Switch to `Advanced Mode` and confirm controls appear.
- Move weight sliders and confirm rank/tier update instantly (no heavy rescore required).
- Click `Recalculate Score` on one job and confirm score updates.
- Expand `Why this score` and verify evidence/risk/question sections render.
- Confirm card shows `Data freshness: Updated ...`.

## Known Follow-ups

1. Dashboard V2 integration review
- Validate any shared component/style overlap after Dashboard V2 merge.

2. Score calibration iteration
- Tune defaults after a week of production telemetry.

3. Chunk-size optimization
- Bundle remains large; consider code-splitting pass post-feature freeze.
