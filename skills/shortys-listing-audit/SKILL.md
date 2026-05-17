---
name: shortys-listing-audit
description: "Audit data quality on Shortys LISTING custom objects. TRACK 2 — requires HubSpot Pro upgrade (custom objects). Stub only until Pro lands."
license: MIT
metadata:
  author: shortys
  version: "0.1"
  category: shortys-custom
  status: track-2
  blocked-on: hubspot-pro-upgrade
---

# Shortys Listing Audit

## Status: Track 2

This skill is **blocked** until HubSpot for Startups Pro upgrade lands. Custom objects (LISTING, BOOKING, INQUIRY) require Pro tier or higher. Until then, listing data is represented via Deal records in the Landlord Acquisition pipeline, not as a separate custom object.

When the LISTING custom object exists, this skill audits its data quality. Pattern mirrors TomGranot's `hubspot-audit` but scoped to the LISTING object.

## Plan (deferred)

When activated, this skill audits:

- Total listings, by neighborhood / submarket
- Listings missing key fields (`gla_sqft`, `monthly_rent`, `listing_type`, `availability_date`)
- Stale listings (no activity 60+ days, still active)
- Listings missing photos
- Listings with broken or expired CTAs
- Match rate per listing (how many demand contacts hit Match Score ≥ 50 against this listing)

## Trigger to activate

Per [[Track 2 Deferred Work]] §"Custom objects (LISTING, BOOKING, INQUIRY)":

> HubSpot for Startups Pro upgrade lands (Pro tier unlocks custom objects). Then design + migrate from Deal-pipeline-as-listing to proper LISTING custom object.

After that migration, drop the Track 2 status, write the full skill, run first audit.

## Status

Stub 2026-05-16. Awaiting Pro upgrade. Estimated first execution: 4-8 weeks post-upgrade (after migration from Deal-pipeline to LISTING object is complete).
