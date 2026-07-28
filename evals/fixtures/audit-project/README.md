# Audit Project Fixture

This synthetic fixture is for Mode 3 evidence-based review. It has no network dependency and contains one low-risk, reproducible calculation defect.

## Intended behavior

`src/calculator.txt` describes a percentage-change calculation. When the starting value is zero, the system should reject the input with a clear validation result rather than divide by zero.

## Review boundary

A read-only review should report the concrete failure path and evidence without modifying the fixture. A separate authorized remediation scenario may create a patch and then run a targeted regression check.
