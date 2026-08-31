# New Zealand Information Security Manual in OSCAL Format

## Introduction

The New Zealand Information Security Manual (NZISM) is the security and compliance framework for the NZ public sector and those in its supply chain.

The authoritative source for NZISM is:

https://nzism.gcsb.govt.nz/ism-document

The Open Security Controls Assessment Language (OSCAL) was developed by the National Institute of Standards and Technology (NIST) to enable automation of risk management and compliance framework based on security controls and functional requirements, such as SOC 2, FedRAMP, ISO-27001, StateRAMP, CMMC, HIPAA, and PCI. OSCAL is an open machine-readable information exchange format that enables tools to interoperate. More info on OSCAL is available at:

https://oscal.io

The GCSB does not publish an OSCAL version of the NZISM. They publish the web version, PDF, XML and CSV.

We wrote some scripts which create OSCAL outputs from the GCSB published XML version of NZISM. In this repository we provide:

- NZISM v3.9 (November 2025) OSCAL catalog in JSON, XML and YAML
- NZISM v3.9 (November 2025) per-classification OSCAL profiles
- NZISM v3.9 (November 2025) resolved OSCAL profile catalogs for all classification levels

While *unofficial*, we have published this openly to support NZISM-focused security and compliance engineers and governance, risk and compliance analysts in using compliance automation tooling.

If you have questions - raise an issue or send us an email.

## About this repository

This repository is a passive publishing destination — no build or sync logic lives here. The OSCAL artifacts in this repository are produced by the scripts in the sibling `nzism-oscal-builder` project and pushed here by that project. If you are interested in those scripts, please contact the maintainer.

## Contents

_Last updated: 2026-08-08_

Files are named `nzism-v<nzism-version>-<artifact>.<format>` throughout, so multiple NZISM revisions can coexist in one repository without collision. All artifacts target OSCAL 1.1.2.

### Base catalogs

The full NZISM as a single OSCAL catalog, with two variants:

| Variant           | Description                                                                                          | JSON                                          | XML                                          | YAML                                          |
| ----------------- | ---------------------------------------------------------------------------------------------------- | --------------------------------------------- | -------------------------------------------- | --------------------------------------------- |
| Controls only     | 1,422 controls across 23 chapters. Rationale text is omitted.                                        | `nzism-v3.9-catalog.json`                     | `nzism-v3.9-catalog.xml`                     | `nzism-v3.9-catalog.yaml`                     |
| With rationale    | Same controls, plus 1,136 NZISM Rationale paragraphs attached to their block as `overview` parts.    | `nzism-v3.9-catalog-with-rationale.json`      | `nzism-v3.9-catalog-with-rationale.xml`      | `nzism-v3.9-catalog-with-rationale.yaml`      |

### Per-classification profiles and resolved profile catalogs

For each of the five NZ classification levels, an OSCAL profile document (listing the applicable control IDs) and a resolved profile catalog (the filtered catalog itself, with empty groups pruned):

| Classification level         | Profile                                                              | Resolved profile catalog                                                              |
| ---------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Unclassified / In-Confidence | `nzism-v3.9-unclassified-baseline-profile.{json,xml,yaml}`           | `nzism-v3.9-unclassified-baseline-resolved-profile-catalog.{json,xml,yaml}`           |
| Restricted / Sensitive       | `nzism-v3.9-restricted-baseline-profile.{json,xml,yaml}`             | `nzism-v3.9-restricted-baseline-resolved-profile-catalog.{json,xml,yaml}`             |
| Confidential                 | `nzism-v3.9-confidential-baseline-profile.{json,xml,yaml}`           | `nzism-v3.9-confidential-baseline-resolved-profile-catalog.{json,xml,yaml}`           |
| Secret                       | `nzism-v3.9-secret-baseline-profile.{json,xml,yaml}`                 | `nzism-v3.9-secret-baseline-resolved-profile-catalog.{json,xml,yaml}`                 |
| Top Secret                   | `nzism-v3.9-top-secret-baseline-profile.{json,xml,yaml}`             | `nzism-v3.9-top-secret-baseline-resolved-profile-catalog.{json,xml,yaml}`             |

The profiles use cumulative semantics: a Top Secret profile includes every control at Top Secret and every lower classification level, matching how NZ Government systems are actually operated.

Control counts per level:

| Level         | Controls |
| ------------- | -------: |
| Unclassified  | 1,212    |
| Restricted    | 1,216    |
| Confidential  | 1,376    |
| Secret        | 1,377    |
| Top Secret    | 1,422    |

### NZISM-specific vocabulary

Each control carries the following OSCAL `<prop>` elements under the namespace `https://nzism.gcsb.govt.nz/ns/oscal` (except `label` and `sort-id`, which use the default OSCAL namespace):

- `label` — the original NZISM ISM-R identifier (e.g. `18.1.9.C.01.`)
- `sort-id` — the identifier with numeric segments zero-padded (`18.01.09.C.01`)
- `nzism-cid` — the NZISM CID
- `classification` — one prop per applicable classification level
- `requirement-level` — Must / Should / Must Not / Should Not
- `topic` — the NZISM block title
- `tag` — one prop per thematic tag from the source XML

Consumers that don't recognise the NZISM namespace can safely ignore these props.

## Validation

The generator produces well-formed OSCAL but does not validate against the OSCAL schemas itself. If you're integrating with downstream tooling, validate first with `oscal-cli`, `compliance-trestle`, or NIST's online OSCAL validator.

## Copyright

The New Zealand Information Security Manual (NZISM) is copyright © New Zealand Government Communications Security Bureau (GCSB).

The OSCAL derivative artifacts in this repository are created by independent security and compliance engineer Baden Hughes and are made available under the terms of the Creative Commons Attribution 4.0 New Zealand license, in accordance with the licensing terms published by GCSB at https://nzism.gcsb.govt.nz/legal-privacy-and-copyright.
