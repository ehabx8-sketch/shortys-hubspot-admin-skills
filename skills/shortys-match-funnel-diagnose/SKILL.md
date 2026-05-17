---
name: shortys-match-funnel-diagnose
description: "Bucket demand-side contacts by shortys_match_score range × workflow entry × reply rate. Surfaces the actual match-funnel performance before tuning weights. Diagnostic-first per Mega Cycle anti-pattern memory."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: stub
---

# Shortys Match Funnel Diagnose

## Plan

Apply the Diagnostic-first anti-pattern memory: BEFORE tuning Match Score weights, build the surfacing tool that exposes the actual funnel. Buckets demand-side contacts by `shortys_match_score` band × which workflow they entered × their actual reply rate. The diagnostic itself usually reveals the score-vs-outcome mismatch.

## Before State

Read all demand-side contacts (`shortys_marketplace_side = demand`). For each:

- `shortys_match_score` (0-100)
- Workflow enrollment history (which workflows they passed through)
- Email engagement: opens, clicks, replies
- Tour bookings (deals in Tenant Booking pipeline past Tour Booked stage)

## Execute

Aggregate into a bucket matrix:

| Match Score band | # contacts | # workflow-A entered | # workflow-B entered | Avg replies | Avg tours booked | Avg bookings |
| --- | --- | --- | --- | --- | --- | --- |
| 0-25 | N | N | N | x.x | x.x | x.x |
| 26-50 | ... | | | | | |
| 51-75 | ... | | | | | |
| 76-100 | ... | | | | | |

Output: `audit-artifacts/phase-2/match-funnel-YYYY-MM-DD.md` with the matrix + commentary on observed patterns.

## After State

The output is the input to the next Match Score recalibration cycle. The cycle's tuning decisions should reference specific cells of the matrix ("score band 51-75 reply rate is X% which doesn't justify the weight applied to factor Y, recommend rebalance").

## Cadence

Anytime Match Score performance is questioned. Auto-run as part of every monthly Match Score recalibration cycle (next scheduled: 2026-06-12).

## Status

Stub 2026-05-16. Implementation deferred until sufficient demand-side workflow + engagement data exists (needs at least 100 contacts who've passed through at least one nurture workflow with measurable engagement). Current data is too sparse for the matrix to be meaningful.
