# Controlled Artifacts

This document defines the artifact types used by club systems engineering practices.

Detailed requirement fields, risk fields, and verification types belong in `docs/authoring.md`. Baseline and change-control rules belong in `docs/baselines-and-change-control.md`. Review-specific readiness rules belong in `docs/reviews.md`.

## Mission Objective

A mission objective is a Level 0 statement of what the mission is intended to demonstrate, accomplish, or evaluate.

It is used to define mission success before flight and evaluate mission success afterward.

## Mission Constraint

A mission constraint is a Level 0 boundary the mission or vehicle must satisfy.

It is used to capture external rules and safety limits. It can also capture resource limits or approved program boundaries.

## Requirement

A requirement is a controlled statement of what the system or a subsystem must satisfy.

It is used to align design work, verification planning, and review decisions.

## Risk

A risk is a controlled statement of a credible future condition that could harm safety or mission success.

It can also capture threats to technical performance, schedule, or resources.

It is used to decide what must be watched or reduced.

It is also used to record when a risk is avoided or accepted.

## Concept of Operations

A concept of operations describes how the mission is expected to run.

It is used to define mission phases and system states.

It also defines nominal and off-nominal behavior.

## Interface-Control Document

An interface-control document defines an interface between systems, subsystems, or external equipment.

It is used to prevent subteams from making incompatible assumptions at integration boundaries.

## Verification Matrix

A verification matrix is a generated view of requirements and their verification status.

It is used to review whether each requirement has a planned method, linked verification work, and final result.

## Verification Plan

A verification plan defines how the team will confirm that the system or subsystem satisfies one or more requirements.

It restates the verification method from the requirement record.

It defines acceptance criteria, required configuration, and expected verification output.

For test, inspection, and demonstration, the plan may be written as a procedure or checklist. For analysis, it may be written as an analysis plan.

## Configuration Record

A configuration record identifies the exact vehicle state used for a test or flight.

It records hardware and software identity.

It also records settings and setup details.

It is used to make verification evidence traceable to the vehicle state that produced it.

## Analysis Report

An analysis report records how an analysis was performed.

It also records the result and conclusion.

It is used to support requirement verification, risk handling, or design decisions.

## Test Report

A test report records what was tested and how it was configured.

It also records what happened and what conclusion was reached.

It is used to support requirement verification, risk handling, or readiness decisions.

## Flight-Readiness Checklist

A flight-readiness checklist records the decision that a vehicle is ready to attempt flight.

It is used to confirm that required verification and risk disposition are complete.

It also confirms that configuration records and approvals are complete.

It authorizes launch operations to commence. It does not replace the launch operations checklist.

## Operations Checklist

An operations checklist is a controlled sequence of actions for operating the vehicle or support equipment.

It is used to execute an approved activity in the correct order.

It records required actions, hold points, abort criteria, and operational signoffs.