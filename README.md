# XPay Labs (xpay) Node.js SDK — Self-Hosted Crypto Payment Gateway for Node.js

English | [中文](README.zh.md)

**XPay Labs (xpay) Node.js SDK** is the official Node.js/TypeScript client for [XPay Labs (xpay)](https://www.xpaylabs.com) — the self-hosted, non-custodial crypto payment gateway. Accept USDT/USDC on TRON (TRC20), 20+ EVM chains (Ethereum, BNB Chain, Polygon, Arbitrum, Optimism, Base), and SUI with zero gateway fees.

This SDK gives Node.js developers full access to the XPay Labs REST API — create collection/payout orders, verify HMAC-signed webhooks, and query supported tokens — while your private keys stay on your own infrastructure.

| Feature | XPay Labs | BitPay | Coinbase Commerce |
|---------|-----------|--------|-------------------|
| Transaction Fees | **0%** (gas only) | 1% per tx | 0.8% + $25/mo |
| Custody Model | **Non-custodial** | Custodial | Custodial |
| Supported Chains | **TRON, EVM, SUI** (20+ chains) | BTC, ETH, LTC | ETH, Base |
| Deployment | **Self-hosted** (Docker) | Cloud (SaaS) | Cloud (SaaS) |
| SDK Support | **Node.js + Java** | PHP, Python | Node.js |
| Mempool Detection | **1–6 seconds** | N/A (block only) | N/A (block only) |

## Features

- Create cryptocurrency collection orders (merchant receives crypto)
- Create cryptocurrency payout orders (merchant sends crypto)
- Real-time order status queries
- HMAC-SHA256 webhook verification with timestamp validation
- Full TypeScript type definitions
- Axios-based HTTP client with configurable timeouts
- Comprehensive error handling

## Installation

```bash
npm install @xpaylabs/node-sdk
# or
yarn add @xpaylabs/node-sdk
```

## Quick Start

```typescript
import { XPay } from '@xpaylabs/node-sdk';

// Initialize the SDK with your API credentials
const xpay = new XPay({
  apiKey: 'your-api-token',
  apiSecret: 'your-api-secret',
  baseUrl: 'https://api.xpaylabs.com',
});

// Create a collection order (merchant receives crypto from user)
async function createCollection() {
  try {
    const collection = await xpay.createCollection({
      amount: 50,
      symbol: 'USDT',
      chain: 'TRON',
      orderId: `order-${Date.now()}`,
      uid: 'user123',
    });
    console.log('Collection created:', collection.data?.address);
  } catch (error) {
    console.error('Error:', error.message);
  }
}
```

## API Reference

### Configuration

```typescript
const xpay = new XPay({
  apiKey: 'your-api-token',
  apiSecret: 'your-api-secret',
  baseUrl: 'https://api.xpaylabs.com',
  timeout: 30000,
});
```

### Payout Orders

#### Create a payout order (merchant sends crypto to user)

```typescript
const payout = await xpay.createPayout({
  amount: 100,
  symbol: 'USDT',
  chain: 'TRON',
  orderId: 'order-123',
  uid: 'user123',
  receiveAddress: 'TXmVthgn6yT1kANGJHTHcbEGEKYDLLGJGp',
});
```

### Collection Orders

#### Create a collection order (merchant receives crypto from user)

```typescript
const collection = await xpay.createCollection({
  amount: 50,
  symbol: 'USDT',
  chain: 'TRON',
  orderId: 'order-123',
  uid: 'user123',
});
```

### Order Status

#### Get order status

```typescript
const orderDetails = await xpay.getOrderStatus('order-123');
```

### Supported Symbols

#### Get supported symbols

```typescript
const allSymbols = await xpay.getSupportedSymbols();
```

### Webhooks

#### Verify and parse webhook

```typescript
app.post('/webhook', express.json(), (req, res) => {
  const webhookData = req.body;
  const signature = webhookData.sign;
  const timestamp = webhookData.timestamp.toString();
  const body = JSON.stringify(webhookData);
  const event = xpay.parseWebhook(body, signature, timestamp);

  if (!event) {
    return res.status(400).send('Invalid webhook signature or timestamp expired');
  }

  switch (event.notifyType) {
    case 'ORDER_SUCCESS':
      console.log(`Order ${event.data.orderId} completed successfully!`);
      break;
    case 'COLLECT_SUCCESS':
      console.log(`Collection completed! Amount: ${event.data.collectAmount}`);
      break;
  }

  res.status(200).send('Webhook received');
});
```

## Error Handling

```typescript
try {
  const payout = await xpay.createPayout({ /* ... */ });
} catch (error) {
  console.error(`Error: ${error.message}`);
  console.error(`Status: ${error.status}`);
  console.error(`Code: ${error.code}`);
}
```

## TypeScript Support

This SDK includes comprehensive TypeScript definitions for all methods and response structures.

## Related Resources

- [XPay Labs Website](https://www.xpaylabs.com)
- [Deployment Guide](https://www.xpaylabs.com/docs)
- [Pricing — 0% Transaction Fees](https://www.xpaylabs.com/pricing)
- [Java SDK](https://github.com/yan253319066/XPayLabs-java-sdk)
- [React Example](https://github.com/yan253319066/XPayLabs-example-react)
- [Vue 3 Example](https://github.com/yan253319066/XPayLabs-example-vue)
- [BitPay Alternative](https://www.xpaylabs.com/alternatives/bitpay)
- [Coinbase Commerce Alternative](https://www.xpaylabs.com/alternatives/coinbase-commerce)

## License

MIT
