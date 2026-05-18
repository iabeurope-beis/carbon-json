# Cloud and Hosted Infrastructure Quickstart

This quickstart is non-normative. It explains how to use cloud, hosting, or provider-issued emissions data as source inputs for a carbon.json disclosure without relying on provider-specific schema rules.

It also includes two worked examples derived from the anonymized sample exports in `sample-data/`.

## 1. Start With the Disclosure Scope

Define:

- legal entity;
- service name and service variant;
- system class (`node_class`);
- reporting period;
- geography scope;
- deployment archetype; and
- denominator source.

## 2. Map Provider-Issued Data to the Core Boundary

For each source input, identify whether it contains:

- Scope 1 direct emissions;
- location-based Scope 2 generation emissions;
- location-based FERA;
- embodied IT and network emissions, or whether these are embedded in a non-decomposed Scope 3 value;
- embodied facility emissions, or whether these are embedded in a non-decomposed Scope 3 value;
- facility overhead;
- shared and idle capacity allocation; and
- services or rows outside the carbon.json core boundary.

Provider-issued emissions outputs MAY be used where they can be mapped to the carbon.json core boundary, denominator rules, allocation requirements, and uncertainty-buffer requirements.

## 3. Keep Location-Based and Market-Based Values Separate

Use location-based Scope 2 generation emissions in `location_based_gross_core`.

Do not net market-based electricity, offsets, removals, or avoided-emissions claims into the core metric. If such claims are disclosed, keep them in `results.supplemental` or `claims`.

## 4. Apply Denominator and Allocation Rules

Use the first available denominator in the hierarchy:

1. `reconciled_delivered_impressions`
2. `winning_events_times_trailing_settled_ratio`
3. `billable_impressions`
4. `winning_events`

Allocate provider-issued emissions to the disclosed service scope using the most specific available basis. Prefer customer-specific source outputs, direct assignment, measured physical allocation, and provisioned-capacity allocation before operational proxies. Use economic allocation only for residual shared services where no better allocator is available.

## 5. Apply Uncertainty Buffers

Determine the quality tier after reviewing source coverage, temporal resolution, allocation method, denominator source, and embodied-emissions treatment.

The published buffer must be at least as conservative as both:

- the denominator fallback buffer; and
- the data-quality tier buffer.

Use of a non-decomposed provider Scope 3 value as a conservative combined input makes the disclosure no better than Tier C. Where embedded embodied-emissions components cannot be isolated, include the full non-decomposed value and disclose that service-life normalization could not be independently verified or restated.

## 6. Disclose Data Quality

Populate:

- `quality.primary_data_share_percent`
- `quality.quality_tier`
- `quality.uncertainty_buffer_percent`

Use `method.source_coverage_note` to explain material gaps, fallback methods, and treatment of non-decomposed source inputs.

## 7. Handle Incomplete Provider Data

When provider-issued data is incomplete:

- supplement missing in-scope categories from compatible sources where possible;
- avoid double counting categories already embedded in the provider-issued output;
- disclose omissions and estimate their share of raw core emissions;
- do not claim conformance if omitted in-scope emissions exceed 5.0% of raw core emissions; and
- reduce the quality tier where incomplete data materially affects the result.

## 8. Validate the Disclosure

Validate the resulting JSON against `schema.json`. Check that:

- `cloud_providers` identifies provider names or categories where applicable;
- the core metric uses the required functional unit;
- raw and published values are non-negative; and
- all required method and quality fields are present.

## Worked Examples

The sample disclosures use a base implementation level: one SSP-wide disclosure covering all impressions for the reporting period, rather than a more granular product, format, region, or buyer-specific variant.

### Inputs

| Input | Example 1 | Example 2 |
|---|---:|---:|
| Reporter | Example Low-Volume SSP Ltd | Example Scaled SSP Ltd |
| System class (`node_class`) | SSP | SSP |
| Service variant | all-impressions | all-impressions |
| Reporting period | 2025-01-01 to 2025-12-31 | 2025-01-01 to 2025-12-31 |
| Provider export | `sample-data/cloud_example_1.csv` | `sample-data/cloud_example_2.csv` |
| Derived disclosure | `sample-data/cloud_example_1.json` | `sample-data/cloud_example_2.json` |
| Denominator type | `billable_impressions` | `reconciled_delivered_impressions` |
| Denominator count | 5,000,000 | 200,000,000,000 |

Both examples use non-decomposed provider Scope 3 as a conservative combined input, which makes each disclosure no better than Tier C and requires a 15% buffer. Example 1 also uses `billable_impressions`, but its 10% denominator buffer is lower than the Tier C buffer, so the published buffer remains 15%. Example 2 uses `reconciled_delivered_impressions`, which adds no denominator buffer.

Because the provider Scope 3 values are not decomposed, the examples do not separately restate embedded embodied-emissions components to carbon.json service lives. The full non-decomposed provider Scope 3 values are included as conservative combined inputs.

### Map the Example Exports

For Example 1, sum `sample-data/cloud_example_1.csv` as follows:

| carbon.json component | CSV column |
|---|---|
| `scope1_direct` | `total_scope_1_emissions_value` |
| `scope2_location_based_generation` | `total_scope_2_lbm_emissions_value` |
| `non_decomposed_provider_scope3` | `total_scope_3_lbm_emissions_value` |
| `scope3_fera_location_based` | `0`, because Scope 3 is not decomposed |
| `embodied_it_and_network_normalized` | `0`, because Scope 3 is not decomposed |
| `embodied_facility_normalized` | `0`, because Scope 3 is not decomposed |

Rows for delivery-only services are outside the core boundary unless they directly support in-scope SSP intermediary processing. In Example 1, `ContentDelivery` has zero emissions and contributes nothing to the numerator.

For Example 2, sum `sample-data/cloud_example_2.csv` as follows:

| carbon.json component | CSV column |
|---|---|
| `scope1_direct` | `carbon_footprint_kgCO2e.scope1` |
| `scope2_location_based_generation` | `carbon_footprint_kgCO2e.scope2.location_based` |
| `non_decomposed_provider_scope3` | `carbon_footprint_kgCO2e.scope3` |
| `scope3_fera_location_based` | `0`, because Scope 3 is not decomposed |
| `embodied_it_and_network_normalized` | `0`, because Scope 3 is not decomposed |
| `embodied_facility_normalized` | `0`, because Scope 3 is not decomposed |

Exclude `ContentDelivery` rows from Example 2 where they represent delivery-only infrastructure outside the SSP intermediary boundary.

### Example 1 Calculation

Provider export sums:

```text
Scope 1 direct                         0.088861 kg CO2e
Scope 2 location-based generation      3.155430 kg CO2e
Non-decomposed provider Scope 3        4.416660 kg CO2e
```

Core calculation:

```text
Raw gross core total = 7.660951 kg CO2e
Billable impressions = 5,000,000

Raw intensity
= 1000 * 7.660951 / 5,000,000
= 0.001532 kg CO2e per 1000 impressions

Published intensity
= 0.001532190 * 1.15
= 0.001762019
= 0.0018 kg CO2e per 1000 impressions
```

The resulting disclosure is `sample-data/cloud_example_1.json`.

### Example 2 Calculation

Provider export sums after excluding delivery-only rows:

```text
Scope 1 direct                         1,779.393 kg CO2e
Scope 2 location-based generation     81,819.199 kg CO2e
Non-decomposed provider Scope 3      118,051.278 kg CO2e
```

Core calculation:

```text
Raw gross core total = 201,649.870 kg CO2e
Reconciled delivered impressions = 200,000,000,000

Raw intensity
= 1000 * 201,649.870 / 200,000,000,000
= 0.001008 kg CO2e per 1000 impressions

Published intensity
= 0.001008249 * 1.15
= 0.001159487
= 0.0012 kg CO2e per 1000 impressions
```

Because Example 2 uses `reconciled_delivered_impressions`, the denominator adds no buffer. The non-decomposed provider Scope 3 fallback still makes the disclosure Tier C, so the required buffer is 15%. The resulting disclosure is `sample-data/cloud_example_2.json`.

### JSON Field Notes

Both examples use `node_class: "SSP"` and `service_variant: "all-impressions"` because the disclosures are SSP-wide base-level implementations rather than product- or format-specific variants.

Both examples declare `deployment_archetype: ["public_cloud"]` and identify provider sources through `cloud_providers`.

The `included_categories` values identify the in-scope infrastructure categories materially represented by the provider export. Example 2 includes `accelerators` because the sample includes machine-learning workloads; Example 1 does not.

The `excluded_categories` list contains the standard categories excluded from the carbon.json core metric, such as market-based electricity, offsets, corporate overhead, end-user devices, and non-operated open internet infrastructure. Delivery-only `ContentDelivery` rows are handled as outside the SSP intermediary boundary; they are not counted as omitted in-scope emissions.

The `allocation_methods` block records how shared infrastructure is attributed to the SSP disclosure. Both examples use `customer_account_usage` for compute and `provider_embedded` for facility overhead. Example 1 uses `transactions_then_cost_for_residual` for shared services because it represents a lower-granularity implementation. Example 2 uses `direct_usage` because the export has monthly service and location detail.

The `pue` block uses `provider_embedded` with `value: null` because facility overhead is assumed to be embedded in the provider emissions output. No separate PUE multiplier is applied in these examples.

The `grid_factors` block follows the export granularity: Example 1 uses annual Scope 2 resolution, while Example 2 uses monthly resolution. The FERA resolution is reported as annual for Example 1 and monthly for Example 2 because the non-decomposed provider Scope 3 inputs follow those export periods, but the examples do not claim a separate FERA value.

The `amortization_lives_years` block repeats the fixed carbon.json service lives. These values are not chosen per example; they come from the specification. Because these examples use non-decomposed provider Scope 3, the files do not separately restate embodied IT/network or facility emissions that cannot be isolated from the provider value.

The `quality` block explains the published buffer. Both examples are Tier C because they use non-decomposed provider Scope 3 as a conservative combined input. Example 1's billable-impression denominator would otherwise require a 10% buffer, but Tier C requires 15%. Example 2's reconciled delivered impressions add no denominator buffer, but Tier C still requires 15%.

The `assurance` and `claims` blocks are intentionally minimal: neither sample is assured, neither applies offsets, and neither uses market-based instruments in the core metric.
