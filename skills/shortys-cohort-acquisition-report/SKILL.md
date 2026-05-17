---
name: shortys-cohort-acquisition-report
description: "Per-cohort weekly funnel: source → enrichment → import → workflow → engagement. Tells you which lead-engine sources are actually producing value at each stage."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: stub
---

# Shortys Cohort Acquisition Report

## Plan

Generate a per-cohort weekly report tracing each cohort's full funnel from source pull through engagement. Cohorts are tagged via `lead_engine_cohort` property (e.g., `2026-05-loopnet-validation`, `2026-05-daily-ring`, `2026-05-apollo-week-1`).

Per cohort, surface:

| Stage | Metric |
| --- | --- |
| Source pull | Raw contacts produced |
| Dedup | Kept (after removing existing HubSpot dupes) |
| Suppression | Kept (after removing SUP-* members) |
| Verification | Kept (passed MX + role-address + disposable checks) |
| Classification | Distribution by `shortys_marketplace_role` |
| Import | Successfully added to HubSpot |
| Workflow enrollment | Number entering each nurture workflow |
| 7-day open rate | % |
| 7-day click rate | % |
| 14-day reply rate | % |
| 30-day tour-booked rate | % |

## Before State

Read all contacts with the target `lead_engine_cohort` value. Pull engagement history per contact.

## Execute

Audit-only. Generate report at `audit-artifacts/phase-L/weekly-cohort-YYYY-WW.md`.

## After State

The report drives source-evaluation decisions:

- Source open rate < 40% at 14 days → AUTO-PAUSE the source per master prompt v4.2 escalation trigger #9
- Source classification confidence < 0.7 → retune the source's segmentation rules
- Source hard-bounce rate > 5% → tighten verification step for that source

## Cadence

Weekly per master prompt v4.2 §8 metrics. Wire to Mega Cycle `lead_acquisition` domain. Run every Friday afternoon (so the weekend doesn't introduce stale-data ambiguity).

## Status

Stub 2026-05-16. Implementation deferred until Apollo Path C is implemented (v4.2 Week 2) and the daily ring chain is actually producing cohort data. Currently the only meaningful cohort is `2026-05-loopnet-validation` (7 broker contacts, too small for meaningful trends).
