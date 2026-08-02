# StrictDoc Integration

This document defines how authors use StrictDoc and the `syseng` command
in program repositories.

StrictDoc-independent writing rules are defined in [Authoring](authoring.md).
Repository ownership is defined in [Repository Model](repository-model.md).
Implementation details for the command belong in the `syseng-tools`
repository.

## Source Of Truth

Program repositories store mission objectives, mission constraints,
requirements, and risks as StrictDoc `.sdoc` source files.

Generated HTML, JSON, Excel, and risk reports are derived views. They may be
used for review, but they do not define the controlled baseline.

## Syseng CLI

Authors use `syseng` instead of invoking StrictDoc directly.

Run commands from the program repository root:

```text
syseng [--project-root PATH] COMMAND [OPTIONS]
```

`--project-root PATH` runs the command against a program repository other than
the current directory.

### `syseng check`

```text
syseng check [--warnings-as-errors]
```

`syseng check` runs StrictDoc parsing and automated checks. Run it before
reviewing or committing controlled record changes. Automated record checks are
listed in [Automated Record Checks](automated-record-checks.md).

`--warnings-as-errors` returns a failing exit code when warnings are reported.

### `syseng export`

```text
syseng export [--output-dir PATH]
```

`syseng export` generates StrictDoc HTML, JSON, Excel, and the `syseng-tools`
risk register from the current `.sdoc` files.

By default, StrictDoc output is written to `build/strictdoc`. The risk register
is written to `build/syseng`.

`--output-dir PATH` changes the StrictDoc output directory. The risk register
still uses the program repository's `build/syseng` directory.

### `syseng serve`

```text
syseng serve [--host HOST] [--port PORT]
```

`syseng serve` starts a local preview of the generated static site from
`build/strictdoc/html`. It serves existing generated HTML only.

If the generated HTML does not exist, `syseng serve` fails with a helpful
message telling the author to run `syseng export`.

`--host HOST` changes the network interface. The default is `127.0.0.1`.

`--port PORT` changes the preview port. The default is `8000`.

### `syseng risk`

```text
syseng risk [--strictdoc-json PATH] [--output-dir PATH]
```

`syseng risk` regenerates only the risk register from existing StrictDoc JSON
data.

Most authors do not need to run this command directly. `syseng export`
regenerates the risk register after exporting StrictDoc data.

By default, `syseng risk` reads `build/strictdoc/json/index.json` and writes the
risk register to `build/syseng`.

`--strictdoc-json PATH` reads a different StrictDoc JSON export.

`--output-dir PATH` writes the risk register to a different output directory.

## Install Tools

Program repositories should pin `syseng-tools` in `requirements-tools.txt`.

The installation procedure is maintained in
[Georgia-Tech-GNC/syseng-tools](https://github.com/Georgia-Tech-GNC/syseng-tools).

Authors should install the pinned tool requirements before running `syseng`
commands.

## Syseng Paths

`syseng` uses these program repository paths:

```text
syseng.toml
records/*.sdoc
build/strictdoc/
build/syseng/
```

`syseng.toml` contains program configuration.

`records/*.sdoc` contains StrictDoc source records.

`build/strictdoc/` contains generated StrictDoc HTML, JSON, and Excel.

`build/syseng/` contains generated `syseng-tools` output, including the risk
register.

Program repositories depend on `syseng-tools`. Do not copy Python checks,
grammar files, or report generators into program repositories.

## Program Configuration

Program repositories commit `syseng.toml` at the repository root.

Example:

```toml
project_title = "Gimbaled TVC"
project_prefix = "TVC"
records_dir = "records"
allowed_vehicle_configurations = ["TVC-F1"]
allowed_mission_phases = ["Powered ascent"]
allowed_flight_attempts = ["Flight 1"]
```

`project_title` is the display title for generated documentation.

`project_prefix` is the UID prefix enforced by `syseng check`.

`records_dir` is the directory containing StrictDoc `.sdoc` record files. It
defaults to `records` when omitted.

`allowed_vehicle_configurations` lists allowed values for
`VEHICLE_CONFIGURATION`, in addition to `All`.

`allowed_mission_phases` lists allowed values for `MISSION_PHASE`, in addition
to `All`.

`allowed_flight_attempts` lists allowed values for `FLIGHT_ATTEMPT`, in
addition to `All`.

The allowed-value lists are optional in TOML syntax, but program repositories
should define them so `syseng check` can reject misspelled or unapproved
applicability values.

`syseng` fails when:

- `syseng.toml` is missing
- `project_title` is missing or an empty string
- `project_prefix` is missing or an empty string
- `records_dir` is missing and the default `records/` directory does not exist
- `records_dir` is an empty string
- `records_dir` does not exist
- an allowed-value list is not a list of non-empty strings

Program repositories do not commit a full `strictdoc_config.py` unless a
program-specific exception is approved.

`syseng check` and `syseng export` generate the StrictDoc configuration they
need under `build/syseng/`.

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

`syseng check` enforces when those fields are required.

Examples include:

- `RESOLUTION_OWNER`
- `RESOLUTION_PLAN`
- `RESOLUTION_DUE_DATE`
- `VERIFICATION_RESULT_LINK`
- `WAIVER_LINK`
- `RESPONSE_PLAN`
- `DUE_DATE`
- `DISPOSITION_NOTES`

Authors do not need to write placeholder values such as `Not required` for
these fields. The grammar defines them as optional, and `syseng check` rejects
placeholder values when the field is not needed.

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

## Risk Rendering

Risk records author the controlled claim as:

```text
CONDITION
CONSEQUENCE
```

StrictDoc HTML renders ordinary grammar fields as small metadata rows. It
renders content fields as larger text blocks.

The risk grammar includes an optional `CONTENT` field before `CONDITION` so
StrictDoc treats `CONDITION` and `CONSEQUENCE` as large content fields instead
of rendering the entire risk record as large text.

Authors do not populate `CONTENT` in risk records.

`syseng check` fails when `RISK.CONTENT` is populated.

Generated reports may combine condition and consequence into the displayed risk
statement:

```text
If [condition], then [consequence].
```

## Risk Calculation

Authors provide likelihood and severity values.

Authors do not provide risk level values.

`syseng-tools` calculates initial and current risk levels from likelihood
and severity according to [Authoring](authoring.md#risk-scoring).

Calculated risk levels appear in generated risk reports. They are not stored in
`.sdoc` source files.

## Display Values

StrictDoc choice values use values that the parser accepts reliably.

When StrictDoc requires token-safe values, generated reports map them back to the
human-readable vocabulary defined by the systems-engineering process.

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
