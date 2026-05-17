---
name: shortys-deliverability-snapshot
description: "Weekly deliverability health check: Postmaster Tools + DMARC report + sender reputation + bounce/complaint trend. Single-page output that tells you whether shortyscre.com is in good standing this week."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: stub
---

# Shortys Deliverability Snapshot

## Plan

Weekly health check on the `shortyscre.com` sending domain (and Week-5+ separate cold-outbound domain when it lands). Collect signals from:

1. Google Postmaster Tools — domain reputation, IP reputation, spam rate
2. DMARC reports (RUF/RUA) — alignment failures, auth pass rate
3. HubSpot Email Health — bounce rate, complaint rate, delivery rate (last 7 days)
4. mail-tester (manual) — score on a fresh test send
5. Bounce/complaint counter trends (week-over-week from `SUP-Hard Bounced` and `SUP-Spam Complained` segment sizes)

## Before State

```python
# Single-page report that compares this week vs last week:
# - Postmaster reputation: stable / improving / degrading
# - DMARC pass rate: %
# - HubSpot bounce rate: % (target < 2%)
# - HubSpot complaint rate: % (target < 0.1%)
# - mail-tester score: out of 10 (target ≥ 9)
```

## Execute

Audit-only skill. No mutations. Generates a one-page weekly snapshot at `audit-artifacts/phase-6/deliverability-YYYY-WW.md` (year-week format).

## After State

If any signal degrades by > threshold:

- Bounce rate > 3% → flag for `/review-bounced-contacts` skill
- Complaint rate > 0.3% → STOP, escalate to CEO persona, pause all marketing sends
- Postmaster reputation drops → reduce send volume by 50% for 7 days
- DMARC pass rate < 99% → audit DNS records, escalate to CTO persona

## Cadence

Weekly. Wire to Mega Cycle `auth_hygiene` or new `deliverability_health` domain. Run every Monday morning before the weekly digest email.

## Status

Stub 2026-05-16. Implementation deferred until first 100+ external sends ship (so signals are meaningful). Currently sender reputation is clean (mail-tester 9.1/10 as of 2026-05-12) but volume is too low for trend analysis. Estimated first useful run: after Tuesday 2026-05-19 calibration + a week of post-send observation.
