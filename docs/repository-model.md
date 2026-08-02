# Repository Model

This document defines where GNC systems engineering information belongs.

## Repository Types

The club uses two repository types for systems engineering work:

- A club-wide process repository.
- Program-specific repositories.

## Club-Wide Process Repository

`docs-systems-engineering` (this repository) is the club-wide process repository.

It owns reusable guidance that should apply across rocket programs:

- Review and baseline rules.
- Authoring and change-control rules.
- Mission-validation rules.
- Controlled vocabulary for artifacts and review states.
- Templates and starter material for program repositories.

It changes when the club changes how systems engineering work should be done.

## Program-Specific Repositories

A program-specific repository is the source of truth for one rocket program.

It owns the program records:

- Mission objectives, mission constraints, requirements, and risks.
- Program-specific concept of operations, interface definitions, reports, records, procedures, and checklists.
- Review dispositions, baseline tags, verification status, evidence links, and configuration history.

It changes when the program record changes.

## Program Boundaries

Each materially distinct rocket program should have its own repository.

A rocket program is materially distinct when the vehicle has a distinct mission and/or a distinct control approach. For example, a gimbaled-TVC vehicle and a fin-tabs vehicle should use separate repositories.

A program should stay in one repository as its capabilities evolve. Versioned baselines and applicability metadata should distinguish vehicle configurations, mission phases, and flight attempts.

## Source Data And Generated Views

Program repositories should store requirements and risks as source data. StrictDoc `.sdoc` files are the expected source format once the `syseng-tools` grammar is defined.

Generated outputs are review views derived from source data. StrictDoc HTML exports, traceability views, and verification matrices may be published for review, but they are not the controlled source.

## Program Repository Layout

Program repositories use this layout:

```text
syseng.toml
requirements-tools.txt
records/
  mission.sdoc
  requirements.sdoc
  risks.sdoc
docs/
  conops/
  interfaces/
  verification/
  reviews/
  configuration/
build/
  strictdoc/
  syseng/
```

`syseng.toml` contains program configuration for `syseng-tools`.

`requirements-tools.txt` pins the program repository tooling.

`records/` contains controlled record source data, including mission records,
requirements, and risks.

`docs/` contains supporting controlled artifacts and evidence.

`build/` contains generated output. Generated output may support review, but it
does not define the controlled baseline.

Program repositories own the program-specific records, supporting artifacts,
baselines, and generated review outputs.

StrictDoc and `syseng` usage for these paths is defined in
[StrictDoc Integration](strictdoc-integration.md).
