# Evidence-Based Audit Report Example

This is a synthetic example of a **read-only** Mode 3 report for `evals/fixtures/audit-project/`. It demonstrates evidence structure; it is not a review of production software.

## Review contract

- Goal: verify the percentage-change behavior described by the fixture.
- In scope: `README.md` and `src/calculator.txt` in the synthetic audit fixture.
- Out of scope: runtime integrations, performance, security controls, and files not present in the fixture.
- Modification authorization: read-only; no fixture files may be changed.
- Independent standard: the expected behavior stated in the fixture README and arithmetic division rules.
- Completion condition: trace all stated inputs through the pseudocode and report only reproducible findings.

## Coverage ledger

- Inventoried: 2 files.
- Checked: both files in full.
- Not checked: none within the declared fixture scope.
- Excluded: external runtime behavior because the fixture is descriptive pseudocode without a runtime.
- Environment limitation: no executable program exists; validation is static execution tracing.

## Confirmed finding

### A-EXAMPLE-001 | General | Unresolved

- Location: `evals/fixtures/audit-project/src/calculator.txt`, percentage-change return expression.
- Trigger condition: `start=0`, with any numeric `end`.
- Failure chain: `start=0, end=10` → `percentage_change` evaluates `(10 - 0) / 0 * 100` → division by zero occurs instead of a validation result → the caller cannot receive the required `invalid start value` outcome.
- Actual behavior: the pseudocode attempts division by zero.
- Expected behavior: reject zero as an invalid starting value before division.
- Evidence: the function contains only the return expression and no zero-start guard; the fixture README explicitly requires rejection.
- Impact and boundary: the zero-start case fails; nonzero cases are not shown to fail by this finding.
- Active disproof: checked for an upstream guard, alternate branch, stated caller constraint, or framework guarantee; none exists in the two-file fixture.
- Current treatment: reported only; no files modified due to the read-only contract.
- Validation: passed for static trace; no runtime test was available.

## Uncertain findings

None. Runtime-specific exception type and message are not classified because the fixture has no executable runtime.

## Recommendations

None beyond remediation of the confirmed defect. No architecture redesign is necessary for this fixture.

## Conclusion

One reproducible defect was confirmed within the declared scope. The review remained read-only. Remediation would require separate authorization, a DEVLOG work unit, a guard implementation, and targeted regression checks for zero and nonzero starting values.
