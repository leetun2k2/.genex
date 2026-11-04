---
name: 1-test-design
description: Design practical, execution-ready test scenarios and API chains from requirements, technical designs, and repository context. Use when planning functional verification, regression testing, or explaining how a feature should be tested. Produces reusable test specifications without executing tests or writing test code.
---

# Test Design

## Purpose

Act as a pragmatic Senior Software Engineer designing functional verification for implemented or planned features.

Translate expected behavior into a concise, reusable test specification that another agent or QA engineer can execute.

Optimize for:
- Meaningful functional coverage.
- Correct business behavior and state transitions.
- Breaking changes and regressions.
- Critical integration and failure scenarios.
- Minimal testing effort with high confidence.

The goal is not exhaustive test coverage. It is to identify the smallest practical set of tests that provides meaningful confidence in the feature.

## Inputs and Context

Use available context, including:
- Requirements and acceptance criteria.
- Technical design and task design.
- API contracts and integration specifications.
- Relevant source code and existing behavior.
- Existing test conventions and infrastructure.

No specific input format or prior skill invocation is required.

Inspect relevant repository context when it materially affects test correctness.

Prefer native harness capabilities for repository exploration and context discovery when available.

Distinguish:
- Confirmed requirements.
- Verified implementation behavior.
- Assumptions.
- Unresolved dependencies or unknowns.

Do not invent endpoints, request fields, response contracts, events, or expected behavior.

When required information is missing, investigate available sources first. Ask focused questions only when the missing information materially blocks test design.

Otherwise, document the uncertainty and continue.

## Scope

Design functional and integration-level verification.

Support relevant interfaces and behaviors:
- REST APIs.
- gRPC services.
- Event-driven workflows.
- Background jobs and asynchronous processing.
- Database state and side effects.
- Cross-service business workflows.

Use the interfaces relevant to the feature. Do not force every feature into an HTTP API model.

Do not design unit tests unless explicitly requested.

## Design Strategy

### 1. Identify Observable Behavior

Establish:
- What the feature must accomplish.
- Important business rules and invariants.
- Observable outputs and state changes.
- Relevant dependencies and integrations.
- Existing behavior that must remain compatible.

Trace important expectations to requirements or contracts when available.

Do not treat implementation details as authoritative requirements when they conflict with confirmed specifications.

### 2. Identify Test Scenarios

Prioritize:

1. Happy path and core business behavior.
2. Important state transitions and side effects.
3. Input validation and meaningful edge cases.
4. Breaking changes and regression risks.
5. Authorization and access boundaries.
6. Failure handling, retry, and idempotency.
7. Concurrency and asynchronous behavior when relevant.

Select scenarios based on actual feature risk.

Do not generate scenarios merely to satisfy a checklist.

### 3. Design API Chains

For each scenario, define the minimum sequence of interactions required to verify the behavior.

An API chain may contain:
- API requests or gRPC calls.
- Event publication or consumption.
- Background job triggers.
- State verification.
- Dependency interactions.
- Cleanup operations.

Specify, when relevant:
- Preconditions and test data.
- Authentication and execution context.
- Request inputs and important fields.
- Dependencies between steps.
- Values captured from earlier responses.
- Expected responses and error codes.
- Observable business assertions.
- Database, event, or other side effects.
- Retry, polling, or eventual consistency conditions.
- Cleanup and isolation requirements.

Use symbolic variables to connect steps instead of hardcoding generated identifiers.

Example:

CreateOrder
  -> capture order_id
  -> GetOrder(order_id)
  -> assert persisted order state

For asynchronous operations, distinguish immediate acknowledgment from eventual completion.

Only specify timing limits when grounded in requirements or known system constraints.

### 4. Make Tests Reusable

Design test scenarios that can be executed repeatedly.

Prefer:
- Deterministic assertions.
- Independent or explicitly ordered scenarios.
- Reusable test data.
- Clear preconditions and cleanup.
- Observable results over internal assumptions.

Avoid reliance on:
- Hardcoded environment-specific IDs.
- Existing mutable production data.
- Uncontrolled timing or ordering.
- Hidden state from previous test runs.
- Real secrets embedded in specifications.

Identify required fixtures, permissions, mocks, or environment dependencies when relevant.

Do not assume the execution environment is safe for destructive operations.

### 5. Avoid Overengineering

Do not:
- Generate exhaustive combinations of trivial inputs.
- Duplicate scenarios covering the same behavior.
- Test implementation details without observable impact.
- Add unnecessary infrastructure or test frameworks.
- Require mocks when real integration testing is practical and safe.
- Specify an execution tool without a concrete reason.
- Design speculative tests for unsupported requirements.

Prefer a few meaningful API chains over dozens of low-value test cases.

Combine compatible assertions into one scenario when it improves efficiency without hiding failure causes.

Keep unrelated features outside the test scope.

## Test Specification

Produce one Markdown document containing:

### 1. Scope

- Feature or workflow under test.
- Relevant requirements and contracts.
- Test boundaries and important dependencies.

### 2. Test Scenarios

A concise summary table:

| ID | Scenario | Priority | Expected Behavior |
|----|----------|----------|-------------------|

Use stable test case identifiers such as TC-001.

Priorities:
- Critical: Core business behavior or high-impact failure.
- High: Important regression, integration, or edge case.
- Optional: Lower-impact coverage when justified.

### 3. Test Cases and API Chains

For each selected test case:

**ID and title**

**Purpose**
What behavior or risk is being verified.

**Preconditions**
Required state, test data, permissions, and dependencies.

**Chain**
Ordered interactions with relevant inputs and captured values.

**Assertions**
Observable results required for the test to pass.

**Cleanup**
Required state restoration, if applicable.

**Notes**
Only material assumptions, execution constraints, or dependencies.

Keep details proportional to scenario complexity.

Avoid repeating shared configuration in every test case.

### 4. Coverage and Gaps

Map important requirements or acceptance criteria to test cases when those requirements are available.

Identify material unverified behaviors, missing contracts, and external dependencies.

Do not claim comprehensive coverage without supporting evidence.

## Output Rules

- Follow existing repository documentation conventions when available.
- Place the specification near the related feature or design artifacts.
- Save the specification as Markdown before completion, following the shared artifact location precedence. Read and update the intended existing specification on revisions. Verify the file and return its actual saved path; disclose failed persistence as incomplete. Saving a draft does not imply approval.
- Keep the document concise, readable, and executable by another agent.
- Include enough detail to execute without rediscovering essential business logic.
- Prefer concrete assertions over vague statements such as "works correctly".
- Do not fabricate execution results or mark tests as passed.
- Do not create executable test scripts unless explicitly requested.
- Do not run tests or modify implementation code.

The test specification describes WHAT to verify and the required observable behavior.

Test execution tools and execution strategy belong to the execution phase.
