# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

XPayLabs crypto payment gateway Node.js SDK (npm package `@xpaylabs/node-sdk` v0.1.4). Supports USDT/USDC on TRON TRC20, EVM chains (Ethereum, BNB Chain, Polygon, Arbitrum, Optimism, Base), and SUI.

## Commands

```bash
npm run build    # tsc compile to dist/ (target ES2018, CommonJS)
npm test         # Jest tests with coverage reports
npm run lint     # ESLint (src --ext .ts)
npm run format   # Prettier format
```

## Architecture

### Exports
- `XPay` — core SDK class (`src/xpay.ts`)
- `ApiClient` — Axios-based HTTP client (`src/api-client.ts`)
- TypeScript types and enums (`src/types.ts`)

### XPay Core API

| Method | Endpoint | Description |
|------|------|------|
| `createPayout()` | POST `/v1/order/createPayout` | Create payout order |
| `createCollection()` | POST `/v1/order/createCollection` | Create collection order |
| `getOrderStatus()` | GET `/v1/order/status/{orderId}` | Query order status |
| `getSupportedSymbols()` | GET `/v1/symbol/supportSymbols` | Get supported tokens/chains |
| `verifyWebhook()` | Local verification | HMAC-SHA256 signature verification (30s window) |
| `parseWebhook()` | Local parsing | Verify and parse webhook payload |

### Authentication
- `X-API-TOKEN` request header (API Key)
- HMAC-SHA256 signature: sort by key → `key=value&key=value` → HMAC-SHA256 → hex digest
- Signed payload includes `sign`, `timestamp`, `nonce`, `data`

### Configuration
- `apiKey` / `apiSecret` (required)
- `baseUrl` (default `https://api.xpaylabs.com`)
- `timeout` (default 30s)

## Testing

```bash
npm test
npx jest --testPathPattern=xpay   # Run specific test file
```

Test file `src/xpay.test.ts` (280 lines), uses mocked `ApiClient`, covers success and error paths for all core methods.

## Notes

- `prepare` script auto-builds on `npm install`/`npm publish`
- `files` field only publishes `dist`, `README.md`, `LICENSE`
- Jest config generates both text and lcov coverage reports
