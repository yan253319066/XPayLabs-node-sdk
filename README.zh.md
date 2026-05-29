# XPay Labs Node.js SDK

[English](README.md) | 中文

XPay Labs 加密货币支付网关的官方 Node.js SDK。

## 安装

```bash
npm install @xpaylabs/node-sdk
# 或者
yarn add @xpaylabs/node-sdk
```

## 功能特性

- 创建加密货币付款订单（商户向用户发送加密货币）
- 创建加密货币收款订单（商户从用户接收加密货币）
- 查询订单状态
- 获取支持的加密货币和链
- 验证和解析 Webhook 通知
- TypeScript 支持，包含完整的类型定义

## 快速开始

```typescript
import { XPay } from '@xpaylabs/node-sdk';

// 使用 API 凭证初始化 SDK
const xpay = new XPay({
  apiKey: 'your-api-token',
  apiSecret: 'your-api-secret',
  baseUrl: 'https://api.xpaylabs.com', // 可选，默认为生产环境 API
});

// 创建付款订单（商户向用户发送加密货币）
async function createPayout() {
  try {
    const payout = await xpay.createPayout({
      amount: 100,
      symbol: 'USDT',
      chain: 'TRON',
      orderId: `order-${Date.now()}`,
      uid: 'user123',
      receiveAddress: 'TXmVthgn6yT1kANGJHTHcbEGEKYDLLGJGp'
    });

    console.log('付款订单创建成功');
    return payout;
  } catch (error) {
    console.error('创建付款订单失败:', error);
  }
}
```

## API 参考

### 配置

```typescript
const xpay = new XPay({
  apiKey: 'your-api-token',
  apiSecret: 'your-api-secret',
  baseUrl: 'https://api.xpaylabs.com',
  timeout: 30000,
});
```

### 付款订单

#### 创建付款订单（商户向用户发送加密货币）

```typescript
const payout = await xpay.createPayout({
  amount: 100,
  symbol: 'USDT',
  chain: 'TRON',
  orderId: 'order-123',
  uid: 'user123',
  receiveAddress: 'TXmVthgn6yT1kANGJHTHcbEGEKYDLLGJGp'
});
```

### 收款订单

#### 创建收款订单（商户从用户接收加密货币）

```typescript
const collection = await xpay.createCollection({
  amount: 50,
  symbol: 'USDT',
  chain: 'TRON',
  orderId: 'order-123',
  uid: 'user123',
});
```

### 订单状态

#### 查询订单状态

```typescript
const orderDetails = await xpay.getOrderStatus('order-123');
```

### 支持的币种

#### 获取支持的币种

```typescript
const allSymbols = await xpay.getSupportedSymbols();
```

### Webhook

#### 验证和解析 Webhook

```typescript
app.post('/webhook', express.json(), (req, res) => {
  const event = xpay.parseWebhook(body, signature, timestamp);

  if (!event) {
    return res.status(400).send('无效的 webhook 签名或时间戳已过期');
  }

  switch (event.notifyType) {
    case 'ORDER_SUCCESS':
      console.log(`订单 ${event.data.orderId} 已完成！`);
      break;
    case 'COLLECT_SUCCESS':
      console.log(`收款已完成！金额: ${event.data.collectAmount}`);
      break;
  }

  res.status(200).send('Webhook received');
});
```

## 错误处理

```typescript
try {
  const payout = await xpay.createPayout({...});
} catch (error) {
  console.error(`错误: ${error.message}`);
}
```

## TypeScript 支持

本 SDK 包含所有方法和数据结构的完整 TypeScript 类型定义。

## 许可证

MIT
