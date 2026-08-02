# Authoring

This document defines how controlled records and their metadata are written.

Artifact definitions are authoritative in [Controlled Artifacts](controlled-artifacts.md). This document defines how those artifacts are written.

## Common Record Rules

The UID is the only unique identifier for a controlled record. Titles are short human-readable labels, not identifiers. Claim fields contain the controlled assertion. Metadata fields provide the context needed to review, maintain, and use the claim.

### Record Classes

A program repository uses four controlled record classes:

| Record class | Authoritative definition | Authoring rules |
| --- | --- | --- |
| Mission objective | [Mission Objective](controlled-artifacts.md#mission-objective) | This document |
| Mission constraint | [Mission Constraint](controlled-artifacts.md#mission-constraint) | This document |
| Requirement | [Requirement](controlled-artifacts.md#requirement) | This document |
| Risk | [Risk](controlled-artifacts.md#risk) | This document |

Mission objectives and mission constraints are not considered requirements. L1 and L2 records are requirements.

### UID Rules

Each controlled record has a permanent UID.

| Record class | Format | Example |
| --- | --- | --- |
| Mission objective | `[PROJECT]-MO-[NNN]` | `TVC-MO-001` |
| Mission constraint | `[PROJECT]-MC-[NNN]` | `TVC-MC-001` |
| Requirement | `[PROJECT]-REQ-[NNN]` | `TVC-REQ-042` |
| Risk | `[PROJECT]-RISK-[NNN]` | `TVC-RISK-006` |

The project prefix identifies the project. The middle token (MO, MC, REQ, RISK) identifies the record class. The number gives the record a stable identity.

DO NOT REUSE UIDs.

If a controlled record changes meaning, create a new record and close the old one. Requirements are closed as `Superseded`. Risks are closed as `Retired`, with a link to the replacement risk when one exists.

### Standard Subteams

Use these subteam-owner values unless a program repository defines an approved extension:

| Subteam |
| --- |
| Systems |
| Structures |
| Electrical |
| Software |
| Controls |
| Aerodynamics |

### Standard Subsystems

Subsystem values identify the part of the vehicle or support system constrained by a record.

Subsystem values do not identify who owns the work. Use subteam owner for responsibility.

Use these subsystem values unless a program repository defines an approved extension:

| Subsystem | Use |
| --- | --- |
| Airframe | Body structure, fins, aerodynamic surfaces, and airframe-side mechanical interfaces. |
| Propulsion | Motor, motor retention, ignition hardware, and thrust-transfer structure. |
| Actuation | Actuators, mechanical transmission, mechanical stops, and local position sensing. |
| Avionics | Flight computer, sensors, telemetry hardware, data storage, and avionics signal wiring. |
| Power | Batteries, power distribution, switching, regulation, and power isolation. |
| Flight Software | Onboard software for estimation, control, flight-state management, fault response, command handling, and logging. |
| Recovery | Parachutes, deployment mechanisms, recovery electronics, harnesses, and landing provisions. |
| Ground Systems | Ground control, launch support equipment, test fixtures, telemetry receiving, and post-flight tools. |
| N/A | Used only when subsystem allocation would be misleading. |

Select the tag based on the constrained item, not the physical location of that item.

Use multiple subsystem tags for true interfaces.

Tag dedicated electronics by subsystem function. Tag general flight-computer, telemetry, sensing, logging, and signal wiring as `Avionics`.

Use `Power` when the controlled claim is about electrical power.

Use `N/A` only when subsystem tagging would be false precision.

Examples:

| Case | Subsystem tag |
| --- | --- |
| Recovery altimeter used only for deployment | `Recovery` |
| IMU used by flight software for state estimation | `Avionics` |
| Actuator motor controller | `Actuation` |
| Battery feeding the avionics and actuator bus | `Power` |
| Flight computer to actuator command interface | `Avionics`, `Actuation` |

## Mission Records

### Mission Objectives

Mission objectives are defined in [Controlled Artifacts](controlled-artifacts.md#mission-objective).

Use a measurable statement when possible:

```text
Demonstrate that the [vehicle/system] can [mission-level capability] under [defined conditions].

Achieve [measurable mission outcome] under [defined conditions].

Determine whether the [vehicle/system] can [capability or behavior] under [defined conditions].
```

### Mission Constraints

Mission constraints are defined in [Controlled Artifacts](controlled-artifacts.md#mission-constraint).

Use one of these statement patterns:

```text
The [mission/vehicle] shall comply with [external rule, standard, permit, or authority] for [defined scope].

The [mission/vehicle] shall remain within [mission-level limit] during [defined phase or condition].

The [mission] shall use [required site, facility, date range, or external resource] for [defined activity].
```

Use the compliance pattern only when the constraint comes from an external rule, standard, permit, or authority.

Each mission constraint should trace to one or more requirements that make the constraint enforceable.

A mission constraint is a Level 0 boundary. A design-constraint requirement is an L1 or L2 restriction allocated to the system or a subsystem.

### Mission Record Fields

Mission objectives and mission constraints use the same fields. Only the statement pattern differs.

#### Identifier

| Field | Allowed values or format |
| --- | --- |
| UID | `[PROJECT]-MO-[NNN]` or `[PROJECT]-MC-[NNN]` |

#### Label

| Field | Allowed values or format |
| --- | --- |
| Title | Three to six word summary |

#### Controlled Claim

| Field | Allowed values or format |
| --- | --- |
| Statement | Objective or constraint statement |

#### Metadata

| Field | Allowed values or format |
| --- | --- |
| Rationale | One to three sentences |
| Subteam owner | One or more standard subteam values |
| Individual owner | Named person or role |
| Vehicle configuration | Controlled value defined by the program repository |
| Mission phase | Controlled value defined by the program repository |
| Flight attempt | Controlled value defined by the program repository |
| Open item status | `None`, `TBD`, `TBR`, `TBS` |
| Resolution owner | Required when open item status is not `None` |
| Resolution plan | Required when open item status is not `None` |
| Resolution due date | Required when open item status is not `None` |
| Status | `Draft`, `Approved`, `Retired`, `Superseded` |

Requirements share these metadata fields and add requirement-specific fields.

Shared metadata fields are explained in [Shared Metadata](#shared-metadata).

## Requirements

### Requirement Statements

Requirements are defined in [Controlled Artifacts](controlled-artifacts.md#requirement).

Each requirement must:

| Rule | Meaning |
| --- | --- |
| Use `shall` | The statement must be mandatory. |
| Name the responsible element | The reader must know what subsystem(s) is/are responsible. |
| State one responsibility | Split combined requirements into separate records. |
| State applicable conditions | Conditions, modes, and phases belong in the statement when they affect compliance. |
| Be objectively verifiable | Do not approve a requirement unless the team can explain how it will be verified. |

Use measurable limits when practical. Avoid vague terms.

### Avoid Implementation Detail

Requirements state what must be true. They do not state how the team will make it true.

Implementation belongs in design artifacts unless the implementation choice is itself mandatory. If the implementation choice is mandatory, write a design-constraint requirement and explain the basis in the rationale.

Do not smuggle a preferred solution into a functional or performance requirement.

Bad Example:

```text
The flight software shall use a complementary filter to estimate vehicle attitude during powered ascent.
```

Problem: this may be a valid design choice, but the requirement states the implementation before proving that the implementation is mandatory.

Better:

```text
The flight software shall estimate vehicle attitude during powered ascent with pitch and yaw error no greater than 0.5 degrees.
```

### Avoid Negative Requirements

Avoid `shall not`.

Negative requirements are easy to misread and hard to verify completely. Prefer a positive requirement that states the required safe, compliant, or bounded behavior.

Use `shall not` only when the prohibited condition is the clearest controlled claim.

Bad Example:

```text
The recovery system shall not deploy the main parachute during powered ascent.
```

Problem: the statement controls a prohibited outcome, but it does not state the required inhibit condition or the allowed deployment condition.

Better:

```text
The recovery system shall inhibit main parachute deployment until the vehicle has detected apogee.
```

### Requirement Types

A requirement may fit more than one type. Choose the type that best describes its primary purpose.

| Type | Use |
| --- | --- |
| Functional | States what the system or subsystem must do. |
| Performance | States how well a function must be performed. |
| Interface | States a required interaction or compatibility boundary. |
| Physical | States a required physical property. |
| Design Constraint | States a mandatory design restriction. |
| Safety | States behavior or characteristics that prevent or mitigate a hazard. |
| Environmental | States required function, performance, or survival under a defined external environment. |

### Accepted Sentence Patterns

The patterns below are the expected requirement sentence formats. A requirement may use another format only when these patterns do not fit the controlled claim.

Use subject-first patterns rather than condition-first patterns UNLESS the condition is the trigger for the required response.

#### Functional

```text
The [system/subsystem] shall [perform the required function].
When [event or condition] occurs, the [system/subsystem] shall [required response].
The [system/subsystem] shall [perform the required function] while in [state, mode, or mission phase].
```

#### Performance

```text
The [system/subsystem] shall maintain [parameter] within [quantitative limit] during [defined conditions].
After [event] occurs, the [system/subsystem] shall complete [function] within [time].
The [system/subsystem] shall provide [at least/no more than] [quantity] under [defined conditions].
The [system/subsystem] shall operate over the range [minimum] to [maximum].
```

#### Interface

```text
The [element] shall provide [signal, data, power, or mechanical connection] to [other element] with [defined characteristics].
The [element] shall accept [input] from [other element] within [defined limits].
The [element] shall be compatible with [external system or defined interface].
```

#### Physical

```text
The [system/subsystem] shall have [physical characteristic] within [quantitative limit].
The [system/subsystem] shall fit within [defined envelope].
The [system/subsystem] shall withstand [load] under [defined conditions] with [required margin or factor of safety].
The [system/subsystem] shall maintain [mass property, geometry, or location] within [defined bounds].
```

#### Design Constraint

```text
The [system/subsystem] shall use [required design choice].
The [system/subsystem] shall not use [prohibited design choice].
The [system/subsystem] shall comply with [standard, rule, or mandated architecture].
The interface between [element A] and [element B] shall use [protocol, connector, geometry, or standard].
```

#### Safety

```text
When [fault or hazardous condition] occurs, the [system/subsystem] shall [required response] within [limit].
The [system/subsystem] shall prevent [hazardous outcome] unless [required conditions].
No single [failure, command, or action] shall cause [hazardous outcome].
The [system/subsystem] shall inhibit [hazardous function] while [unsafe condition].
```

#### Environmental

```text
The [system/subsystem] shall meet [specified performance] while exposed to [quantified environment].
After exposure to [environment], the [system/subsystem] shall [required condition or performance].
```

### Requirement Fields

The requirement statement is the controlled claim.

#### Identifier

| Field | Allowed values or format |
| --- | --- |
| UID | `[PROJECT]-REQ-[NNN]` |

#### Label

| Field | Allowed values or format |
| --- | --- |
| Title | Three to six word summary |

#### Classification

| Field | Allowed values or format |
| --- | --- |
| Level | `L1`, `L2` |
| Type | `Functional`, `Performance`, `Interface`, `Physical`, `Design Constraint`, `Safety`, `Environmental` |

#### Controlled Claim

| Field | Allowed values or format |
| --- | --- |
| Statement | Requirement statement |

#### Metadata

| Field | Allowed values or format |
| --- | --- |
| Parent | UID of the parent mission record or requirement |
| Rationale | One to three sentences |
| Applicable subsystem | One or more standard subsystem values |
| Subteam owner | One or more standard subteam values |
| Individual owner | Named person or role |
| Vehicle configuration | Controlled value defined by the program repository |
| Mission phase | Controlled value defined by the program repository |
| Flight attempt | Controlled value defined by the program repository |
| Open item status | `None`, `TBD`, `TBR`, `TBS` |
| Resolution owner | Required when open item status is not `None` |
| Resolution plan | Required when open item status is not `None` |
| Resolution due date | Required when open item status is not `None` |
| Verification method | `Test`, `Analysis`, `Inspection`, `Demonstration` |
| Verification plan link | Link to the artifact that defines how verification will be performed |
| Verification result link | Link to the artifact containing the verification evidence |
| Waiver link | Required when status is `Waived` |
| Status | `Draft`, `Approved`, `Verified`, `Failed`, `Waived`, `Retired`, `Superseded` |

Child requirements and related risks should be derived from relations when the tool supports it.

### Requirement Status

| Status | Meaning |
| --- | --- |
| Draft | Written but not approved. |
| Approved | Accepted into the requirements baseline. |
| Verified | Objective evidence shows the requirement is satisfied. |
| Failed | Verification showed noncompliance. |
| Waived | Approved exception permits noncompliance. |
| Retired | No longer applicable. |
| Superseded | Replaced by another requirement. |

### Waivers

Waivers are defined in [Controlled Artifacts](controlled-artifacts.md#waiver).

The requirement owns the waiver link.

The waiver link points to the waiver artifact.

The waiver applicability must be the same as or narrower than the requirement applicability.

Do not use a waiver to rewrite the requirement.

If a requirement cannot clearly represent both waived and non-waived applicability scopes, split the requirement.

Waiver approval rules belong in [Reviews](reviews.md) or the applicable program review procedure.

### Verification Methods

The requirement record is the source of truth for verification method.

| Method | Meaning |
| --- | --- |
| Test | The team operates or measures the item under controlled conditions. The result directly shows whether the requirement is met. |
| Analysis | The team uses calculation, simulation, model evaluation, or interpretation of existing data to show compliance. |
| Inspection | The team examines the product, schematics, records, or configuration without operating the item. |
| Demonstration | The team observes the item performing the required function. |

Test-derived data may support analysis. Use `Analysis` only when the compliance conclusion comes from the analytical result, not from the raw test result alone.

Use `Test` when the controlled operation or measurement directly determines whether the requirement is satisfied.

Examples:

| Case | Verification method | Reason |
| --- | --- | --- |
| A bench test measures actuator step response and compares the measured rise time directly to the requirement limit. | `Test` | The measurement directly determines compliance. |
| Static-fire thrust data is used to update a simulation that predicts ascent stability margin. | `Analysis` | The compliance conclusion comes from the model result. |
| A reviewer confirms that the avionics harness uses the required connector keying. | `Inspection` | The item is examined without operation. |
| The team observes that telemetry packets appear on the ground station during an end-to-end checkout. | `Demonstration` | The required function is observed in operation. |

### Verification Links

A requirement record must link to the verification plan when the plan exists.

After verification, the requirement record must link to the verification result.

The verification result records the pass/fail conclusion and links to the evidence used to reach that conclusion.

Evidence may be a report, dataset, photo, log, or signed checklist.

Do not embed verification evidence in the requirement record.

### Flight-Critical Safety Requirements

A flight-critical safety requirement is a safety requirement whose failure could create an unsafe launch, flight, recovery, or ground-operation condition.

No flight-critical safety requirement may depend on flight for initial verification.

Do not use `Demonstration` as the only planned verification method when failure during the demonstration could create an unsafe condition. Verify the requirement first by test, analysis, or inspection under controlled conditions.

## Shared Metadata

### Applicability

Applicability identifies the scope for which a record is valid.

Applicability is represented by three metadata fields:

| Field | Use |
| --- | --- |
| Vehicle configuration | Identifies the hardware and software configuration. |
| Mission phase | Identifies the phase where the record applies. |
| Flight attempt | Identifies the flight or test campaign where the record applies. |

Use `All` only when the record truly applies across the full field.

Do not use `N/A` to avoid making an applicability decision.

### Rationale

Every mission objective, mission constraint, and requirement must have a rationale.

A rationale explains why the record exists.

#### Required Content

A rationale must identify the source of the record.

For L1 and L2 requirements, the source is usually the parent record.

Include the consequence of weakening or omitting the record when that consequence is not obvious.

State the basis for any quantitative value, substantive assumption, or mandatory design restriction.

Do not use the rationale to restate the record, describe the verification procedure, or include detailed calculations. Link supporting work instead.

If the record contains `TBD`, `TBR`, or `TBS`, the rationale must explain the uncertainty.

#### Good Patterns

Mission objective rationale:

```text
This objective supports the program goal of demonstrating closed-loop attitude control during powered ascent. Without it, the flight would not establish whether the vehicle can maintain a commanded attitude profile under motor thrust.
```

Mission constraint rationale:

```text
This constraint is required by the selected launch site's range safety rules. Without it, the mission cannot be approved for launch at that site.
```

Derived requirement rationale:

```text
This requirement supports TVC-MO-001 by providing the attitude estimate needed for closed-loop control. Without it, the vehicle could not determine whether corrective actuation is required during powered ascent.
```

Quantitative value rationale:

```text
This requirement supports TVC-REQ-001 by ensuring the logged data is fast enough to reconstruct the control response after flight. The 50 Hz minimum is based on the expected control-loop bandwidth and post-flight analysis needs.
```

Design-constraint rationale:

```text
This requirement supports TVC-REQ-018 by requiring the connector standard used by the avionics harness. The restriction prevents incompatible harness revisions during integration.
```

TBR rationale:

```text
This requirement supports TVC-REQ-021 by setting a provisional actuator bandwidth target for controller design. The value is TBR because it is based on vendor data and must be confirmed by bench testing before CDR.
```

#### Bad Examples

Bad Example:

```text
This requirement supports TVC-MO-001 by improving attitude-control performance during ascent.
```

Problem: this names a parent objective, but it does not explain what capability would be lost if the requirement were weakened or omitted.

Bad Example:

```text
The 50 Hz logging rate gives the team enough data to evaluate the flight.
```

Problem: this mentions the value, but it does not state where the value came from.

Bad Example:

```text
This requirement will be verified by reviewing the flight log after recovery.
```

Problem: this describes verification instead of explaining why the requirement exists.

### Open Items

Open items and their types are defined in [Baselines and Change Control](baselines-and-change-control.md#open-items).

Use these field values:

| Status | Use |
| --- | --- |
| `None` | No open item exists. |
| `TBD` | Open item exists (To be Determined). |
| `TBR` | Open item exists (To be Resolved). |
| `TBS` | Open item exists (To be Supplied). |

### Resolution Plans

A resolution plan states the specific work that will remove the open item.

For `TBD`, state how the missing value will be determined.

For `TBR`, state what must confirm, revise, or approve the provisional value.

For `TBS`, state who or what source must provide the missing information.

Preferred formats:

| Status | Format |
| --- | --- |
| `TBD` | Determine `[missing value]` by completing `[work product or decision]`. |
| `TBR` | Confirm or revise `[provisional value]` by completing `[work product or approval]`. |
| `TBS` | Obtain `[missing information]` from `[source]`. |

Bad Resolution Plans:

| Bad plan | Problem |
| --- | --- |
| `Determine later.` | Does not identify the work. |
| `Ask the team.` | Does not identify the responsible source. |
| `Do analysis.` | Does not identify the analysis output. |
| `Review before launch.` | Does not identify the decision needed to close the item. |

## Risks

### Risk Claim

Risks are defined in [Controlled Artifacts](controlled-artifacts.md#risk).

A risk claim is written as two authored fields:

```text
Condition: [condition that could make the risk occur]
Consequence: [harm if the risk occurs]
```

The condition and consequence are the source of truth.

The displayed risk statement is generated from those fields:

```text
If [condition], then [consequence].
```

Do not write `If` in the condition field. Do not write `then` in the consequence field.

The condition and consequence must be specific enough to score and assign to an owner.

Do not write a concern as a task. Create a task only after the risk response is selected.

Bad Example:

```text
Condition: Actuator bandwidth testing is not completed before CDR.
Consequence: The vehicle may fail to maintain commanded attitude during powered ascent.
```

Problem: this makes an unfinished mitigation task the condition. The risk should state the technical condition that could harm the mission.

Better:

```text
Condition: Actuator bandwidth is lower than assumed.
Consequence: The vehicle may fail to maintain commanded attitude during powered ascent.
```

### Risk Types

A risk may fit more than one type. Choose the type that best describes its primary consequence.

| Type | Use |
| --- | --- |
| Safety | Could injure people, damage property, or violate safety rules. |
| Mission Success | Could prevent a mission objective from being achieved or evaluated. |
| Technical | Could prevent the design from meeting requirements. |
| Schedule | Could delay a critical milestone. |
| Resource | Could exceed available budget, staffing, or facilities. |

### Accepted Claim Patterns

The patterns below are the expected condition and consequence formats. A risk may use another format only when these patterns do not fit the controlled claim.

Each condition/consequence pair is a single accepted pattern. Do not mix condition and consequence lines from different patterns.

#### Safety

```text
Condition: [hazardous condition or failure] occurs.
Consequence: [person, property, or vehicle] may [harmful consequence].

Condition: [required control] fails.
Consequence: [hazardous outcome] may occur during [activity].
```

#### Mission Success

```text
Condition: [condition] occurs.
Consequence: [mission objective] may not be achieved or evaluated.

Condition: The [system/subsystem] cannot [required capability].
Consequence: [mission-level outcome] may be degraded or lost.
```

#### Technical

```text
Condition: [technical assumption, component, or performance] is incorrect or insufficient.
Consequence: [requirement or function] may not be satisfied.

Condition: [interface or configuration] is incompatible with [affected element].
Consequence: [integration or operation] may fail.
```

#### Schedule

```text
Condition: [dependency, resource, or work product] is delayed.
Consequence: [milestone, review, or test] may be delayed.

Condition: [decision or information] is not available by [date or milestone].
Consequence: [work] may be blocked.
```

#### Resource

```text
Condition: [budget, staffing, or facility] is unavailable or insufficient.
Consequence: [planned work] may be delayed or reduced.

Condition: [vendor or source] cannot provide [item or service].
Consequence: [scope, schedule, or design] may be affected.
```

### Risk Fields

The risk condition and consequence are the controlled claim.

#### Identifier

| Field | Allowed values or format |
| --- | --- |
| UID | `[PROJECT]-RISK-[NNN]` |

#### Label

| Field | Allowed values or format |
| --- | --- |
| Title | Three to six word summary |

#### Classification

| Field | Allowed values or format |
| --- | --- |
| Type | `Safety`, `Mission Success`, `Technical`, `Schedule`, `Resource` |

#### Controlled Claim

| Field | Allowed values or format |
| --- | --- |
| Condition | Condition that could make the risk occur |
| Consequence | Harm if the risk occurs |

#### Metadata

| Field | Allowed values or format |
| --- | --- |
| Subteam owner | One or more standard subteam values |
| Individual owner | Named person or role |
| Vehicle configuration | Controlled value defined by the program repository |
| Mission phase | Controlled value defined by the program repository |
| Flight attempt | Controlled value defined by the program repository |
| Initial likelihood | `Low`, `Medium`, `High` |
| Initial severity | `Low`, `Medium`, `High` |
| Initial risk level | Calculated from likelihood and severity |
| Current likelihood | `Low`, `Medium`, `High` |
| Current severity | `Low`, `Medium`, `High` |
| Current risk level | Calculated from likelihood and severity |
| Risk response | `Watch`, `Mitigate`, `Avoid`, `Accept` |
| Response plan | Required unless the response is `Accept` |
| Due date | Required unless the response is `Accept` |
| Linked requirements | Requirement UIDs affected by the risk |
| Linked artifacts | Artifacts related to the risk or response |
| Status | `Open`, `Accepted`, `Closed`, `Retired` |
| Disposition notes | Required for `Accepted`, `Closed`, and `Retired` |

Common linked artifacts include analysis reports, test reports, interface definitions, and design decisions.

### Risk Scoring

Likelihood answers how likely the risk is to occur.

| Likelihood | Score | Meaning |
| --- | --- | --- |
| Low | 1 | Unlikely, but credible. |
| Medium | 2 | Plausible. |
| High | 3 | Likely unless action is taken. |

Severity answers how harmful the consequence would be.

| Severity | Score | Meaning |
| --- | --- | --- |
| Low | 1 | Minor redesign, small delay, or limited mission impact. |
| Medium | 2 | Major redesign, missed test, or degraded mission success. |
| High | 3 | Safety issue, launch cancellation, mission failure, or major hardware loss. |

Risk level is likelihood multiplied by severity.

| Risk level | Score |
| --- | --- |
| Low | 1-2 |
| Medium | 3-4 |
| High | 6-9 |

Risk level does not choose the response automatically. It constrains how casually the risk may be handled.

High risks require project-lead review before they may be accepted or left unresolved.

### Risk Responses

| Response | Meaning |
| --- | --- |
| Watch | Track the risk without immediate mitigation. |
| Mitigate | Take action to reduce likelihood or severity. |
| Avoid | Change the mission, design, or plan to eliminate the risk. |
| Accept | Consciously tolerate the residual risk without further reduction. |

A watched risk must state what is being monitored and what trigger causes action.

A mitigated risk must identify the concrete action that reduces likelihood or severity.

An avoided risk must identify the mission, design, or plan change that eliminates the risk.

An accepted risk does not require a response plan. It requires disposition notes that state why the residual risk is acceptable and who accepted it.

### Risk Status

| Status | Meaning |
| --- | --- |
| Open | Applicable and not yet in a terminal disposition. |
| Accepted | Still applicable, but consciously tolerated. |
| Closed | Resolved, avoided, disproven, or reduced enough to stop active tracking. |
| Retired | No longer applicable because the context changed. |

Accepted, Closed, and Retired risks require disposition notes.

### Allowable Response-Status Combinations

Risk response identifies the selected handling strategy.

Risk status identifies the current lifecycle state.

Use these response and status combinations:

| Status | Allowed responses |
| --- | --- |
| `Open` | `Watch`, `Mitigate`, `Avoid` |
| `Accepted` | `Accept` |
| `Closed` | `Watch`, `Mitigate`, `Avoid` |
| `Retired` | `Watch`, `Mitigate`, `Avoid` |

## Complete Record Examples

Example mission objective:

```text
UID: TVC-MO-001
Title: Controlled Powered Ascent
Statement: Demonstrate that the vehicle can maintain vertical attitude during powered ascent under nominal launch conditions.
Rationale: This objective supports the program goal of demonstrating thrust-vector control in flight. Without it, the mission would not show whether the control approach works under ascent loads.
Subteam owner: Systems
Individual owner: Systems Lead
Vehicle configuration: TVC-F1
Mission phase: Powered ascent
Flight attempt: Flight 1
Open item status: None
Resolution owner: Not required
Resolution plan: Not required
Resolution due date: Not required
Status: Approved
```

Example mission constraint:

```text
UID: TVC-MC-001
Title: Range Safety Compliance
Statement: The vehicle shall comply with applicable range safety rules for the selected launch site.
Rationale: This constraint is required by the launch authority. Without it, the mission cannot be approved for flight.
Subteam owner: Systems
Individual owner: Systems Lead
Vehicle configuration: All
Mission phase: All
Flight attempt: All
Open item status: None
Resolution owner: Not required
Resolution plan: Not required
Resolution due date: Not required
Status: Approved
```

Example L1 requirement:

```text
UID: TVC-REQ-001
Title: Powered Ascent Attitude Estimate
Level: L1
Type: Functional
Statement: The vehicle shall estimate roll, pitch, and yaw attitude during powered ascent.
Parent: TVC-MO-001
Rationale: This requirement supports TVC-MO-001 by providing the state estimate needed for attitude control. Without it, controlled ascent cannot be evaluated.
Applicable subsystem: Avionics, Flight Software
Subteam owner: Electrical, Software, Controls
Individual owner: Systems Lead
Vehicle configuration: TVC-F1
Mission phase: Powered ascent
Flight attempt: Flight 1
Open item status: None
Resolution owner: Not required
Resolution plan: Not required
Resolution due date: Not required
Verification method: Analysis
Verification plan link: docs/verification/attitude-estimate-analysis.md
Verification result link: Not required until verification is complete
Waiver link: Not required
Status: Approved
```

Example L2 requirement:

```text
UID: TVC-REQ-014
Title: Attitude Log Rate
Level: L2
Type: Performance
Statement: The flight software shall log attitude estimate data at no less than 50 Hz during powered ascent.
Parent: TVC-REQ-001
Rationale: This requirement supports post-flight evaluation of TVC-REQ-001. The 50 Hz rate is based on the expected control-loop bandwidth and reviewable post-flight data needs.
Applicable subsystem: Flight Software
Subteam owner: Software, Controls
Individual owner: Software Lead
Vehicle configuration: TVC-F1
Mission phase: Powered ascent
Flight attempt: Flight 1
Open item status: None
Resolution owner: Not required
Resolution plan: Not required
Resolution due date: Not required
Verification method: Test
Verification plan link: docs/verification/flight-logging-test.md
Verification result link: Not required until verification is complete
Waiver link: Not required
Status: Approved
```

Example risk:

```text
UID: TVC-RISK-003
Title: Actuator Bandwidth Shortfall
Type: Technical
Condition: Actuator bandwidth is lower than assumed.
Consequence: The vehicle may fail to maintain commanded attitude during powered ascent.
Generated display: If actuator bandwidth is lower than assumed, then the vehicle may fail to maintain commanded attitude during powered ascent.
Subteam owner: Controls
Individual owner: Controls Lead
Vehicle configuration: TVC-F1
Mission phase: Powered ascent
Flight attempt: Flight 1
Initial likelihood: Medium
Initial severity: High
Initial risk level: High
Current likelihood: Medium
Current severity: High
Current risk level: High
Risk response: Mitigate
Response plan: Complete actuator frequency-response testing and update the control model with measured bandwidth.
Due date: Before CDR
Linked requirements: TVC-REQ-001
Linked artifacts: docs/verification/actuator-frequency-response-test.md
Status: Open
Disposition notes: Not required
```
