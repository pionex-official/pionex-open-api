# Task List: Params Check API for Futures Grid & Spot Grid

## Task 1 — Add CheckParamsResponse schema

**File:** `openapi_bot.yaml`  
**Action:** Append `CheckParamsResponse` schema to the `components/schemas` section.

**Schema to add:**
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
          description: Parameter constraints for the given trading pair
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

**Verification:** YAML is parseable; `CheckParamsResponse` appears in `components/schemas`.

---

## Task 2 — Add futures grid check_params endpoint

**File:** `openapi_bot.yaml`  
**Action:** Insert new path `/api/v1/bot/orders/futuresGrid/check_params` after the `/create` endpoint (after line ~238, before `/adjustParams`).

**YAML block to insert:**
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

**Verification:** Path `/api/v1/bot/orders/futuresGrid/check_params` appears in the YAML.

---

## Task 3 — Add spot grid check_params endpoint

**File:** `openapi_bot.yaml`  
**Action:** Insert new path `/api/v1/bot/orders/spotGrid/check_params` after the `/create` endpoint (after line ~436, before `/adjustParams`).

**YAML block to insert:**
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

**Verification:** Path `/api/v1/bot/orders/spotGrid/check_params` appears in the YAML.

---

## Task 4 — Update API key permissions in spec header

**File:** `openapi_bot.yaml`  
**Action:** In `info.description`, locate the **Bot reading** permission list (around line 70) and add two new entries:

```
- `POST /api/v1/bot/orders/futuresGrid/check_params` — Check futures grid parameters
- `POST /api/v1/bot/orders/spotGrid/check_params` — Check spot grid parameters
```

**Verification:** Both new endpoints appear under the Bot reading permission list in the description.

---

## Task 5 — Validate YAML syntax

**Action:** Run `python3 -c "import yaml; yaml.safe_load(open('openapi_bot.yaml'))"` or use any YAML linter to confirm no syntax errors were introduced.

**Verification:** Command exits with code 0 and no errors.
