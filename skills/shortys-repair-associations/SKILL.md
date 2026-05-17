---
name: shortys-repair-associations
description: "Find Shortys contacts missing expected company associations (by email-domain match) and repair via API. Monthly hygiene."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: stub
---

# Shortys Repair Associations

## Plan

For every contact with an email like `firstname.lastname@company.com`, verify the contact has an `associated_company` record set to a company with matching domain. If not, create the company (if missing) and associate. Monthly cadence.

Extends TomGranot's `enrich-company-name` pattern (which fills the company NAME on the contact) by ensuring the structural ASSOCIATION exists, not just the property value.

## Before State

```python
# Count contacts where:
# - has an email
# - email domain is corporate (not gmail/yahoo/hotmail/etc.)
# - NOT has an associated company OR associated company's domain doesn't match email domain
```

## Execute

For each affected contact:

1. Extract domain from email
2. Check if a company with that domain exists in HubSpot
3. If yes: create the association
4. If no: create the company (minimal fields: domain, name = email-domain-cleaned-up) then associate

Use HubSpot CRM Associations v4 API.

## After State

- 0 corporate-email contacts without an associated company on matching domain
- Snapshot of all new company records created (for rollback)

## Rollback

Reverse-PATCH: delete the contact-company associations created. Delete the new company records (with confirmation per company, since they may have been enriched after creation).

## Cadence

Monthly per master prompt v4.2. Wire to Mega Cycle `ecosystem_documentation` domain.

## Status

Stub 2026-05-16. Implementation deferred until first monthly hygiene cycle (target: 2026-06-15). Current cohort is small enough that ad-hoc fixes work.
