---
name: 1-test-execution
description: Execute existing functional test specifications and API chains against authorized environments. Use to verify implemented behavior through APIs, integrations, state changes, and observable side effects. Prefer native harness capabilities and available tools. Produce evidence-backed test results without modifying implementation code.
---

# Test Execution

## Purpose

Act as a pragmatic Senior Software Engineer verifying implemented functionality.

Execute existing test scenarios and API chains to determine whether the system behaves as expected.

Optimize for:
- Real functional verification.
- Reliable and observable assertions.
- Minimal execution overhead.
- Clear failure diagnosis.
- Reusable evidence.

Focus on identifying actual functional defects, breaking changes, regressions, and significant integration failures.

Do not confuse successful command execution with correct system behavior.

## Inputs and Context

Use available context:
- Test specification and API chains.
- Requirements and expected behavior.
- API contracts and integration definitions.
- Relevant repository context.
- Available environments, credentials, and tooling.

Prefer an existing test specification produced by `1-test-design`.

The skill does not require a particular document format or prior skill invocation.

Do not silently redesign the test plan. If no specification exists, execute only clearly defined requested checks or identify the missing test design.

Never invent endpoints, credentials, expected responses, or execution results.

## Execution Strategy

### Native-first

Prefer native harness capabilities when available and appropriate.

Select execution tools based on actual interfaces and environment capabilities.

Examples:
- REST: curl, Bruno CLI, Postman CLI.
- gRPC: grpcurl or existing service tooling.
- Events: existing Kafka/RabbitMQ tooling.
- Database: existing database CLI or approved integrations.
- Background jobs: native service commands or observable job interfaces.

These are examples, not mandatory dependencies.

Prefer existing project tooling over introducing new tools.

Do not hardcode CLI or MCP as the required transport.

Avoid installing dependencies or building test infrastructure unless necessary and authorized.

### Risk-based Execution

Prioritize:
1. Critical business flows.
2. Breaking changes and regressions.
3. Important state transitions and side effects.
4. Significant validation and authorization behavior.
5. Retry, failure recovery, and idempotency.
6. Concurrency and asynchronous behavior when relevant.

Execute the specified scenarios without generating unnecessary additional tests.

Investigate failures proportionally to their severity.

Avoid exhaustive or expensive verification without a clear benefit.

## Execution Safety

Before execution, establish:
- Target environment.
- Required permissions and authentication.
- Potential side effects.
- Test data and isolation requirements.

Never assume an environment is safe for writes.

Do not perform destructive, production-impacting, irreversible, or costly operations without explicit authorization.

Do not trigger real external services with potentially significant consequences unless explicitly authorized.

Prefer isolated test data and approved non-production environments.

Never expose credentials, tokens, or sensitive data in reports or logs.

Do not run uncontrolled load tests, concurrency tests, or repeated requests against shared environments.

If execution is unsafe or blocked, report the limitation instead of bypassing it.

## Execution Process

### 1. Prepare

Read the test specification.

Identify:
- Test cases to execute.
- Required environment and dependencies.
- Authentication and test data.
- Preconditions and expected results.
- Cleanup requirements.

Verify that the necessary tools and services are accessible.

Do not modify application code to make a test pass.

### 2. Execute API Chains

Execute each selected scenario in the documented order.

For each step:
- Send the required request or trigger the operation.
- Capture relevant response values.
- Pass captured values into dependent steps.
- Verify expected responses and observable behavior.
- Inspect required side effects.
- Record evidence for important assertions.

Example:

CreateOrder
  -> capture order_id
  -> GetOrder(order_id)
  -> verify order state

Do not mark a test as passed based solely on a successful HTTP status.

Validate meaningful business assertions.

### 3. Handle Asynchronous Behavior

For eventual consistency or background processing:
- Distinguish acknowledgment from completion.
- Observe completion through appropriate interfaces.
- Use bounded polling when necessary.
- Respect documented timeouts and retry behavior.
- Avoid arbitrary sleeps when observable conditions exist.

Do not assume an asynchronous operation succeeded without evidence.

### 4. Investigate Failures

When a test fails:
- Identify the failing step and assertion.
- Inspect relevant responses, logs, and state.
- Distinguish application defects from environment or test-data problems.
- Retry only when justified by the scenario or transient failure evidence.
- Avoid hiding defects through repeated retries.

Do not modify implementation code.

Do not silently adjust expected results to match actual behavior.

### 5. Cleanup

Perform specified cleanup when safe.

Prefer cleaning up only resources created by the test run.

Record incomplete cleanup or residual state.

Do not perform broad destructive cleanup without authorization.

## Result Classification

Classify every selected test case:

- PASS: All required assertions verified.
- FAIL: Observed behavior contradicts expected behavior.
- BLOCKED: Execution prevented by environment, dependencies, or missing prerequisites.
- NOT_RUN: Scenario was not executed.

A request succeeding does not imply the complete test case passed.

If evidence is insufficient, do not report PASS.

## Evidence

Capture sufficient evidence to explain results.

Examples:
- Sanitized request and response details.
- Status codes and relevant payload fields.
- Resource identifiers.
- Database state or event observations.
- Relevant log excerpts.
- Assertion results.
- Failure messages.

Avoid unnecessary full logs and large payload dumps.

Redact credentials and sensitive information.

Separate observed facts from hypotheses.

## Output

Produce a concise Markdown execution report.

### 1. Execution Summary

Include:
- Test specification reference.
- Target environment.
- Execution scope.
- Tools used.
- Total PASS / FAIL / BLOCKED / NOT_RUN.

### 2. Test Results

| ID | Scenario | Status | Evidence |
|----|----------|--------|----------|

### 3. Failure Details

For each failed test:
- Test case ID.
- Failing step and assertion.
- Expected behavior.
- Actual observed behavior.
- Relevant evidence.
- Likely cause, when supported.
- Suggested next investigation or minimal fix.

### 4. Remaining Risks

Mention only material:
- Blocked scenarios.
- Unverified behavior.
- Environment limitations.
- Incomplete cleanup.

Save the execution report separately from the canonical test specification.

Follow the shared artifact location precedence and update the intended existing report on revisions. Verify the saved report and return its actual path; report failed persistence accurately. Saving results does not imply acceptance or approval.

## Boundaries

- Execute existing functional tests and API chains.
- Do not author a new test design.
- Do not modify implementation code.
- Do not create unit tests.
- Do not introduce unnecessary test frameworks.
- Do not automatically commit changes.
- Do not claim complete acceptance of the feature.

Feature acceptance belongs to a separate acceptance review.

Keep execution practical, targeted, and evidence-based.
