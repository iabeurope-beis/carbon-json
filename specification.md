# carbon.json v1.0 Public-Feedback Draft Specification

Status: Public feedback draft

Version: v1.0 public-feedback draft

Intended use: Voluntary disclosure and implementation testing; not yet a final conformance standard.

Normative language: The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, MAY, and OPTIONAL are to be interpreted as described in RFC 2119 when they appear in uppercase.

## 1. Purpose

carbon.json defines a standardized, machine-readable disclosure for reporting the carbon intensity of a digital advertising system, service, or service variant.

The mandatory reported metric is location-based gross core emissions intensity, expressed as kg CO2e per 1,000 attributable delivered impressions.

carbon.json is narrower than a corporate greenhouse-gas inventory and narrower than a full campaign-footprint model. It provides a standardized infrastructure-intensity input for use in path-level emissions estimation when combined with information about the actual systems involved in delivering an impression.

## 2. Design Objectives

The v1.0 public-feedback draft is designed to support:

1. comparable infrastructure-intensity disclosure across digital advertising systems;
2. machine-readable publication and validation;
3. separation of the mandatory location-based gross core metric from market-based electricity, offsets, removals, and avoided-emissions claims;
4. consistent denominator and allocation treatment for failed, idle, shared, and reserve activity;
5. explicit data-quality tiers and uncertainty buffers; and
6. implementation across owned data centers, leased data centers, colocation, public cloud, and edge infrastructure without provider-specific schema rules.

## 3. Relationship to Existing Standards and Industry Work

carbon.json is intended to support consistent infrastructure emissions disclosure for use alongside the IAB Europe digital ad emissions methodology and related industry sustainability frameworks, including the Ad Net Zero Global Media Sustainability Framework.

It is designed to be used with established greenhouse-gas accounting and ICT infrastructure guidance, including product/service inventory principles, Scope 2 location-based reporting, Scope 3 fuel- and energy-related activities, ICT-service boundary guidance, data-center PUE measurement, and product carbon data-quality metadata.

Where a reporter's internal inventory convention conflicts with this specification, the reporter MUST apply the carbon.json rules for the disclosed metric. If source data cannot be decomposed or restated, it may be used only under an explicit fallback allowed by this specification.

## 4. Scope

### 4.1 What carbon.json Measures

carbon.json measures the emissions intensity of in-scope digital advertising system infrastructure attributable to a declared disclosure scope.

Permitted disclosure scopes include:

- a service variant and region;
- a service variant globally; or
- a company-level global disclosure for the covered advertising infrastructure.

### 4.2 What carbon.json Does Not Measure

carbon.json does not measure:

- the full advertiser campaign footprint;
- media production emissions;
- publisher page or app content emissions unrelated to the disclosed ad system function;
- end-user device emissions;
- third-party ISP or open internet emissions not directly operated or contracted by the reporter;
- corporate offices, warehouses, employee travel, or general enterprise overhead; or
- corporate greenhouse-gas inventories.

Excluded items MAY be estimated elsewhere in a broader methodology, but they MUST NOT be mixed into the carbon.json core metric.

## 5. Definitions

| Term | Definition |
|---|---|
| Actual path | The specific chain of systems actually involved in delivering an impression. Unused candidate paths are outside the actual path. |
| Ad system infrastructure | Compute, storage, networking, power, cooling, edge, and directly supporting facility systems required to operate the disclosed advertising system or service variant. |
| Attributable delivered impression | A delivered or commercially settled ad impression in the reporting period to which the disclosed system contributed processing or delivery work. |
| Core boundary | The mandatory emissions boundary used to calculate the core metric. |
| Core metric | `location_based_gross_core`, the mandatory comparable carbon.json intensity metric. |
| Disclosure scope | The legal entity, service name, service variant, reporting period, geography, and deployment archetype covered by a file. |
| Embodied emissions | Upstream lifecycle emissions associated with production and deployment of in-scope infrastructure assets, annualized over the standardized service lives in this specification. |
| FERA | Fuel- and energy-related activities under Scope 3 Category 3 that are associated with fuel and energy consumed by in-scope infrastructure and not already counted in Scope 1 or Scope 2 generation emissions. |
| Location-based electricity emissions | Emissions calculated using average grid emission factors for the place and time of energy consumption. |
| Market-based electricity emissions | Emissions that reflect contractual instruments or procurement choices. |
| Published value | The disclosed carbon intensity after applying the required uncertainty buffer. |
| Raw value | The carbon intensity before applying the uncertainty buffer. |
| Service variant | A materially distinct operating mode whose carbon intensity may differ because of format, architecture, workflow, or geography. |

## 6. Conformance

A carbon.json disclosure is conformant with this public-feedback draft only if all of the following are true:

1. The file validates against `schema.json`.
2. The file covers exactly one annual or monthly reporting period.
3. The mandatory `location_based_gross_core` metric is calculated according to Sections 7 through 17.
4. Market-based electricity, offsets, removals, and avoided-emissions claims are not netted into the core metric.
5. Exclusions inside the declared core boundary do not exceed 5.0% of raw core emissions.
6. Embodied-emissions inputs are normalized to the service lives in Section 13 unless the source already uses those same lives or the reporter uses the non-decomposed provider Scope 3 fallback in Section 15.3.
7. The denominator follows the hierarchy in Section 10.
8. The required uncertainty buffer is applied to the published value.
9. The geography and service-scope labels are supported by the underlying numerator and denominator data.
10. Failed, unused, idle, reserve, and resiliency-related activity are included in the numerator where they support delivered impressions.

A file that fails one or more of these conditions MUST NOT be labeled as conformant carbon.json v1.0.

## 7. Mandatory Reported Metric

Each conformant file MUST publish exactly one mandatory comparable metric:

`location_based_gross_core`

The mandatory unit is:

`kg CO2e per 1000 impressions`

For reporting period `p` and disclosure scope `s`:

```text
E_location_based_gross_core_abs =
  E_scope1_direct
  + E_scope2_location_based_generation
  + E_scope3_fera_location_based
  + E_embodied_it_and_network_normalized
  + E_embodied_facility_normalized
  + E_non_decomposed_provider_scope3, where applicable

I_location_based_gross_core_raw =
  1000 * E_location_based_gross_core_abs / D_attributable

I_location_based_gross_core_published =
  I_location_based_gross_core_raw * (1 + buffer_percent / 100)
```

The raw core breakdown MUST disclose the following components as absolute emissions and as intensities:

- `scope1_direct`
- `scope2_location_based_generation`
- `scope3_fera_location_based`
- `embodied_it_and_network_normalized`
- `embodied_facility_normalized`

Where a provider-issued output cannot be decomposed without producing a less representative estimate, `non_decomposed_provider_scope3` MAY be disclosed as a conservative combined input under Section 15.3. It MUST NOT duplicate emissions already counted in FERA or embodied components.

## 8. Core Boundary

The core boundary MUST include emissions attributable to operating the disclosed ad system infrastructure.

Included categories are:

- Scope 1 direct operational emissions from in-scope facilities and infrastructure;
- Scope 2 location-based generation emissions for purchased electricity, steam, heat, or cooling used by in-scope infrastructure;
- Scope 3 FERA on a location-based basis;
- normalized embodied emissions for in-scope IT and network assets; and
- normalized embodied emissions for in-scope facility and support infrastructure.

The core boundary MUST exclude:

- market-based electricity claims;
- offsets, removals, and avoided-emissions claims;
- downstream end-of-life emissions;
- corporate overhead and enterprise support functions;
- offices, warehouses, employee travel, and general business operations;
- product development, software development, and R&D environments not part of the production ad system;
- end-user devices;
- open internet and access-network infrastructure not directly operated or contracted by the reporter; and
- non-advertising business activities.

Excluded emissions within the declared core boundary MUST NOT exceed 5.0% of raw core emissions.

## 9. In-Scope Infrastructure Categories

A conformant file MUST disclose the inclusion status of materially used infrastructure categories through the schema's `included_categories` and `excluded_categories` fields.

In-scope categories include:

- servers;
- accelerators;
- storage;
- networking;
- backup power;
- power distribution;
- cooling and mechanical systems;
- building shell and data-center fit-out; and
- edge sites used for in-scope intermediary functions.

If an in-scope category is immaterial or omitted, the reporter MUST disclose the exclusion and include any omitted emissions in the 5.0% exclusions test.

## 10. Denominator and Activity Treatment

All relevant activity supporting delivered impressions MUST be allocated to attributable delivered impressions. The numerator MUST include failed bids, auctions that did not win, duplicate path attempts, invalidated or filtered opportunities, idle capacity, reserve capacity, resiliency capacity, and shared-service overhead where those activities support delivered impressions.

The denominator MUST use the first available value in this hierarchy:

| Priority | `denominator_type` | Required minimum buffer |
|---:|---|---:|
| 1 | `reconciled_delivered_impressions` | 0% |
| 2 | `winning_events_times_trailing_settled_ratio` | 5% |
| 3 | `billable_impressions` | 10% |
| 4 | `winning_events` | 15% |

The same denominator MUST be used for the core metric and for any intensity-based supplemental metrics. The denominator count MUST relate to the same reporting period, geography, and service scope as the numerator.

## 11. Electricity Accounting Rules

The core metric MUST use location-based accounting for Scope 2 generation emissions and location-based FERA.

The following MUST NOT be included in the core metric:

- market-based Scope 2 values;
- contractual zero-emission claims;
- residual-mix adjustments intended for market-based reporting;
- offsets or renewable procurement claims netted into electricity use; and
- electricity factors that already include offsetting or removals.

Location-based Scope 2 generation factors used for `scope2_location_based_generation` MUST represent grid generation emissions only. FERA and transmission-and-distribution losses MUST be included separately in `scope3_fera_location_based`.

Reporters SHOULD use the most granular authoritative location-based factors available for the geography and reporting period. If material electricity or FERA factors are coarser than the reporting period, the quality tier and uncertainty buffer MUST reflect that limitation.

## 12. Facility Overhead and PUE Rules

A conformant file MUST disclose one PUE mode:

- `measured_iso30134_2`
- `provider_embedded`
- `billed_total_energy_no_pue_needed`
- `default_conservative`

If the reporter starts from IT electricity rather than total facility electricity, it MUST add facility overhead through measured PUE, provider-embedded facility overhead, or a conservative default.

The reporter MUST NOT apply PUE again if total facility electricity is already used or if the provider-issued result already includes facility overhead.

Where a conservative default is required, the default PUE is 1.60. Use of the default PUE makes the affected disclosure no better than Tier C and requires an additional 10% operational-method uplift inside the raw operational result before the general uncertainty buffer is applied.

Facility overhead MUST be allocated by measured IT energy where available, then by provisioned kW, rack power draw, or another capacity proxy closely related to power demand. Revenue-based allocation MUST NOT be used for facility overhead.

## 13. Embodied Emissions Rules

The core metric MUST include upstream lifecycle emissions of in-scope infrastructure assets from raw-material extraction through manufacturing and delivery to site. Installation and commissioning emissions MUST be included where they are available in the source inventory. End-of-life emissions MUST NOT be included in the core metric.

For conformant publication, separately disclosed embodied emissions MUST be normalized to these service lives:

| Asset category | Standard life |
|---|---:|
| IT equipment, accelerators, storage, rack-level networking | 4 years |
| Shared network gear when separately tracked | 5 years |
| UPS, PDU, batteries, in-row cooling, CRAC, and comparable shorter-life support equipment | 15 years |
| Chillers, cooling towers, AHU, generators, main mechanical and electrical plant, transformers, and distribution systems | 20 years |
| Building shell and structural works | 25 years |
| Combined facility category when shell and support equipment are inseparable | 20 years |

If the source inventory uses a different service life, the reporter MUST restate the source linearly:

```text
embodied_normalized = embodied_source_annualized * (source_life / standard_life)
```

Missing embodied categories MUST be disclosed, counted in the exclusions cap, and reflected in the quality tier.

Where the non-decomposed provider Scope 3 fallback in Section 15.3 is used, the reporter is not required to separately restate embedded embodied-emissions components to the service lives in this section. The reporter MUST include the full non-decomposed provider Scope 3 value without selectively removing unfavorable components, disclose the limitation in `method.source_coverage_note`, and apply the Tier C treatment required by Section 15.3.

## 14. Allocation Rules

The reporter MUST allocate emissions so that allocated emissions equal total emissions in scope for the disclosure boundary, except for disclosed exclusions within the 5.0% cap.

The allocation hierarchy is:

1. direct assignment;
2. measured physical allocation;
3. provisioned-capacity allocation;
4. operational proxy allocation; and
5. economic allocation for residual shared services only.

Physical and capacity allocation methods include metered kWh, power telemetry, CPU time, accelerator time, instance-hours, storage occupancy, bytes transferred, packets, port utilization, reserved vCPU, RAM, storage capacity, provisioned bandwidth, and provisioned kW.

Operational proxies MAY be used only where direct physical or capacity metrics are not available. Economic allocation MUST be limited to residual shared services and MUST be disclosed. Revenue allocation MUST NOT be used for facility electricity, facility embodied emissions, or dedicated infrastructure categories.

## 15. Deployment-Archetype Rules

### 15.1 Owned and Leased Data Centers

For owned and leased data centers, the reporter SHOULD use utility invoices, meter data, energy telemetry, fuel records, refrigerant records, asset registers, procurement records, LCA data, and facility-level or sub-facility allocation methods consistent with Section 14.

### 15.2 Colocation

For colocation, the reporter SHOULD use the best available source in this order:

1. metered rack or cage electricity;
2. billed electricity for the leased footprint;
3. total facility electricity allocated by rack power, contracted kW, or equivalent capacity metric.

If a colocation operator supplies a customer-specific emissions output that already includes facility overhead or embodied categories, the reporter MAY use it if it can be mapped to the carbon.json boundary, denominator, allocation, service-life, and uncertainty-buffer requirements.

### 15.3 Public Cloud and Hosted Infrastructure

Provider-issued emissions outputs MAY be used as source inputs where they can be mapped to the carbon.json core boundary, denominator rules, allocation requirements, and uncertainty-buffer requirements.

A provider-issued source input may be used in a conformant core metric if all applicable conditions are met:

1. location-based Scope 2 generation emissions are available or derivable;
2. facility overhead is embedded or can be added without double counting;
3. shared and idle capacity are allocated to the reporter or service through a conformant allocation method;
4. location-based FERA is separately disclosed or can be supplemented without double counting;
5. embodied IT, network, facility, and support infrastructure emissions are included or can be supplemented and normalized, unless the non-decomposed provider Scope 3 fallback below is used;
6. end-of-life emissions are excluded from the core metric or can be separated; and
7. any missing in-scope categories remain within the 5.0% exclusions cap.

Where a provider-issued Scope 3 value cannot be decomposed but is attributable to in-scope infrastructure, the reporter MAY use the full non-decomposed value as a conservative combined input. This fallback is an exception to separate service-life restatement for embedded embodied-emissions components that cannot be isolated from the provider-issued value. Use of this fallback makes the disclosure no better than Tier C, and the reporter MUST disclose in `method.source_coverage_note` that embedded embodied-emissions service lives could not be independently verified or restated.

### 15.4 Edge Infrastructure

Edge infrastructure MUST be included when it is directly operated or contracted by the disclosed system and used for in-scope intermediary functions, including ad selection, bid request processing, auctioning, routing, decisioning, or impression verification directly tied to the system's function.

Delivery-only infrastructure, third-party internet transit, and access-network infrastructure MUST be excluded unless the same infrastructure is directly used by the reporter for in-scope intermediary processing.

## 16. Geographic and Service-Scope Rules

A file may be labeled region-specific only if at least 90% of both raw core emissions and denominator impressions arise from the listed region or region set.

A file may be labeled service-variant-specific only if at least 90% of both raw core emissions and denominator impressions are attributable to the listed service variant.

A reporter MUST NOT publish only a low-carbon region, subfleet, or deployment archetype while presenting it as representative of a broader service.

## 17. Data Quality Tiers and Uncertainty Buffers

A conformant file MUST disclose:

- `primary_data_share_percent`
- `quality_tier`
- `uncertainty_buffer_percent`

The primary-data share MUST be emissions-weighted.

| Tier | Minimum criteria | Required buffer |
|---|---|---:|
| A | At least 80% primary or provider-specific data; reporting-period-specific activity; physical or capacity allocation; reconciled delivered impressions; complete embodied restatement; no material cost or revenue allocation | 0% |
| B | At least 60% primary or provider-specific data; reporting-period-specific operational data; one engineering proxy step; denominator no weaker than billable impressions; complete embodied restatement; no material cost or revenue allocation | 5% |
| C | At least 40% primary or provider-specific data and at least one material coarse method, default PUE, coarse grid factor, material economic allocation, winning-events denominator, combined facility-life default, or non-decomposed provider Scope 3 fallback | 15% |
| D | Less than 40% primary or provider-specific data, material location imprecision, material spend or revenue allocation, or material modeled embodied categories that remain within the exclusions cap | 30% |

The overall tier MUST be the lowest tier triggered by any material element of the method. The disclosed `uncertainty_buffer_percent` MUST be at least as high as the applicable denominator buffer and quality-tier buffer.

## 18. Publication Cadence, Recasting, and Versioning

A reporter SHOULD publish at least one annual carbon.json file for each reporting year. Monthly files MAY be published where the reporter has sufficiently granular activity, energy, and emissions data.

Annual files SHOULD be published no later than 120 calendar days after the reporting period ends. Monthly files SHOULD be published no later than 45 calendar days after month-end.

If a disclosure is recast, the new file MUST increment `record_version`, identify the superseded record where possible, and disclose the affected periods in `recast_of_periods` where applicable.

## 19. Path-Level Use

For path-level calculation, users may sum the `location_based_gross_core.published_value` for each disclosing system on the actual path.

Users MUST NOT:

- sum market-based values across systems as if they were comparable;
- sum offset-adjusted values;
- average unused candidate paths into the actual-path total; or
- replace a system's core value with a lower supplemental claim.

When multiple disclosures exist for a system, path-level users SHOULD use the most specific applicable disclosure in this order:

1. service variant and region;
2. service variant globally;
3. company global.

The use of default factors for non-disclosing systems is outside the conformance requirements for individual carbon.json disclosures in v1.0. The steward may publish default factors in future guidance. Feedback is requested on the appropriate statistical basis, governance process, and minimum disclosure count for such defaults.

## 20. Rounding and Numeric Rules

Reporters SHOULD calculate raw absolute emissions to at least 0.1 kg CO2e internally and raw intensities to at least six decimal places internally.

Published values MUST be non-negative. `raw_value` SHOULD be published to six decimal places or fewer if exact. `published_value` SHOULD be published to four decimal places.

Supplemental values that include offsets, removals, or market-based claims MUST include an explicit note that they are non-comparable and excluded from the core metric.

## 21. Assurance

Third-party assurance is OPTIONAL in v1.0.

If assurance is claimed, the disclosure MUST state the assurance level, boundary, assurance provider name, completion date, and standard used. A reporter MUST NOT imply assurance over categories outside the stated boundary.

## 22. Schema Requirements

A conformant file MUST be UTF-8 encoded JSON and MUST validate against the published carbon.json v1.0 public-feedback draft JSON Schema.

The required top-level objects are:

- `info`
- `reporter`
- `functional_unit`
- `results`
- `method`
- `quality`

`assurance` and `claims` are optional but recommended where relevant.

Where provider-issued source data is used, `cloud_providers` identifies the source provider or provider category.

## 23. Interoperability Requirements

carbon.json files SHOULD be served over HTTPS using the `application/json` content type.

Reporters SHOULD maintain stable current and historical endpoints or a manifest of prior annual and monthly files.

Consumers SHOULD treat unknown future schema versions as incompatible unless compatibility is explicitly documented.

## 24. References to Non-Normative Implementation Materials

The following repository files are non-normative:

- `docs/implementation-guide.md`
- `docs/quickstart-cloud.md`
- `docs/conformance-checklist.md`
- `examples/`
- `sample-data/`
- `references.md`

If non-normative guidance conflicts with this specification or `schema.json`, this specification and `schema.json` control for conformance to the public-feedback draft.
