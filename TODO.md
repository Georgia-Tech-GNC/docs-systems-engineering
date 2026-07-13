# Documentation TODOs

This is the working backlog for turning the draft requirements and risk plan into club-wide systems engineering practice. Complete an item when the process it governs is needed; prefer concise guidance and reusable templates over empty documents.

## First, remove review ambiguity

- [x] **Define repository responsibilities** in `docs/repository-model.md`: what belongs in this club-wide process repository versus what belongs in each program-specific repository.
- [x] **Define the controlled artifact vocabulary** in `docs/controlled-artifacts.md`: mission objectives, mission constraints, requirements, concept of operations, interface-control documentation, verification matrix, risk register, configuration records, analysis/test reports, operations checklists, and flight-readiness checklist.
- [ ] **Define baseline semantics** in `docs/baselines-and-change-control.md`: when baselines are created, what an annotated baseline tag contains, why tags are immutable, how post-review dispositions are merged, and how changes between baselines are approved.
- [ ] **Define open-item handling** in `docs/baselines-and-change-control.md`: allowed use of TBD, TBR, and TBS; required resolution owner, plan, and due date; and which reviews may proceed with unresolved items.
- [ ] **Define applicability conventions** in `docs/authoring.md`: how requirements, risks, evidence, waivers, and configuration records identify the vehicle configuration, mission phase, and flight attempt to which they apply.

## Define the review ladder

- [ ] **Define MCR, SRR, PDR, CDR, TRR, and FRR** in `docs/reviews.md`. For each review, state its purpose, participants, required inputs, required repository state, decisions to make, exit criteria, disposition process, approver, and resulting baseline tag.
- [ ] **Define the requirements baseline after each review**: which requirement levels must exist, which fields must be populated, which open items are allowed, which verification expectations must be present, and which requirement statuses are acceptable.
- [ ] **Define the risk baseline after each review**: which risks must exist, which scores and responses must be assigned, which high or accepted risks require lead approval, and what residual-risk basis must be recorded.
- [ ] **Define review disposition rules**: how action items are captured, who may close them, what must be merged before baseline tagging, and how unresolved review findings are carried forward.

## Define authoring and validation guidance

- [ ] **Create a concise authoring reference** in `docs/authoring.md`: requirement and risk fields, allowed values, field applicability, ID allocation, relation usage, and one good example of each record type.
- [ ] **Document requirement-writing rules**: use of `shall`, single responsibility, measurable limits, applicable conditions, objective verification, implementation-neutral language, and accepted requirement types.
- [ ] **Document rationale expectations**: source, consequence, basis for quantitative limits or assumptions, and special handling for provisional values.
- [ ] **Document risk-writing rules**: risk types, cause and consequence fields, likelihood/severity scoring, current versus initial risk, response plans, and disposition notes.
- [ ] **Specify automated semantic checks**: required fields, UID rules, relation integrity, TBD/TBR/TBS resolution data, risk-level calculations, and warning-only sentence-template checks.

## Define program-repository workflow

- [ ] **Define the recommended repository layout and local commands** in `CONTRIBUTING.md`: where requirements, risks, evidence, configuration records, templates, and generated output belong; how to install pinned tools; and how to validate and preview changes.
- [ ] **Define the contribution workflow** in `CONTRIBUTING.md`: branch, commit, and pull-request conventions; required reviewers; PR contents; baseline changes; and any expedited path for time-critical corrections.
- [ ] **Define StrictDoc architecture**: shared grammar fields, relation conventions, generated HTML, verification matrix generation, and CI expectations.
- [ ] **Decide how project starters are created**: whether this repository should provide a template directory, copied starter files, or a separate starter repository for new rocket programs.

## Define templates before tests and flight

- [ ] **Create a minimal interface-control template** covering mechanical, electrical, power, data/software, ownership, and verification information needed for integration.
- [ ] **Create minimal analysis and test-report templates** covering objective, configuration, method, result, linked requirements/risks, evidence location, anomalies, and conclusion.
- [ ] **Create a configuration-record template** identifying exact hardware, software revision, parameters, calibration state, and setup used for each test or flight.
- [ ] **Define verification-evidence handling**: procedure and evidence naming, storage/linking, reviewer expectations, failure and re-test handling, waivers, and the rule that flight-critical safety requirements receive initial verification before flight.
- [ ] **Create operations and flight-readiness checklist templates** with owner, vehicle/configuration identity, sign-offs, hold points, abort criteria, and links to applicable requirements and risks.
- [ ] **Define the minimum concept-of-operations content** needed to describe mission phases, actors, system states, nominal flow, off-nominal behavior, and success criteria without duplicating requirements.

## After the core workflow is proven

- [ ] Publish generated StrictDoc HTML for read-only member access; decide separately whether browser editing is worth supporting.
- [ ] Confirm that generated verification matrices and traceability views are sufficient; add custom reporting only for a demonstrated review need.
- [ ] Decide which parts of the proven grammar, CI, templates, and guidance should become reusable starters for future rocket-program repositories.
