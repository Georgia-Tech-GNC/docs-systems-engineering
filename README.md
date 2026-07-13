# Systems Engineering Practices

This repository defines the club-wide systems engineering practices used to manage requirements, risks, reviews, verification evidence, and flight-readiness decisions. It is process documentation, not a program record: individual rocket programs keep their own requirements, risks, evidence, baselines, configuration history, and review dispositions in their program repositories.

The near-term goal is to turn the draft requirements and risk plan into concise, reusable guidance that project teams can apply without creating documentation bloat.

## Repository Role

- Define the common process for mission definition, requirements development, risk management, verification planning, configuration records, and design reviews.
- Define the expected repository architecture for program-specific documentation repositories.
- Define shared metadata, status values, review gates, baseline rules, and validation expectations.
- Provide templates and examples that project repositories can copy and tailor.
- Avoid storing project-specific decisions, requirement records, risk records, test evidence, or flight-readiness records here.

## Repository Structure

- `docs/repository-model.md`: defines what belongs in this club-wide process repository versus program-specific repositories.
- `docs/controlled-artifacts.md`: defines the controlled artifact vocabulary used by project teams and reviews.
- `docs/baselines-and-change-control.md`: defines baseline semantics, review disposition handling, and change-control expectations.
- `docs/reviews.md`: defines MCR, SRR, PDR, CDR, TRR, FRR, and the expected state after each review.
- `docs/authoring.md`: defines requirements, risks, rationale, verification, metadata, and validation guidance.
- `templates/`: contains reusable starting points for program artifacts such as concept of operations, interface-control documents, configuration records, reports, checklists, and flight-readiness records.

## Current Focus

The current working order is tracked in `TODO.md`. The immediate priority is to define the repository model first, then the controlled artifact vocabulary, baseline/change-control rules, and review definitions.
