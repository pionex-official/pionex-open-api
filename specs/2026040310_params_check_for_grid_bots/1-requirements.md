# Requirements: Params Check API for Futures Grid & Spot Grid

## Background

Users and SDK developers need to pre-validate bot creation parameters before submitting actual orders to avoid wasted API calls and improve feedback latency. Two GitHub issues request this feature:

- **Issue #17**: [Futures Grid Bot Params Check API](https://github.com/pionex-official/pionex-ai-kit/issues/17)
- **Issue #31**: [Spot Grid Bot Params Check API](https://github.com/pionex-official/pionex-ai-kit/issues/31)

## Problem Statement

Currently, the only way to validate bot parameters is to submit an actual create order request. This causes:
1. Wasted API calls and quota consumption on invalid parameters
2. Delayed error feedback (only after full order submission)
3. Difficulty building robust client-side validation in SDKs and UIs

## Requirements

### R1 — Futures Grid Params Check Endpoint

Add a new endpoint:
```
POST /api/v1/bot/orders/futuresGrid/check_params
```

**Behavior:**
- Accepts the same parameters as `POST /api/v1/bot/orders/futuresGrid/create`
- Performs parameter validation without creating any order or executing any trade
- Returns validation result and parameter constraint information
- Requires authentication (Bot reading permission is sufficient — no trading permission needed)

### R2 — Spot Grid Params Check Endpoint

Add a new endpoint:
```
POST /api/v1/bot/orders/spotGrid/check_params
```

**Behavior:**
- Accepts the same parameters as `POST /api/v1/bot/orders/spotGrid/create`
- Performs parameter validation without creating any order or executing any trade
- Returns validation result and parameter constraint information
- Requires authentication (Bot reading permission is sufficient)

### R3 — Response Format

Both endpoints return:
- `valid` (boolean): whether all parameters pass validation
- `errors` (array): list of validation errors, each with field name and error message
- `constraints` (object): parameter constraints for the symbol (e.g., min/max price, row range, min investment)

## Acceptance Criteria

- [ ] `POST /api/v1/bot/orders/futuresGrid/check_params` is documented in `openapi_bot.yaml`
- [ ] `POST /api/v1/bot/orders/spotGrid/check_params` is documented in `openapi_bot.yaml`
- [ ] Request schemas reuse or reference existing create request schemas
- [ ] Response schemas document `valid`, `errors`, and `constraints` fields
- [ ] Authentication requirements are documented (Bot reading permission)
- [ ] API key permissions section in the spec header is updated
- [ ] YAML is syntactically valid

## Out of Scope

- Server-side implementation (this repo is spec-only)
- Client SDK updates
- Changes to existing create/adjust/cancel endpoints
