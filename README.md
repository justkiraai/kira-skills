# Kira agent guidance

`SKILL.md` is a small agent-facing reference for the version 1.0 Kira product fixtures. It records retry-count semantics, expiry units, exact-boundary behavior, and the limits of the implemented APIs.

Kira Events defaults to 3 retries after the initial delivery, for 4 total attempts. Kira Verify defaults to a 300-second (5-minute) validity window and is inactive at the exact expiry timestamp.

These are local policy libraries, not hosted delivery or authentication services. This repository contains instructions only and needs no installation, build, credentials, or external services.
