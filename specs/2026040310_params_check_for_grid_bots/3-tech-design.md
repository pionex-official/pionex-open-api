# Tech Design: Params Check API for Futures Grid & Spot Grid

## Overview

This is a spec-only change to `openapi_bot.yaml`. Two new POST endpoints are added, each with a dedicated request schema and a shared response schema.

## File to Modify

- `openapi_bot.yaml` — single file, all changes contained here

## New Endpoints

### 1. Futures Grid Check Params

**Placement:** Insert after `/api/v1/bot/orders/futuresGrid/create` (around line 239), before `/api/v1/bot/orders/futuresGrid/adjustParams`.

```yaml
  /api/v1/bot/orders/futuresGrid/check_params:
    post:
      tags: [Futures Grid]
      summary: Check futures grid parameters
      description: "Validate futures grid bot creation parameters without creating an order. Weight: 1."
      operationId: checkFuturesGridParams
      security:
        - apiKey: []
      parameters:
        - $ref: '#/components/parameters/Timestamp'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateFuturesGridRequest'
      responses:
        '200':
          description: Validation result
          content:
            application/json:
              schema:
                allOf:
                  - $ref: '#/components/schemas/BaseResponse'
                  - type: object
                    properties:
                      data:
                        $ref: '#/components/schemas/CheckParamsResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
```

### 2. Spot Grid Check Params

**Placement:** Insert after `/api/v1/bot/orders/spotGrid/create` (around line 436), before `/api/v1/bot/orders/spotGrid/adjustParams`.

```yaml
  /api/v1/bot/orders/spotGrid/check_params:
    post:
      tags: [Spot Grid]
      summary: Check spot grid parameters
      description: "Validate spot grid bot creation parameters without creating an order. Weight: 1."
      operationId: checkSpotGridParams
      security:
        - apiKey: []
      parameters:
        - $ref: '#/components/parameters/Timestamp'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateSpotGridRequest'
      responses:
        '200':
          description: Validation result
          content:
            application/json:
              schema:
                allOf:
                  - $ref: '#/components/schemas/BaseResponse'
                  - type: object
                    properties:
                      data:
                        $ref: '#/components/schemas/CheckParamsResponse'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
```

## New Schema: CheckParamsResponse

**Placement:** Add in `components/schemas` section (after existing schemas).

```yaml
    CheckParamsResponse:
      type: object
      description: Result of a parameter validation check
      properties:
        valid:
          type: boolean
          description: Whether all parameters pass validation
          example: false
        errors:
          type: array
          description: List of validation errors (empty when valid is true)
          items:
            type: object
            properties:
              field:
                type: string
                description: The parameter field that failed validation
                example: "buOrderData.top"
              message:
                type: string
                description: Human-readable error description
                example: "top price must be greater than bottom price"
        constraints:
          type: object
          description: Parameter constraints for the given symbol
          properties:
            minPrice:
              type: string
              description: Minimum allowed grid price
              example: "0.01"
            maxPrice:
              type: string
              description: Maximum allowed grid price
              example: "1000000"
            minRow:
              type: integer
              description: Minimum number of grid levels
              example: 2
            maxRow:
              type: integer
              description: Maximum number of grid levels
              example: 200
            minInvestment:
              type: string
              description: Minimum investment amount in quote currency
              example: "10"
            pricePrecision:
              type: integer
              description: Number of decimal places for price
              example: 2
            amountPrecision:
              type: integer
              description: Number of decimal places for investment amount
              example: 4
```

## API Key Permissions Update

In the `info.description` section (around line 70-84), add two new entries under the **Bot reading** permission list:

```
- `POST /api/v1/bot/orders/futuresGrid/check_params` — Check futures grid parameters
- `POST /api/v1/bot/orders/spotGrid/check_params` — Check spot grid parameters
```

> Rationale: check_params is a read-only validation — it does not create orders or execute trades. Bot reading permission is sufficient, consistent with the GET query endpoints.

## Design Decisions

| Decision | Choice | Reason |
|----------|--------|--------|
| Request schema | Reuse `CreateFuturesGridRequest` / `CreateSpotGridRequest` | Identical parameters; avoids duplication and drift |
| Response schema | Shared `CheckParamsResponse` | Both grid types return the same validation result structure |
| Permission | Bot reading (not Bot trading) | No trade execution; mirrors GET endpoint behavior |
| Constraints in response | Include symbol-level constraints | Allows clients to build dynamic validation without hardcoding limits |
