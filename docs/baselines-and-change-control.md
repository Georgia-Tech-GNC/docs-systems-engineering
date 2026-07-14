# Baselines And Change Control

This document defines how program repositories create baselines, change approved records, and manage open items.

Review-specific entry and exit criteria belong in `docs/reviews.md`.

Pull-request format, reviewer workflow, local validation commands, and exact baseline tagging commands belong in `CONTRIBUTING.md`.

## Current Approved State

`main` is the current approved state of a program repository.

Changes enter `main` through pull requests.

A pull request records the proposed change and affected artifacts. It also records the review discussion and approval.

## Baseline

A baseline is an approved snapshot of a program repository.

It is used when a review or major decision needs a stable reference. The baseline identifies the exact source state that was accepted.

## Review Baseline

A review baseline is an annotated Git tag on the commit accepted after a review.

Review baseline tags use this format:

```text
baseline-mcr-YYYY-MM-DD
baseline-srr-YYYY-MM-DD
baseline-pdr-YYYY-MM-DD
baseline-cdr-YYYY-MM-DD
baseline-trr-YYYY-MM-DD
baseline-frr-YYYY-MM-DD
```

The date is the date the baseline is approved, not necessarily the date the review meeting began.

## When A Baseline Is Created

A review baseline is created after review dispositions are resolved and merged.

Do not tag the commit presented at the review if the review produced required changes. Tag the commit that includes the accepted dispositions.

## Annotated Tag Contents

The annotated tag message should state the review name and approval date.

It should also identify unresolved exceptions that were accepted at the review. Detailed review findings belong in the program repository, not in the tag message.

## Immutability

A baseline tag is immutable.

Do not move, delete, or reuse a baseline tag after it has been published. If the baseline contains an error, correct the repository through the normal change process.

## Changes Between Baselines

Changes between baselines require pull-request approval.

The approver must be a systems lead or an affected subteam lead. Program-specific rules may require additional approval for safety-critical changes, high-risk acceptance, or flight-readiness changes.

## Open Items

An open item records missing, provisional, or externally supplied information in a controlled record.

Use `TBR` instead of `TBD` when a reasonable provisional value can be stated.

## Open-Item Types

`TBD` means To Be **Determined**. The required value or information is not yet known.

`TBR` means To Be **Resolved**. A provisional value or statement needs more work before approval. That work may be analysis, testing, or review.

`TBS` means To Be **Supplied**. The required information must be supplied by someone else. The source may be another person, subteam, vendor, or external organization.

## Open-Item Resolution

Each open item must have a resolution owner.

Each open item must have a resolution plan and due date.

The resolution plan must state what work will close the open item.

Resolving an open item is a change to the controlled record. It must be reviewed and merged through the normal change process.

## Open Items In Baselines

Open items may remain in a baseline only when the applicable review definition allows them or the review explicitly accepts them as exceptions.

Review-specific open-item tolerance belongs in `docs/reviews.md`.

## Generated Outputs

Generated outputs may be attached to a review or published for browsing.

They do not define the baseline. The baseline is the tagged source commit in the program repository.
