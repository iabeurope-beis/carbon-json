# carbon.json v1.0 Public-Feedback Draft Conformance Checklist

This checklist is non-normative. It is intended to help reviewers assess whether a draft disclosure appears to satisfy the public-feedback draft requirements.

A disclosure is conformant only if:

- [ ] all required schema fields are present
- [ ] reporting period is annual or monthly
- [ ] one mandatory `location_based_gross_core` metric is published
- [ ] denominator follows the required hierarchy
- [ ] location-based Scope 2 generation is kept separate from market-based claims
- [ ] Scope 3 FERA is included or validly handled
- [ ] embodied IT/network emissions are included, normalized, or validly handled through the non-decomposed provider Scope 3 fallback
- [ ] embodied facility emissions are included, normalized, or validly handled through the non-decomposed provider Scope 3 fallback
- [ ] exclusions inside the core boundary do not exceed 5%
- [ ] market-based electricity, offsets, removals, and avoided-emissions claims are not netted into the core metric
- [ ] uncertainty buffer is applied
- [ ] data-quality tier is disclosed
- [ ] file validates against `schema.json`

Additional review checks:

- [ ] reporting scope is supported by numerator and denominator data
- [ ] failed, unused, idle, reserve, and resiliency-related activity are allocated to delivered impressions
- [ ] PUE or provider-embedded facility overhead is applied exactly once
- [ ] separately disclosed embodied emissions are normalized to the required service lives
- [ ] use of non-decomposed provider Scope 3, if any, includes the full provider value and discloses that embedded embodied-emissions service lives could not be independently verified or restated
- [ ] supplemental claims are clearly labeled non-comparable
- [ ] record versioning and recast metadata are complete where applicable
