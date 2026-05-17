---
name: shortys-suppression-enforcement-check
description: "Verify every active HubSpot workflow (and Vercel-function workflow replacement) excludes all 3 Shortys suppression segments at enrollment. Audit-only, no mutations. Run quarterly or after any new workflow ships."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: draft
---

# Shortys Suppression Enforcement Check

Audit-only skill. Walks every active workflow (HubSpot workflows + Vercel-function workflow replacements) and verifies enrollment criteria exclude all three Shortys suppression segments. Required by [.claude/rules/every-workflow-excludes-suppression.md](../../../shortys-waitlist/.claude/rules/every-workflow-excludes-suppression.md).

## Why this matters

A workflow that sends without checking suppression → contact gets emailed despite global unsubscribe → CAN-SPAM violation + deliverability damage at scale. With 50K contacts incoming, one workflow with broken enrollment criteria can produce thousands of bad sends in a day.

The three suppression segments:

1. `SUP-Hard Bounced` (any contact with `hs_email_hard_bounced = true`)
2. `SUP-Spam Complained` (any contact with `hs_email_spam_report_count > 0`)
3. `SUP-Unsubscribed Globally` (any contact with `hs_email_optout = true`)

## Plan

1. Inventory all active workflows (HubSpot UI + Vercel function routes at `/api/workflows/*`)
2. For each, fetch enrollment criteria
3. Verify all 3 suppression segments are excluded
4. Output: pass/fail report with line-item findings per workflow

## Before State

```python
# Audit script outline (Python via hubspot-api-client)
import os
from hubspot import HubSpot

api = HubSpot(access_token=os.getenv("HUBSPOT_ACCESS_TOKEN"))

# 1. List all active workflows (HubSpot)
# 2. For each, fetch enrollment criteria via Automation API
# 3. Parse filter groups, check for membership-NOT-IN against the 3 suppression lists

# Also audit Vercel function workflow replacements:
# - /api/workflows/auto-assign
# - /api/workflows/instant-response
# - /api/workflows/nurture-tick
# - /api/workflows/re-engagement
# - /api/workflows/hot-lead-track
# Each Vercel route should check suppression-list membership before sending
```

For Vercel routes, audit-only the code: look for `hs_email_optout`, `hs_email_hard_bounced`, `hs_email_spam_report_count` checks (or equivalent list-membership API calls).

## Execute

This is an audit-only skill. No mutations. Output is a report:

```markdown
# Shortys Suppression Enforcement Audit — YYYY-MM-DD

## Summary

| Workflow type | Total active | Compliant | Non-compliant |
| --- | --- | --- | --- |
| HubSpot workflows | N | N | N |
| Vercel function routes | 5 | N | N |

## Findings

### COMPLIANT workflows
- WF-01-S: ✓ all 3 suppression segments excluded at enrollment
- ...

### NON-COMPLIANT workflows
- WF-XX: ✗ missing SUP-Spam Complained exclusion — line: <selector>
- ...

## Action items
- Fix WF-XX enrollment criteria: add SUP-Spam Complained to NOT-IN list
- ...
```

Save report to `audit-artifacts/phase-4/suppression-enforcement-YYYY-MM-DD.md`.

## After State

If any non-compliant workflows found, the skill prescribes the specific fix per workflow. Re-run the audit after fixes to confirm 100% compliance.

## Rollback

N/A — audit-only skill, no mutations to roll back. Discarded reports can be deleted.

## Cadence

Quarterly per master prompt v4.2, or after any new workflow ships. Wire to Mega Cycle `workflow_health` domain.

## Status

Skeleton drafted 2026-05-16. Awaiting first implementation pass (estimated 3-4 hours) and first run against current HubSpot portal + Vercel routes.
