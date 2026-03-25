# Pionex Open API Specification

OpenAPI Specification (OAS) files for the [Pionex](https://www.pionex.com) cryptocurrency exchange API.

## Overview

This repository contains machine-readable API definitions in [OpenAPI 3.0](https://spec.openapis.org/oas/v3.0.3) format for Pionex's public and private endpoints.

| File | Description |
| --- | --- |
| `openapi.yaml` | REST API — Market data, trading, and account endpoints |
| `openapi_bot.yaml` | Bot API — Futures Grid bot endpoints (Beta) |
| `websocketapi.yaml` | WebSocket API — Real-time market data and account updates |

## Quick Start

You can use these spec files with any OpenAPI-compatible tool:

- **Swagger UI / Swagger Editor** — Import a YAML file to browse and try out endpoints interactively.
- **Code Generation** — Use [OpenAPI Generator](https://openapi-generator.tech/) or similar tools to generate client SDKs in your preferred language.
- **Postman** — Import the YAML file directly into Postman to create a ready-to-use collection.

## API Endpoints

| API | Base URL / Endpoint |
| --- | --- |
| REST | `https://api.pionex.com` |
| WebSocket (Public) | `wss://ws.pionex.com/wsPub` |
| WebSocket (Private) | `wss://ws.pionex.com/ws` |

## Authentication

Private endpoints require an API Key and HMAC SHA256 signature. See the `info.description` section in each spec file for full authentication details, or refer to the [Pionex API documentation](https://pionex-doc.gitbook.io/apidocs).

## License

This project is licensed under the [MIT License](LICENSE).
