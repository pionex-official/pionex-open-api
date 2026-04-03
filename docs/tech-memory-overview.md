# Technical Memory & Key Decisions

## Iteration: 2026040310_params_check_for_grid_bots

**Date:** 2026-04-03  
**Feature:** Params Check API for Futures Grid and Spot Grid

### What Changed
- Added `POST /api/v1/bot/orders/futuresGrid/check_params` endpoint to `openapi_bot.yaml`
- Added `POST /api/v1/bot/orders/spotGrid/check_params` endpoint to `openapi_bot.yaml`
- Added `CheckParamsResponse` schema to `components/schemas`
- Updated API key permissions section to list both new endpoints under Bot reading

### Key Decisions

**Request schema reuse:** Both check_params endpoints reuse the existing `CreateFuturesGridRequest` / `CreateSpotGridRequest` schemas rather than defining new ones. Rationale: identical parameter sets; avoids schema duplication and drift between check and create endpoints.

**Shared response schema:** A single `CheckParamsResponse` schema is used for both futures and spot grid check endpoints. Rationale: identical validation result structure.

**Bot reading permission (not Bot trading):** check_params is read-only validation with no trade execution, so Bot reading permission is sufficient. Consistent with existing GET query endpoints.

**Constraints in response:** The response includes symbol-level parameter constraints (min/max price, row range, min investment, precision). This allows SDK clients to build dynamic UI validation without hardcoding exchange limits.
