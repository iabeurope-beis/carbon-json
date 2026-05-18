# carbon.json

carbon.json is a public-feedback draft specification and JSON Schema for publishing comparable carbon-intensity disclosures for digital advertising system infrastructure.

It defines a machine-readable disclosure format for reporting the emissions intensity of a digital advertising system, service, or service variant, expressed as kg CO2e per 1,000 attributable delivered impressions.

carbon.json is not a full campaign-footprint calculator and does not replace corporate greenhouse-gas inventories. It provides a standardized infrastructure-intensity input that can be used in path-level emissions estimation when combined with information about the actual systems involved in delivering an impression.

## Status

Status: Public feedback draft

Version: v1.0 public-feedback draft

Intended use: Voluntary disclosure and implementation testing; not yet a final conformance standard.

## What carbon.json includes

- A mandatory location-based gross core emissions-intensity metric.
- A common functional unit: kg CO2e per 1,000 attributable delivered impressions.
- Boundary rules for operational energy, FERA, embodied IT and network infrastructure, and embodied facility infrastructure.
- Denominator, allocation, data-quality, uncertainty-buffer, publication, and versioning requirements.
- A provider-neutral JSON Schema and illustrative disclosures.

## What carbon.json does not measure

carbon.json does not measure the full advertiser campaign footprint, media production emissions, publisher content emissions unrelated to the disclosed ad system function, end-user device emissions, open internet emissions outside the reporter's operated or contracted infrastructure, or corporate greenhouse-gas inventories.

Market-based electricity claims, offsets, removals, and avoided-emissions claims are not netted into the mandatory core metric.

## Repository Contents

| File | Purpose |
|---|---|
| `specification.md` | Normative public-feedback draft |
| `schema.json` | JSON Schema for validating disclosures |
| `examples/` | Provider-neutral illustrative disclosures |
| `docs/quickstart-cloud.md` | Non-normative cloud implementation guide |
| `docs/implementation-guide.md` | Non-normative implementation guidance |
| `docs/conformance-checklist.md` | Checklist for reviewing draft conformance |
| `PUBLIC-FEEDBACK.md` | How to comment on the draft |
| `references.md` | Non-normative references |

The latest schema, examples, quickstart, and implementation resources for the public-feedback draft are maintained in this repository.

## Key Links

- [Specification](specification.md)
- [JSON Schema](schema.json)
- [Examples](examples/)
- [Cloud quickstart](docs/quickstart-cloud.md)
- [Implementation guide](docs/implementation-guide.md)
- [Public feedback process](PUBLIC-FEEDBACK.md)

## Provide Feedback

The public feedback period is open until 19 August 2026.

Feedback may be submitted through GitHub Issues or by email to IAB Europe's Data & Innovation Strategist, Dimitris Beis, at beis [at] iabeurope [dot] eu.

Methodology comments are most useful when they identify the affected section, describe the implementation concern, propose specific wording or schema changes, and explain the expected impact on comparability, feasibility, or data quality.

## License

The repository content is provided under CC0-1.0 unless stated otherwise in a file. Trademarks, logos, and organizational names are not licensed for reuse unless explicitly stated.
