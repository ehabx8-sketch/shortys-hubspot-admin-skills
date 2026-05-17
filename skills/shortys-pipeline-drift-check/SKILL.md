---
name: shortys-pipeline-drift-check
description: "Walk all 3 Shortys deal pipelines (Landlord Acquisition, Tenant Booking, Broker Partnership), flag stage transitions that diverge from expected rates. Quarterly diagnostic."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: stub
---

# Shortys Pipeline Drift Check

## Plan

Audit each of Shortys' 3 deal pipelines for stage-velocity drift vs expected rates. Extends TomGranot's `cleanup-deals` pattern.

Shortys pipelines:

- Landlord Acquisition (8 stages): Outreach Sent → Discovery Call → Property Assessment → Terms Proposed → Agreement Signed → Listing Live → First Booking → Repeat Landlord
- Tenant Booking (8 stages): Inquiry Received → Match Sent → Tour Booked → Tour Completed → Booking Proposal → Booking Confirmed → Stay Completed → Repeat Customer
- Broker Partnership (6 stages): Outreach Sent → Partnership Call → Proposal Sent → Agreement Signed → Active Partner → Referring Partner

## Before State

For each pipeline:

- Total deals per stage
- Avg days in stage (vs expected baseline)
- Win/loss rate to next stage
- Stale deals (no activity 30+ days for tenant pipelines, 60+ days for landlord/broker)

## Execute

Audit-only initially. Phase 2 may add a `flag-stale-deals` execute step.

## After State

Output: drift report per pipeline at `audit-artifacts/phase-8/pipeline-drift-YYYY-MM-DD.md` with per-stage observations + recommendations (extend stage, deprecate stage, reassign deals to right pipeline).

## Status

Stub 2026-05-16. Implementation deferred until first quarterly cycle (target: 2026-08-15) when there's actual pipeline movement to analyze. Currently pipelines have minimal data (24 listings from Loopnet validation, no booked tours yet).
