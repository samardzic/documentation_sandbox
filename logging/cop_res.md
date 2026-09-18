# Test Execution Logging: Analysis and Improvement Recommendations

## Overall Assessment

The proposed logging approach is a solid foundation for a hardware validation or post-silicon test framework. It already considers test metadata, DUT state, equipment interaction, log retention, parsing, team delivery, and automated reporting.

The main improvement is to make the logs **machine-consumable first and human-readable second**. Structured, traceable logs enable:

- Automated parsing and triage
- Reliable root-cause analysis
- Automatic report generation
- Failure trend analysis
- Cross-referencing between tests, DUTs, equipment, and artifacts
- Integration with dashboards or test-management systems

### Current strengths

- Test and run identification are considered
- DUT state is captured before and after interactions
- Equipment shutdown is included
- Log maintenance and alternative storage are recognized
- Exceptions, errors, connection events, and retries are identified for parsing
- Automatic test-report generation is planned

### Main gaps

- No formal log schema
- No explicit traceability between run, test, step, DUT, equipment, and artifacts
- No clearly defined first-failure data capture
- Runtime logs and test results are not separated
- Retry results could hide instability if only the final result is reported
- Retention is based only on execution count
- No integrity, redaction, concurrency, or interrupted-run strategy

---

## 1. Run and Test Identification

### Original idea

- Timestamp at test start and end
- Test name
- Test ID

### Recommended metadata

Each execution should include:

- Run ID
- Test ID
- Test name
- Test version
- Test-suite name and version
- Framework version
- Git commit ID
- Git branch or release tag
- Execution mode, such as manual, CI, regression, or debug
- Operator or automation identity
- Execution host
- Process ID
- Start timestamp
- End timestamp
- Duration
- Final result: `PASS`, `FAIL`, `SKIP`, `BLOCKED`, `ABORTED`, or `ERROR`
- Abort reason, if applicable

This information makes a run reproducible and allows a failure to be linked to the exact source-code and framework revision.

### Recommended run ID

Use a unique identifier independent of the folder name. For example:

```text
RUN-20260918-142500-000123
```

For distributed or parallel execution, a UUID can be safer:

```text
RUN-a8d71de6-7c5c-46d0-a0aa-201cf7ef16a1
```

---

## 2. Log and Run Folder Structure

### Original idea

- Subfolder named using timestamp and test run ID
- Store configuration-loading information

### Recommended structure

```text
logs/
└── 20260918_142500_RUN_000123/
    ├── execution.log
    ├── results.json
    ├── environment.json
    ├── configuration.json
    ├── equipment.json
    ├── steps.json
    ├── failures.json
    ├── dut_state_pre.json
    ├── dut_state_post.json
    ├── artifacts/
    │   ├── screenshots/
    │   ├── waveforms/
    │   ├── register_dumps/
    │   └── memory_dumps/
    └── reports/
        └── report.html
```

### Why separate files

- `execution.log` contains chronological human-readable events.
- `results.json` contains the machine-readable final result.
- `environment.json` captures the execution environment.
- `configuration.json` contains the resolved test configuration.
- `equipment.json` records equipment allocation and setup.
- `steps.json` contains structured step outcomes.
- `failures.json` contains normalized failure records.
- `artifacts/` stores binary or large diagnostic outputs.
- `reports/` stores generated human-readable reports.

Do not embed large waveforms, screenshots, or dumps directly in the main log. Store them as artifacts and place references to them in log events.

---

## 3. Configuration Loading

Do not log only that configuration loading occurred. Capture:

- Configuration source file
- Configuration version or checksum
- Load timestamp
- Overrides from environment variables or command-line arguments
- Resolved configuration after all overrides
- Validation result
- Missing, unknown, or deprecated settings
- Secret redaction status

Example:

```json
{
  "event": "configuration_loaded",
  "source": "configs/ethernet_validation.yaml",
  "sha256": "<checksum>",
  "validation": "PASS",
  "overrides": {
    "dut.ip": "192.0.2.10"
  }
}
```

Never write passwords, access tokens, private keys, or sensitive credentials to logs. Apply centralized redaction before data reaches any log handler.

---

## 4. Structured Logging

### Original idea

- Timestamp every log entry
- Define log levels

### Recommended event fields

Each machine-readable event should include, where applicable:

- Timestamp
- Monotonic elapsed time
- Log level
- Event type
- Component
- Action
- Result
- Duration
- Run ID
- Test ID
- Step ID
- Correlation ID
- DUT ID
- Equipment ID
- Attempt number
- Thread, task, or process identity
- Human-readable message
- Structured context

Example:

```json
{
  "timestamp": "2026-09-18T14:25:32.456+02:00",
  "elapsed_ms": 10543,
  "level": "INFO",
  "event": "equipment_command",
  "component": "Keithley2280",
  "action": "set_voltage",
  "parameters": {
    "channel": 1,
    "voltage_v": 1.8
  },
  "result": "SUCCESS",
  "duration_ms": 34,
  "run_id": "RUN-20260918-142500-000123",
  "test_id": "TEST-POWER-004",
  "step_id": "STEP-012",
  "correlation_id": "CORR-98f21"
}
```

### Recommended format

Use line-delimited JSON, commonly called JSON Lines or JSONL, for structured execution events:

```text
one JSON object per line
```

This format preserves chronological streaming, supports incremental parsing, and does not require loading the entire log into memory.

A readable text log can be emitted in parallel from the same structured events.

---

## 5. Timestamp Strategy

Use two time representations:

1. A timezone-aware wall-clock timestamp in ISO 8601 format
2. A monotonic elapsed-time value for accurate duration calculation

Example:

```text
2026-09-18T14:25:32.456+02:00
```

Also record:

- Timezone offset
- Clock source
- Time-synchronization status for distributed hosts

A wall clock can change during execution because of synchronization. A monotonic clock should therefore be used for durations and timeouts.

---

## 6. Log Levels

Use a clear and consistently enforced definition:

```text
TRACE     Very detailed protocol traffic, register access, or SCPI traffic
DEBUG     Internal state, decisions, variables, and diagnostic context
INFO      Normal test milestones and successful operations
WARNING   Unexpected but recoverable condition, fallback, or retry
ERROR     Operation or step failed, but cleanup or partial continuation is possible
CRITICAL  Unsafe or unrecoverable condition requiring immediate abort
```

### Additional recommendation

Log level alone should not encode the test result. Include a separate event type and result field.

For example, a failed assertion is not merely an `ERROR` message. It should be a structured assertion event:

```json
{
  "event": "assertion",
  "expected": 1,
  "observed": 0,
  "result": "FAIL"
}
```

Allow log-level configuration globally and per component so noisy protocol traces can be enabled selectively.

---

## 7. Step-Based Logging

Introduce a formal step model:

```text
STEP-001
STEP-002
STEP-003
```

Each step should record:

- Step ID
- Parent step ID, if steps can be nested
- Description
- Start time
- End time
- Duration
- Preconditions
- Action
- Expected result
- Actual result
- Verification method
- Result
- Retry count
- Failure code
- Related artifacts

Example:

```text
[STEP-012]
Action: Enable Ethernet MAC
Expected: Link state is UP
Actual: Link state is DOWN
Result: FAIL
```

Structured form:

```json
{
  "step_id": "STEP-012",
  "description": "Enable Ethernet MAC",
  "expected": {
    "link_state": "UP"
  },
  "observed": {
    "link_state": "DOWN"
  },
  "result": "FAIL"
}
```

A formal step model makes it possible to identify the exact stopping point and generate reports automatically.

---

## 8. DUT Identification and Configuration

### Original idea

- Record the device configuration used

### Recommended DUT data

Capture:

- DUT serial number
- Board serial number
- SoC part number
- SoC revision or stepping
- Board revision
- Firmware version
- Bootloader version
- Configuration profile
- Fuse or lifecycle state, where appropriate
- Interface information
- Power configuration
- Clock configuration
- Temperature or thermal condition

Avoid recording uniquely sensitive data unless it is required for validation and approved for storage.

Example:

```json
{
  "dut_id": "DUT-07",
  "soc_part": "<part-number>",
  "soc_revision": "B1",
  "board_revision": "EVB-3",
  "firmware_version": "1.8.4",
  "configuration_profile": "ethernet_nominal"
}
```

---

## 9. DUT State Tracking

### Original idea

- Device state before interaction
- Device state confirmation after every interaction

This is one of the strongest parts of the approach.

Explicitly separate:

- State before the step
- Expected state after the step
- Observed state after the step
- Verification method
- Verification result
- State differences

Example:

```text
STEP-042

State before:
PLL_LOCK=0

Expected state after:
PLL_LOCK=1

Observed state after:
PLL_LOCK=0

Verification result:
FAIL
```

For large state snapshots, store the complete snapshots as artifacts and log only:

- Artifact path
- Checksum
- Important differences
- Verification result

This prevents the execution log from becoming excessively large.

---

## 10. Equipment Identification and Configuration

Capture for each instrument:

- Logical equipment name
- Manufacturer
- Model
- Serial number
- Firmware version
- Driver version
- VISA resource, IP address, USB path, or COM port
- Communication backend
- Configuration before use
- Configuration after setup
- Self-test result
- Calibration status or due date, if available
- Reset status

Example:

```json
{
  "equipment_id": "PSU-01",
  "manufacturer": "Keithley",
  "model": "2280S",
  "connection": "USB0::<resource>",
  "role": "DUT_MAIN_POWER",
  "self_test": "PASS"
}
```

---

## 11. Resource Allocation

### Original idea

- Resource allocation

Expand this into a first-class subsystem.

Track:

- Resource name
- Equipment ID
- Connection address
- Reservation timestamp
- Lease expiration or heartbeat
- Release timestamp
- Run ID
- Owner
- Allocation result
- Contention or wait condition
- Forced release status

Example:

```text
Resource: PSU-01
Reserved by: RUN-20260918-142500-000123
Reserved at: 14:21:01
Released at: 14:44:55
Result: RELEASED
```

Use locking or a reservation service to prevent two tests from controlling the same equipment simultaneously.

Always release resources in a guaranteed cleanup block, even after an exception or user abort.

---

## 12. Equipment Command History

For hardware validation, record equipment and DUT communication at an appropriate diagnostic level.

### SCPI example

```text
SCPI TX: VOLT 1.8
SCPI RX: OK
```

### Register-access example

```text
MIPI WRITE
Address=0x1004
Data=0x81234567
```

Structured command records should include:

- Sequence number
- Request timestamp
- Response timestamp
- Direction
- Interface
- Command
- Sanitized parameters
- Response
- Duration
- Timeout
- Result
- Related step ID

Binary payloads should normally be stored as artifacts with checksums rather than printed in full.

---

## 13. Connection-State Logging

### Original idea

- Parse DUT connection events

Do not log only connected or disconnected. Record the connection state machine:

```text
DISCONNECTED
CONNECTING
CONNECTED
AUTHENTICATING
READY
DEGRADED
RECONNECTING
FAILED
CLOSED
```

For each transition, capture:

- Previous state
- New state
- Trigger
- Attempt number
- Duration
- Error code
- Interface details

This distinguishes a physical-link failure from protocol initialization, authentication, timeout, or DUT-readiness failures.

---

## 14. Retry Logging

### Original idea

- Parse retries

Never hide instability by reporting only the final successful attempt.

Example:

```text
Attempt 1 of 3: FAIL due to timeout
Attempt 2 of 3: FAIL due to timeout
Attempt 3 of 3: PASS
Final operation result: PASS_WITH_RETRIES
```

Structured example:

```json
{
  "event": "retry_attempt",
  "step_id": "STEP-005",
  "operation": "connect_dut",
  "attempt": 2,
  "max_attempts": 3,
  "result": "FAIL",
  "reason": "TIMEOUT",
  "backoff_ms": 1000
}
```

The final test report should expose:

- Total retries
- Steps containing retries
- Recovery success
- Pass-with-retry condition

A passing test that required retries may indicate a flaky DUT, instrument, driver, network, or framework component.

---

## 15. Exceptions and Error Model

### Original idea

- Parse exceptions and errors

Define a normalized failure record containing:

- Failure ID
- Failure category
- Failure code
- Message
- Exception type
- Stack trace
- Source component
- Run ID
- Test ID
- Step ID
- First occurrence time
- Last occurrence time
- Recoverable status
- Retry status
- Root exception and causal chain
- Related artifacts

Suggested categories:

```text
ASSERTION_FAILURE
DUT_CONNECTION_FAILURE
EQUIPMENT_CONNECTION_FAILURE
EQUIPMENT_COMMAND_FAILURE
TIMEOUT
CONFIGURATION_ERROR
RESOURCE_CONFLICT
FRAMEWORK_ERROR
SAFETY_ABORT
USER_ABORT
CLEANUP_FAILURE
```

Separate the primary failure from secondary cleanup failures. Otherwise, a shutdown exception can obscure the actual test failure.

---

## 16. First Failure Data Capture

Introduce **First Failure Data Capture**, abbreviated as FFDC.

At the first meaningful failure, automatically capture:

- Relevant register dump
- Recent execution-log tail
- Recent equipment-command history
- DUT state
- Equipment state
- Stack trace
- Active configuration
- Resource allocation
- Screenshots
- Oscilloscope waveforms
- Logic-analyzer captures
- Memory dump, when useful and safe

Example manifest:

```json
{
  "failure_id": "FAIL-0001",
  "step_id": "STEP-042",
  "timestamp": "2026-09-18T14:31:05.113+02:00",
  "artifacts": [
    "artifacts/register_dumps/fail_0001.txt",
    "artifacts/waveforms/fail_0001_ch1.csv",
    "artifacts/screenshots/fail_0001_scope.png"
  ]
}
```

FFDC should execute once for the first causal failure unless later failures require distinct diagnostic captures.

---

## 17. Exact Breaking-Point Logic

### Original idea

- Define logic for marking the exact breaking point

Define three concepts:

1. **First failed operation**: the earliest low-level operation that failed
2. **First failed verification**: the earliest expected-versus-observed mismatch
3. **Termination point**: the step at which execution stopped

These can be different.

Example:

```text
First failed operation: STEP-041, register write timed out
First failed verification: STEP-042, PLL_LOCK expected 1 but observed 0
Termination point: STEP-043, safety precondition not met
```

Recommended termination rules:

- Continue after explicitly recoverable errors
- Retry only errors declared retryable
- Stop a test after a failed mandatory assertion
- Skip dependent steps after prerequisite failure
- Run safe cleanup regardless of test result
- Mark independent unaffected tests according to suite policy

Use a stable failure code and causal links rather than relying only on free-text messages.

---

## 18. Equipment Ramp-Down and Cleanup

Correct the term **equipment rump down** to **equipment ramp-down**.

Treat cleanup as a formal execution phase:

```text
INITIALIZATION
SETUP
TEST_EXECUTION
FAILURE_CAPTURE
CLEANUP
REPORTING
COMPLETE
```

For each equipment resource, define:

- Safe target state
- Ordered shutdown sequence
- Maximum ramp rate
- Verification after shutdown
- Timeout
- Emergency fallback

Example:

```text
PSU output ramped from 1.8 V to 0 V
Output disabled
Measured output: 0.01 V
Safe-state verification: PASS
```

Cleanup should be idempotent, meaning it can be called more than once without causing unsafe behavior.

Also capture cleanup failures separately:

```text
Primary test result: FAIL
Cleanup result: FAIL
Final run result: FAIL_WITH_CLEANUP_ERROR
```

---

## 19. Artifact Management

Add a dedicated artifact section containing:

- Register dumps
- Memory dumps
- Scope screenshots
- Scope waveform files
- Logic-analyzer captures
- Protocol traces
- Equipment configuration
- DUT firmware image identification
- DUT serial number
- Console output
- Generated plots

Each artifact record should include:

- Artifact type
- File name
- Relative path
- Creation timestamp
- Producing component
- Run ID
- Test ID
- Step ID
- Failure ID, if applicable
- File size
- Checksum
- Retention class

Checksums help detect partial or corrupted captures.

---

## 20. Log Parsing and Event Taxonomy

### Original parsing targets

- Exceptions
- Errors
- DUT connection
- Retries
- Resource allocation

Convert these into explicit event types rather than extracting them only from text:

```text
run_started
run_finished
test_started
test_finished
step_started
step_finished
configuration_loaded
resource_reserved
resource_released
dut_state_captured
dut_connection_changed
equipment_command
assertion
retry_attempt
failure_detected
artifact_created
cleanup_started
cleanup_finished
report_created
```

A parser should consume stable event names and fields. Free-text parsing should remain a compatibility fallback, not the primary mechanism.

---

## 21. Log Delivery and Team Collaboration

### Original idea

- Summarize logs with the second part of the team

Define exactly what is delivered:

### Automatic summary

- Run ID
- Test or suite name
- DUT identity
- Start and end time
- Duration
- Overall result
- Passed, failed, skipped, blocked, and aborted tests
- First causal failure
- Exact failing step
- Retry summary
- Cleanup status
- Links or paths to logs, report, and artifacts

### Failure notification

A concise failure notification can contain:

```text
Run: RUN-20260918-142500-000123
Result: FAIL
Test: Ethernet Link Validation
Breaking point: STEP-042
Reason: PLL_LOCK expected 1, observed 0
Retries: 2 of 2 failed
Cleanup: PASS
Report: reports/report.html
Artifacts: artifacts/fail_0001/
```

Avoid sending unfiltered full logs as the default delivery. Send a structured summary and provide access to the detailed run package.

---

## 22. Automatic Test Reports

### Original idea

- Automatically create a test report

Generate two outputs.

### Human-readable report

Recommended sections:

- Executive summary
- Run metadata
- DUT metadata
- Equipment metadata
- Configuration summary
- Test result summary
- Step timeline
- Expected-versus-observed failures
- Retry summary
- First-failure analysis
- Cleanup status
- Artifact links
- Known limitations

### Machine-readable report

Example:

```json
{
  "schema_version": "1.0",
  "run_id": "RUN-20260918-142500-000123",
  "result": "FAIL",
  "failure_step": "STEP-042",
  "failure_code": "PLL_NOT_LOCKED",
  "duration_sec": 125.4,
  "cleanup_result": "PASS"
}
```

Machine-readable output can be consumed by dashboards, CI systems, trend-analysis tools, or test-management systems.

Where relevant, also consider standard test-result formats such as JUnit XML in addition to the framework's richer JSON report.

---

## 23. Correlation and Traceability

Introduce hierarchical identifiers:

```text
RUN-00123
└── TEST-ADC-004
    └── STEP-017
        └── ATTEMPT-002
```

Also use a correlation ID for a logical operation spanning several components.

Every related item should reference these identifiers:

- Execution-log entries
- Test results
- DUT transactions
- Equipment commands
- State snapshots
- Screenshots
- Waveforms
- Register dumps
- Exceptions
- Reports

This makes it possible to reconstruct a complete interaction across the Python framework, instrument driver, DUT interface, and test report.

---

## 24. Log Retention and Maintenance

### Original idea

- Keep the last 10 executions
- Use alternate log storage

Keeping only the last 10 executions is simple but can delete valuable failures too quickly.

Recommended policy dimensions:

- Maximum age
- Maximum total size
- Maximum run count
- Final result
- Project or release milestone
- Manual pinning
- Compliance classification

Example policy:

```text
Successful runs:
- Keep for 30 days
- Delete oldest first when storage exceeds its limit

Failed or aborted runs:
- Keep for 180 days
- Never delete while pinned to an active investigation

Release qualification runs:
- Archive according to project policy
```

The exact numbers should be selected according to available storage and project requirements.

Use staged storage where useful:

```text
Local fast storage -> recent active runs
Central storage -> retained run packages
Archive storage -> release or investigation evidence
```

Before deleting a run, verify that any scheduled upload completed successfully.

---

## 25. Compression, Rotation, and Size Control

Add controls for long regression runs:

- Rotate logs by size or time
- Compress closed log segments
- Limit repeated identical messages
- Sample high-frequency telemetry where full capture is unnecessary
- Preserve all events surrounding a failure
- Store large binary data separately

If repeated-message suppression is used, record the number of suppressed messages:

```text
Repeated message suppressed 12,431 times between 14:20:01 and 14:25:00
```

Do not suppress safety, failure, state-transition, or assertion events.

---

## 26. Interrupted and Incomplete Runs

Design explicitly for:

- Process crash
- Host restart
- Network loss
- Power failure
- User cancellation
- CI cancellation
- DUT lockup

Write a run-state marker such as:

```text
RUNNING
COMPLETE
ABORTED
INCOMPLETE
```

On framework startup, detect stale `RUNNING` markers and classify those runs as `INCOMPLETE` rather than leaving them ambiguous.

Flush critical events immediately or at controlled checkpoints so the most important diagnostic data survives a crash.

---

## 27. Parallel Execution and Concurrency

If tests can run concurrently:

- Never write unrelated runs to the same file without synchronization
- Include process, thread, or asynchronous-task identity
- Use a sequence number per event stream
- Use a central resource lock
- Keep every artifact linked to its run and step
- Avoid folder-name collisions

Ensure log events remain reconstructable even when they arrive out of order from multiple processes or hosts.

---

## 28. Data Integrity and Schema Versioning

Add:

- Log-schema version
- Report-schema version
- Configuration checksum
- Artifact checksum
- Optional run-manifest checksum

Example:

```json
{
  "schema_version": "1.0",
  "framework_version": "2.4.1",
  "configuration_sha256": "<checksum>"
}
```

Schema versioning allows parsers and reports to evolve without silently misinterpreting older logs.

---

## 29. Security and Data Protection

Implement a centralized policy for:

- Secret redaction
- Personal-data minimization
- Access control
- Encryption in transit and at rest where required
- Safe device-address handling
- Retention and deletion
- Audit access where required

Potentially sensitive fields should be either removed, masked, or hashed consistently.

Example:

```text
Authorization: ***REDACTED***
Password: ***REDACTED***
Token: ***REDACTED***
```

Do not rely on individual test authors to remember redaction. Apply it in the logging infrastructure.

---

## 30. Recommended Result Model

Use more than a binary pass or fail:

```text
PASS
PASS_WITH_RETRIES
FAIL
ERROR
SKIP
BLOCKED
ABORTED
INCOMPLETE
```

Suggested meaning:

- `PASS`: All expected verifications passed without recovery.
- `PASS_WITH_RETRIES`: Final expectation passed, but one or more attempts failed.
- `FAIL`: DUT behavior did not meet an expected result.
- `ERROR`: The framework, environment, or equipment prevented valid execution.
- `SKIP`: The test was intentionally not executed.
- `BLOCKED`: A prerequisite was not satisfied.
- `ABORTED`: Execution was stopped intentionally or for safety.
- `INCOMPLETE`: The run ended without a reliable final state.

Keeping `FAIL` separate from `ERROR` is important. A DUT validation failure is different from an unavailable instrument or a framework defect.

---

## 31. Recommended Final Run Package

For every execution, the framework should produce:

```text
run_folder/
├── manifest.json
├── execution.log
├── execution.jsonl
├── results.json
├── environment.json
├── configuration.json
├── equipment.json
├── resources.json
├── steps.json
├── failures.json
├── dut_state_pre.json
├── dut_state_post.json
├── artifacts/
│   ├── screenshots/
│   ├── waveforms/
│   ├── register_dumps/
│   ├── memory_dumps/
│   └── protocol_traces/
└── reports/
    ├── report.html
    └── junit.xml
```

### Manifest purpose

`manifest.json` should act as the index of the complete run package:

```json
{
  "schema_version": "1.0",
  "run_id": "RUN-20260918-142500-000123",
  "state": "COMPLETE",
  "result": "FAIL",
  "files": [
    "execution.log",
    "execution.jsonl",
    "results.json",
    "reports/report.html"
  ]
}
```

---

## 32. Recommended Implementation Priority

### Priority 1: Minimum reliable logging

1. Unique run, test, and step IDs
2. Structured JSONL events
3. Explicit result model
4. DUT, firmware, framework, and Git revision metadata
5. Formal step-based expected-versus-observed logging
6. Guaranteed safe cleanup
7. Exception stack traces and primary-failure preservation

### Priority 2: Debug efficiency

1. First Failure Data Capture
2. Equipment command history
3. DUT state before and after important actions
4. Retry attempt visibility
5. Artifact manifests and checksums
6. Exact breaking-point logic

### Priority 3: Scale and collaboration

1. Automatic HTML and machine-readable reports
2. Central storage and retention policy
3. Resource reservation and locking
4. Parallel-execution support
5. Team notifications with concise summaries
6. Dashboard and trend-analysis integration

### Priority 4: Robustness and governance

1. Schema versioning
2. Interrupted-run recovery
3. Secret redaction
4. Access controls
5. Log rotation and compression
6. Data-integrity verification

---

## 33. Revised Original Proposal

### Test execution logging

#### Log identity and metadata

- Unique run ID
- Test ID and test name
- Test and framework versions
- Git commit ID
- DUT identity and firmware version
- Execution host and operator
- Start and end timestamps
- Duration
- Final result

#### Run folder

- Subfolder named using timestamp and run ID
- Separate human-readable log and structured JSONL log
- Resolved configuration snapshot
- Environment snapshot
- Equipment inventory and configuration
- DUT pre-state and post-state
- Structured step results
- Failure records
- Artifact folder
- Generated reports

#### Log content

- Timestamp and monotonic elapsed time on every entry
- Defined log level
- Event type and component
- Run ID, test ID, step ID, and correlation ID
- Device configuration used
- Expected and observed DUT state
- Equipment commands and responses
- Retry attempts
- Resource allocation and release
- Exceptions and causal stack traces
- Artifact references
- Equipment ramp-down and safe-state verification

#### Parsing and analytics

- Exceptions
- Errors by category and code
- Assertions and mismatches
- DUT connection-state transitions
- Equipment connection failures
- Retries and recovered operations
- Resource contention
- First causal failure
- Exact termination point
- Cleanup failures
- Test duration and step duration

#### Maintenance

- Retention based on age, size, count, and result
- Preserve failed and pinned runs longer than successful runs
- Rotate and compress long logs
- Upload completed run packages to central storage
- Verify uploads before local deletion
- Detect and classify incomplete runs

#### Delivery

- Automatically generate a concise team summary
- Identify the exact breaking point and causal failure
- Link logs, reports, and artifacts
- Generate a human-readable HTML report
- Generate JSON and optional JUnit XML results
- Support dashboards and trend analysis

---

## Conclusion

The original approach covers the correct high-level areas. The most important architectural improvement is to treat each log entry as a structured event connected through run, test, step, attempt, and correlation identifiers.

The highest-value additions are:

1. Structured JSONL logging
2. Formal step-based expected-versus-observed records
3. First Failure Data Capture
4. Complete DUT and equipment traceability
5. Visible retry history
6. Safe and verified equipment ramp-down
7. Separate human and machine reports
8. Retention based on result, age, and storage size
9. Interrupted-run handling
10. Centralized secret redaction

With these additions, the logging system can scale from local Python hardware tests to shared laboratory automation and larger post-silicon validation environments.
