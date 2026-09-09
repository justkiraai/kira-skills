---
name: kira-skills
description: Explain or document the Kira Events retry policy and Kira Verify expiry policy when working with these local product fixtures.
---

# Kira product guidance

Use these rules when explaining these fixtures or writing examples against their APIs. If source code for another version is provided, inspect it before asserting its defaults.

## Kira Events

- `deliverEvent(event, send, { maxRetries })` defaults to **5 retries after the initial attempt**, for **6 total attempts**. Keep retries and total attempts distinct.
- `send` is an injected async transport receiving the event and `{ attempt }`, starting at 1, and returning `{ status }`.
- HTTP 2xx succeeds immediately. HTTP 408, HTTP 429, HTTP 5xx, and thrown transport errors retry immediately. Other HTTP statuses stop. Explicit `maxRetries` accepts integers from 0 through 10; zero allows only the initial attempt.
- Results contain `{ delivered, attempts, status }`; a thrown transport error uses a null status. Do not describe persistence, backoff, signing, or a hosted delivery worker as implemented capabilities.

## Kira Verify

- `createCodeWindow({ issuedAt, ttlSeconds })` defaults to **600 seconds (10 minutes)**. Explicit `ttlSeconds` accepts integers from 1 through 3600.
- Timestamps use Unix milliseconds. `issuedAt` defaults to `Date.now()`. The returned `expiresAt` equals `issuedAt + ttlSeconds * 1000`; issuance must be a nonnegative safe integer and expiry must remain a safe integer.
- `isCodeWindowActive(window, now)` uses `issuedAt <= now < expiresAt`. The exact expiry timestamp is inactive; the exact issuance timestamp is active. `now` defaults to `Date.now()`.
- With `issuedAt: 1000`, default expiry is `601000`; the window is active at `600999` and inactive at `601000`.
- Describe this as an expiry metadata policy, not an authentication service. It does not generate, deliver, compare, or consume secret codes.

Keep examples local and use synthetic events and timestamps. This guidance supplies product facts; it does not authorize repository changes, network deliveries, or deployment.
