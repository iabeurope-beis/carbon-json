# carbon.json Implementation Guide

This guide is non-normative. It provides practical workflow guidance for preparing a carbon.json disclosure. The normative requirements are in `specification.md` and `schema.json`.

## Annual Disclosure Workflow

1. Define the disclosure scope: legal entity, service name, service variant, system class (`node_class`), geography, deployment archetype, and reporting period.
2. Map the production infrastructure used by that scope across owned, leased, colocation, cloud, hosted, and edge environments.
3. Collect operational energy, direct emissions, FERA, embodied infrastructure, denominator, and allocation data for the reporting period.
4. Apply the denominator hierarchy in the specification.
5. Allocate shared, idle, reserve, failed, and resiliency-related activity to delivered impressions.
6. Normalize separately disclosed embodied emissions to the carbon.json service lives.
7. Calculate raw absolute emissions and raw intensity.
8. Determine the data-quality tier and uncertainty buffer.
9. Prepare the JSON disclosure and validate it against `schema.json`.
10. Publish the file at a stable endpoint and retain prior versions when records are recast.

## Monthly Disclosure Workflow

Monthly files use the same schema as annual files. They should be published only where activity, energy, emissions, and allocation data support the monthly boundary.

For each month:

- use the monthly reporting period in `info.reporting_period`;
- match operational emissions and denominator data to the same month;
- use monthly or finer Scope 2 generation factors where available;
- disclose the temporal resolution of FERA factors;
- prorate separately disclosed embodied emissions by calendar days in the month unless the source already provides monthly allocation; and
- keep `record_version` independent for each published monthly record.

## Owned, Leased, and Colocation Workflow

For operated or contracted facilities:

1. Start with metered facility electricity, utility invoices, or rack/cage electricity.
2. Add direct fuel and refrigerant emissions where applicable.
3. Add location-based Scope 2 generation emissions and location-based FERA.
4. Include facility overhead exactly once through total billed facility energy, measured PUE, or provider-embedded overhead.
5. Map IT, network, power, cooling, and building assets to the embodied categories in the specification.
6. Allocate shared facility and infrastructure emissions using physical or capacity metrics where available.

If only IT electricity is available and no provider-embedded facility overhead is supplied, apply the PUE rules in the specification.

## Public Cloud and Hosted Infrastructure Workflow

Provider-issued emissions outputs may be used as source inputs where they can be mapped to carbon.json requirements.

For each source input:

- identify whether Scope 2 generation is location-based;
- determine whether facility overhead is already embedded;
- determine whether FERA is available separately or combined with other Scope 3 values;
- determine whether embodied IT, network, facility, and support infrastructure are included or embedded in a non-decomposed Scope 3 value;
- remove or exclude out-of-boundary services where separable;
- avoid double counting when supplementing missing categories; and
- document source coverage and gaps in `method.source_coverage_note`.

If a provider-issued Scope 3 value is infrastructure-attributable but not decomposed, it may be used as a conservative combined input only under the conditions in the specification. In that case, do not separately restate embedded embodied-emissions components that cannot be isolated; include the full non-decomposed value, disclose the limitation in `method.source_coverage_note`, and apply Tier C treatment.

## Multi-Service Allocation Workflow

When one infrastructure pool supports multiple services:

1. Directly assign dedicated assets and workloads first.
2. Allocate dynamic compute by measured usage such as CPU time, accelerator time, or instance-hours.
3. Allocate idle, reserve, and standby capacity by provisioned capacity.
4. Allocate storage by occupied capacity and storage class.
5. Allocate networking by bytes, packets, port utilization, or another engineering metric.
6. Allocate residual shared services by operational proxy where possible.
7. Use economic allocation only for residual shared services where no physical, capacity, or operational proxy is available.

Record the dominant allocation methods in `method.allocation_methods` and explain material fallback methods in `method.source_coverage_note`.

## Treatment of Incomplete Data

Incomplete data should be handled conservatively:

- disclose missing in-scope categories;
- estimate omitted in-scope emissions and keep them within the 5.0% exclusions cap;
- reduce the quality tier where source granularity, coverage, or allocation quality is limited;
- apply the required uncertainty buffer; and
- do not publish a conformant disclosure if missing in-scope categories exceed the exclusions cap.

Do not replace missing operational or embodied emissions with market-based claims, offsets, removals, or avoided-emissions claims.

## Use of Provider-Issued Emissions Outputs

Provider-issued emissions outputs are source inputs, not automatic conformance evidence.

Before using such outputs, check whether they:

- cover the same reporting period and service scope as the disclosure;
- use location-based Scope 2 generation for the core metric;
- include or allow supplementation of FERA;
- include, allow supplementation of, or validly embed embodied IT, network, facility, and support infrastructure in a non-decomposed provider Scope 3 value;
- include facility overhead exactly once;
- allocate shared and idle capacity to the reporter or service; and
- can be reconciled with the denominator used in the carbon.json file.

Where a source output cannot be mapped to the core boundary without material omissions or double counting, it should not be used alone for a conformant disclosure.

## Validation Workflow

Before publishing:

1. Validate the file against `schema.json`.
2. Confirm that `raw_value`, `published_value`, and the emissions breakdown are internally consistent.
3. Confirm that all required schema fields are present.
4. Confirm that provider sources, where disclosed, are identified in `cloud_providers`.
5. Confirm that non-comparable supplemental claims are not netted into the core metric.
6. Confirm that issue comments, recasts, and versioning metadata are accurate.
