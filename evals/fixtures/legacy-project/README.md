# Legacy Project Fixture

This synthetic fixture represents an older project without the standardized fact layer. It is read-only input for Mode 2 tests.

## Purpose

The project contains one native source entry and a safe configuration template. A Mode 2 workflow should understand it without modifying it, then create an independent standardized copy elsewhere.

## Native layout

- `src/main.txt`: source behavior description.
- `.env.example`: non-working configuration placeholders only.

## Expected behavior

The source describes a local text transformation and has no external dependency. The real `.env` file is intentionally absent and must not be invented during copying.
