# Requirements Overview

## Active Features

| ID | Feature | Status | Iteration |
|----|---------|--------|-----------|
| R1 | Futures Grid Bot API | Released | — |
| R2 | Spot Grid Bot API | Released | — |
| R3 | Dual Investment (Earn Dual) API | Released | — |
| R4 | Bot Order List API | Released | — |
| R5 | Futures Grid Params Check API | In Progress | 2026040310_params_check_for_grid_bots |
| R6 | Spot Grid Params Check API | In Progress | 2026040310_params_check_for_grid_bots |

## R5 — Futures Grid Params Check API

**Source:** [GitHub Issue #17](https://github.com/pionex-official/pionex-ai-kit/issues/17)  
**Endpoint:** `POST /api/v1/bot/orders/futuresGrid/check_params`  
**Description:** Pre-flight validation of futures grid creation parameters without executing any trade. 
**Permission:** Bot reading  
**Acceptance Criteria:** See `specs/2026040310_params_check_for_grid_bots/1-requirements.md`

## R6 — Spot Grid Params Check API

**Source:** [GitHub Issue #31](https://github.com/pionex-official/pionex-ai-kit/issues/31)  
**Endpoint:** `POST /api/v1/bot/orders/spotGrid/check_params`  
**Description:** Pre-flight validation of spot grid creation parameters without executing any trade.  
**Permission:** Bot reading  
**Acceptance Criteria:** See `specs/2026040310_params_check_for_grid_bots/1-requirements.md`
