# Automated Record Checks

This document lists automated checks for controlled records.

Authoring rules are defined in [Authoring](authoring.md). StrictDoc integration
is defined in [StrictDoc Integration](strictdoc-integration.md).

## Check IDs

Every automated check has a stable `CHK-###` identifier.

| ID range | Enforced by | Meaning |
| --- | --- | --- |
| `CHK-0XX` | StrictDoc | Native StrictDoc parsing, grammar, and traceability checks. |
| `CHK-1XX` | Shared tooling | Checks implemented by `syseng check` because StrictDoc cannot enforce the rule directly. |

The ID range identifies the enforcement source. It does not change the expected
diagnostic quality.

Each check result should include the shortest useful path to fixing the issue:

- check ID
- result type
- affected UID, when known
- file and line, when known
- specific missing field, invalid value, or broken target

Example:

```text
CHK-008 ERROR TVC-REQ-014 records/requirements.sdoc:58: relation target does not exist: TVC-REQ-999.

CHK-111 ERROR TVC-REQ-014 records/requirements.sdoc:42: open item status is TBD, but resolution plan is missing.
```

## Result Types

`syseng check` reports these result types:

| Result | Meaning |
| --- | --- |
| Error | The record should not be accepted into a controlled baseline. |
| Warning | The record may be acceptable, but a reviewer should inspect it. |

## StrictDoc-Native Checks

These checks are detected by StrictDoc when shared tooling runs StrictDoc
parsing or export.

`syseng check` should map known StrictDoc diagnostics to these IDs and preserve
the affected UID, file, and line when available.

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-001` | Error | StrictDoc source files | `.sdoc` and `.sgra` files parse successfully. |
| `CHK-002` | Error | Documents | Imported grammar files resolve. |
| `CHK-003` | Error | Records | Record tags are defined by the active grammar. |
| `CHK-004` | Error | Records | Field names are defined for the record tag. |
| `CHK-005` | Error | Records | Required grammar fields are present. |
| `CHK-006` | Error | Choice fields | Field values match the allowed `SingleChoice` or `MultipleChoice` options. |
| `CHK-007` | Error | Relations | Relation syntax matches the active grammar. |
| `CHK-008` | Error | Relations | Relation target UIDs exist. |
| `CHK-009` | Error | Records | UIDs are unique across the StrictDoc project. |
| `CHK-010` | Error | Documents | Included documents can be loaded as one StrictDoc project. |

StrictDoc may emit diagnostics that are not yet mapped to a `CHK-0XX` ID.
Shared tooling should still report those diagnostics with the most precise
available location.

## Shared Tooling Checks

These checks depend on project configuration, project-specific vocabularies,
relations, conditional fields, generated values, file existence, or text
patterns.

### Identifier Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-101` | Error | All records | UID uses the configured project prefix. |
| `CHK-102` | Error | Mission records | `Objective` records use `[PROJECT]-MO-[NNN]`; `Constraint` records use `[PROJECT]-MC-[NNN]`. |
| `CHK-103` | Error | Requirements | Requirement UIDs use `[PROJECT]-REQ-[NNN]`. |
| `CHK-104` | Error | Risks | Risk UIDs use `[PROJECT]-RISK-[NNN]`. |
| `CHK-105` | Error | All records | UID number is exactly three digits. |
| `CHK-106` | Warning | All records | Title contains three to six words. |

### Applicability Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-107` | Error | All records | Vehicle configuration is either `All` or a value defined by the program repository. |
| `CHK-108` | Error | All records | Mission phase is either `All` or a value defined by the program repository. |
| `CHK-109` | Error | All records | Flight attempt is either `All` or a value defined by the program repository. |
| `CHK-110` | Error | All records | Applicability fields do not use `N/A`. |

The program repository owns the allowed values for vehicle configuration,
mission phase, and flight attempt.

### Open Item Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-111` | Error | All records with open item fields | `RESOLUTION_OWNER`, `RESOLUTION_PLAN`, and `RESOLUTION_DUE_DATE` are present when `OPEN_ITEM_STATUS` is `TBD`, `TBR`, or `TBS`. |
| `CHK-112` | Error | All records with open item fields | Resolution fields are absent when `OPEN_ITEM_STATUS` is `None`. |

### Requirement Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-113` | Error | Requirements | Requirement statement contains `shall`. |
| `CHK-114` | Warning | Requirements | Requirement statement does not use `shall not` unless the requirement type is `Design Constraint` or `Safety`. |
| `CHK-115` | Error | L1 requirements | Parent relation points to a mission record. |
| `CHK-116` | Error | L2 requirements | Parent relation points to an L1 requirement. |
| `CHK-117` | Error | Requirements | Requirement has exactly one parent relation. |
| `CHK-118` | Error | Requirements with status `Verified` or `Failed` | `VERIFICATION_RESULT_LINK` is present. |
| `CHK-119` | Error | Requirements with status `Waived` | `WAIVER_LINK` is present. |
| `CHK-120` | Error | Requirements without status `Waived` | `WAIVER_LINK` is absent. |
| `CHK-121` | Error | Requirements | Verification and waiver links are repository-relative paths. |
| `CHK-122` | Warning | Requirements | Populated verification and waiver links resolve to files in the current repository state. |
| `CHK-123` | Error | Requirements | `Applicable subsystem` does not combine `N/A` with any other subsystem value. |

### Mission Record Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-124` | Warning | Mission objectives | Statement starts with an accepted mission-objective pattern. |
| `CHK-125` | Warning | Mission constraints | Statement starts with an accepted mission-constraint pattern. |
| `CHK-126` | Warning | Mission constraints | At least one requirement traces to the mission constraint. |

### Risk Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-127` | Error | Risks | `CONTENT` is absent. |
| `CHK-128` | Error | Risks | Condition does not start with `If`. |
| `CHK-129` | Error | Risks | Consequence does not start with `then`. |
| `CHK-130` | Error | Risks | Risk has at least one `Affects` relation to a requirement. |
| `CHK-131` | Error | Risks | Every `Affects` relation points to a requirement. |
| `CHK-132` | Error | Risks with response `Watch`, `Mitigate`, or `Avoid` | `RESPONSE_PLAN` is present. |
| `CHK-133` | Error | Risks with response `Watch`, `Mitigate`, or `Avoid` | `DUE_DATE` is present. |
| `CHK-134` | Error | Risks with status `Accepted`, `Closed`, or `Retired` | `DISPOSITION_NOTES` is present. |
| `CHK-135` | Error | Risks | Linked artifact paths are repository-relative paths. |
| `CHK-136` | Warning | Risks | Populated linked artifact paths resolve to files in the current repository state. |
| `CHK-137` | Error | Risks | Risk response and status match an allowed combination from [Authoring](authoring.md#allowable-response-status-combinations). |

Risk levels are generated values. Authors do not write risk level fields in
`.sdoc` source.

### Placeholder Checks

| ID | Result | Applies to | Rule |
| --- | --- | --- | --- |
| `CHK-138` | Error | Optional link, owner, plan, date, and notes fields | Optional fields do not contain placeholder values such as `Not required`, `N/A`, `None`, or `TBD` when the field is not required. |
| `CHK-139` | Warning | Free-text fields | Free-text fields do not contain unresolved placeholders such as `[TBD]`, `TODO`, or `FIXME`. |

## Human Review Items

These rules come from [Authoring](authoring.md), but automated checks can only
assist review:

| Topic | Human review question |
| --- | --- |
| Rationale quality | Does the rationale explain why the record exists and the consequence of weakening or omitting it? |
| Open item rationale | Does the rationale explain the uncertainty when the record contains `TBD`, `TBR`, or `TBS`? |
| Resolution plan quality | Does the resolution plan identify the work, source, or decision needed to remove the open item? |
| Requirement scope | Does the requirement state one responsibility without hidden implementation detail? |
| Requirement measurability | Is the requirement objectively verifiable? |
| Requirement conditions | Are conditions, modes, and phases stated where they affect compliance? |
| Requirement wording | Is a negative requirement the clearest controlled claim? |
| Risk specificity | Are the condition and consequence specific enough to score and assign to an owner? |
| Risk response quality | Does the selected response match the actual plan? |
| Watch response quality | Does the response plan state what is monitored and what trigger causes action? |
| Mitigation response quality | Does the response plan identify the action that reduces likelihood or severity? |
| Avoidance response quality | Does the response plan identify the mission, design, or plan change that eliminates the risk? |
| Waiver applicability | Is the waiver applicability the same as or narrower than the requirement applicability? |
| High risk disposition | Has project-lead review occurred before accepting or leaving a high risk unresolved? |
