# StrictDoc Integration

This document defines how authors use StrictDoc and the shared `syseng` command
in program repositories.

StrictDoc-independent writing rules are defined in [Authoring](authoring.md).
Repository ownership is defined in [Repository Model](repository-model.md).
Implementation details for the shared command belong in the `syseng-tools`
repository.

## Source Of Truth

Program repositories store mission objectives, mission constraints,
requirements, and risks as StrictDoc `.sdoc` source files.

Generated HTML, JSON, Excel, and risk reports are derived views. They may be
used for review, but they do not define the controlled baseline.

## Shared CLI

Authors use the shared `syseng` command instead of invoking StrictDoc directly.

The initial command set is:

```text
syseng check
syseng export
syseng serve
syseng risk
```

`syseng check` runs StrictDoc parsing and shared automated checks. Run it before
reviewing or committing controlled record changes. Automated record checks are
listed in [Automated Record Checks](automated-record-checks.md).

`syseng export` generates StrictDoc HTML, JSON, Excel, and shared reports from
the current `.sdoc` files.

`syseng serve` starts a local preview of the generated static site from
`build/strictdoc/html`.

If the generated HTML does not exist, `syseng serve` should fail with a helpful
message telling the author to run `syseng export`.

`syseng risk` generates the risk register from StrictDoc data.

Program documentation may provide shorter wrappers, but the shared command name
is `syseng`.

## Install Tools

Program repositories should pin `syseng-tools` in `requirements-tools.txt`.

The installation procedure is maintained in
[Georgia-Tech-GNC/syseng-tools](https://github.com/Georgia-Tech-GNC/syseng-tools).

Authors should install the pinned tool requirements before running `syseng`
commands.

## Repository Layout

Program repositories use this layout:

```text
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

`records/` contains controlled record source data.

`docs/` contains supporting controlled artifacts and evidence.

`build/strictdoc/` contains generated StrictDoc output.

`build/syseng/` contains generated shared-tooling output.

Program repositories own the program-specific `.sdoc` records, supporting
artifacts, baselines, and generated review outputs.

Program repositories depend on the shared tooling package. Do not copy shared
Python checks, grammar files, or report generators into program repositories.

## Program Configuration

Program repositories commit a small `syseng.toml` file for project-specific
configuration.

The committed configuration contains program facts such as:

- project title
- project prefix
- records directory

Program repositories do not commit a full `strictdoc_config.py` unless a
program-specific exception is approved.

The `syseng` command generates the StrictDoc configuration it needs under
`build/syseng/`.

Authors edit `syseng.toml` when program-level settings change. Authors do not
edit generated StrictDoc configuration files.

## Record Tags

StrictDoc records use three node tags:

```text
MISSION_RECORD
REQUIREMENT
RISK
```

Mission objectives and mission constraints use the same `MISSION_RECORD` tag.
The `RECORD_TYPE` field identifies whether the record is an `Objective` or a
`Constraint`.

Requirements use the `REQUIREMENT` tag.

Risks use the `RISK` tag.

## Optional Conditional Fields

Fields that are required only under certain conditions are optional in the
StrictDoc grammar.

The shared `syseng check` command enforces when those fields are required.

Examples include:

- `RESOLUTION_OWNER`
- `RESOLUTION_PLAN`
- `RESOLUTION_DUE_DATE`
- `VERIFICATION_RESULT_LINK`
- `WAIVER_LINK`
- `RESPONSE_PLAN`
- `DUE_DATE`
- `DISPOSITION_NOTES`

Do not write placeholder values such as `Not required` only to satisfy the
StrictDoc grammar.

## Relations

Requirements use StrictDoc relations to identify parent mission records or
parent requirements.

Risks use StrictDoc relations to identify affected requirements:

```text
RELATIONS:
- TYPE: Parent
  VALUE: TVC-REQ-001
  ROLE: Affects
```

The reverse role is rendered as `Affected by risk`.

Supporting artifacts remain ordinary fields until the shared tooling proves that
StrictDoc file relations are suitable for program use.

## Risk Rendering

Risk records author the controlled claim as:

```text
CONDITION
CONSEQUENCE
```

The risk grammar includes an optional `CONTENT` field before `CONDITION` so
StrictDoc renders only condition and consequence as large content fields.

Authors do not populate `CONTENT` in risk records.

The shared `syseng check` command fails when `RISK.CONTENT` is populated.

Generated reports may combine condition and consequence into the displayed risk
statement:

```text
If [condition], then [consequence].
```

## Risk Calculation

Authors provide likelihood and severity values.

Authors do not provide risk level values.

The shared tooling calculates initial and current risk levels from likelihood
and severity according to [Authoring](authoring.md#risk-scoring).

Calculated risk levels appear in generated risk reports. They are not stored in
`.sdoc` source files.

## Display Values

StrictDoc choice values use values that the parser accepts reliably.

When StrictDoc requires token-safe values, generated reports map them back to the
human-readable vocabulary defined by the shared process.

Example:

```text
Flight_Software -> Flight Software
Ground_Systems -> Ground Systems
N_A -> N/A
```

The grammar uses `HUMAN_TITLE` so generated StrictDoc pages display field labels
in readable form.

## Tooling Boundary

The `syseng` command is provided by the `syseng-tools` repository.

This document owns author-facing usage. The `syseng-tools` README owns package
maintenance details such as local development setup, command internals, tests,
and release mechanics.

## Future Work

Evaluate a canonical schema after the hand-maintained grammar becomes stable.

The canonical schema should generate `program.sgra` and remove duplicated common
field definitions.

Evaluate editable web authoring after the text-based workflow and shared command
set are proven.
