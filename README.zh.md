# XPay Labs Node.js SDK — 自托管加密货币支付网关 Node.js 开发包

[English](README.md) | 中文

**XPay Labs Node.js SDK** 是 [XPay Labs](https://www.xpaylabs.com) 自托管、非托管加密货币支付网关的官方 Node.js/TypeScript 客户端。支持在 TRON (TRC20)、20+ EVM 链（Ethereum、BNB Chain、Polygon、Arbitrum、Optimism、Base）和 SUI 上接收 USDT/USDC 支付，零网关手续费。

本 SDK 为 Node.js 开发者提供完整的 XPay Labs REST API 访问能力 — 创建收款/付款订单、验证 HMAC 签名 Webhook、查询支持的代币，私钥始终保留在你的基础设施上。

## 功能特性

- 创建加密货币收款订单（商户接收加密货币）
- 创建加密货币付款订单（商户发送加密货币）
- 实时订单状态查询
- HMAC-SHA256 Webhook 签名验证
- 完整的 TypeScript 类型定义
- 可配置超时的 Axios HTTP 客户端
- 全面的错误处理

## 安装

```bash
npm install @xpaylabs/node-sdk
# 或者
yarn add @xpaylabs/node-sdk
```

## 快速开始

```typescript
import { XPay } from '@xpaylabs/node-sdk';

const xpay = new XPay({
  apiKey: 'your-api-token',
  apiSecret: 'your-api-secret',
  baseUrl: 'https://api.xpaylabs.com',
});

async function createCollection() {
  const collection = await xpay.createCollection({
    amount: 50,
    symbol: 'USDT',
    chain: 'TRON',
    orderId: `order-${Date.now()}`,
    uid: 'user123',
  });
  console.log('Collection address:', collection.data?.address);
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

创建商户向用户发送加密货币的订单。

### 收款订单

创建商户从用户接收加密货币的订单。

### 订单状态

查询指定订单的当前状态。

### 支持的币种

获取平台支持的所有加密货币和链。

### Webhook

验证和解析 Webhook 通知，支持 HMAC-SHA256 签名验证。

## 相关资源

- [XPay Labs 官网](https://www.xpaylabs.com)
- [部署文档](https://www.xpaylabs.com/docs)
- [Java SDK](https://github.com/yan253319066/XPayLabs-java-sdk)
- [React 示例](https://github.com/yan253319066/XPayLabs-example-react)
- [Vue 3 示例](https://github.com/yan253319066/XPayLabs-example-vue)

## 许可证

MIT
