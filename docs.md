# OneKey 开发者文档结构

---

## MetaMask 文档分析

### 1. MetaMask 的核心分类依据

MetaMask 将 dApp 接入文档分为两大类：

| 分类 | Wallet API | SDK |
|------|------------|-----|
| 定位 | 直接与浏览器扩展交互 | 在 Wallet API 基础上封装 |
| 适用场景 | Web-only dApp | 跨平台 dApp |
| 平台支持 | 浏览器扩展 + 移动端内置浏览器 | 桌面 + 移动端 + 多浏览器 |
| 连接方式 | EIP-6963 / EIP-1193 注入 | 深链接 + QR 码 + Session |
| 依赖 | 无额外依赖 | 需要安装 @metamask/sdk |
| 复杂度 | 低 | 中 |

**分类的本质依据：平台覆盖范围 + 连接方式的封装程度**

### 2. MetaMask 各模块做的工作

#### Wallet API 做的事情
- 定义 Provider 接口规范 (EIP-1193)
- 钱包发现机制 (EIP-6963)
- JSON-RPC 方法文档
- 签名、交易、网络管理等操作指南
- 直接使用示例 (Vanilla TS / React TS)
- 第三方库集成指引 (Wagmi, RainbowKit 等)

#### SDK 做的事情
- 跨平台连接封装 (桌面 + 移动端)
- 深链接和 QR 码连接
- Session 管理
- 多种框架的 SDK (JS, React, Vue, Node, Unity, Android, iOS, React Native, Flutter)
- 高级功能 (批量 RPC, connectAndSign 等)

### 3. SDK 深链接 + QR码 + Session 机制详解

#### 3.1 为什么需要这些机制？

**核心问题：移动端浏览器无法直接访问钱包 App**

```
传统方式的痛点:

桌面端: 浏览器扩展注入 window.ethereum ✓ 没问题

移动端:
  Safari/Chrome ──✗──> MetaMask/OneKey App

  用户被迫使用 "在钱包内置浏览器中打开"
  体验极差，用户流失严重
```

#### 3.2 SDK 连接架构

```
┌─────────────────────────────────────────────────────────────────┐
│                    SDK 连接架构                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  桌面浏览器 dApp                    移动浏览器 dApp              │
│       │                                  │                      │
│       ▼                                  ▼                      │
│  检测到扩展？─────Yes───> 直接连接      生成 Deeplink            │
│       │                                  │                      │
│       No                                 ▼                      │
│       │                            跳转到钱包 App               │
│       ▼                                  │                      │
│  显示 QR 码 <─────────────────────────────┘                     │
│       │                                                         │
│       ▼                                                         │
│  用户扫码 ──> 钱包 Mobile 连接                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 3.3 连接建立的完整流程

```
1. dApp 生成密钥对 (ECIES 椭圆曲线加密)
   │
   ▼
2. 生成 Deeplink/QR码，包含:
   - dApp 的 ECIES 公钥
   - Socket.io 房间 ID
   - dApp 元数据 (名称、图标、URL)
   │
   ▼
3. 用户扫码/点击深链接
   │
   ▼
4. 钱包 Mobile 打开，连接到 Socket.io 服务器
   │
   ▼
5. 钱包生成自己的 ECIES 密钥对
   │
   ▼
6. 双方通过 Socket.io 交换公钥，生成共享密钥
   │
   ▼
7. 建立端到端加密通道，发送 JSON-RPC 请求
```

#### 3.4 Session 管理

| 状态 | 说明 |
|------|------|
| 活跃 | 正常通信 |
| 暂停 | 钱包 App 在后台超过 20 秒 |
| 恢复 | 用户重新打开钱包 App，自动恢复 |
| 清除 | 用户刷新/关闭 dApp 页面 |

#### 3.5 各平台行为差异

| 平台 | 连接方式 | 通信方式 |
|------|----------|----------|
| 桌面 Web (有扩展) | 直接注入 | window.ethereum |
| 桌面 Web (无扩展) | QR 码 | Socket.io 加密通道 |
| 移动 Web | Deeplink | Socket.io 加密通道 |
| Android | Deeplink | 本地直接通信 |
| iOS | Deeplink | Socket.io 加密通道 |
| Node.js | 控制台 QR 码 | Socket.io 加密通道 |

### 4. SDK vs 第三方库 (RainbowKit 等) 对比

#### 4.1 定位差异

| 维度 | @metamask/sdk (钱包官方 SDK) | RainbowKit / Wagmi (第三方库) |
|------|------------------------------|------------------------------|
| **定位** | 单一钱包的跨平台连接 | 多钱包的统一抽象层 |
| **钱包支持** | 仅该钱包 | 多钱包 (MetaMask, Coinbase, Rainbow...) |
| **移动连接** | 自建深链接 + Socket.io | 依赖 WalletConnect 协议 |
| **桌面扩展** | 直接检测连接 | EIP-6963 发现多个钱包 |
| **UI 组件** | 无 (Headless) | 有现成 UI |
| **依赖关系** | 独立 | 基于 Wagmi/viem |

#### 4.2 架构关系

```
┌─────────────────────────────────────────────────────────────────┐
│                        开发者选择                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  方式 1: 直接使用 Wallet API                                     │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  window.ethereum / window.$onekey                        │   │
│  │  EIP-1193 / EIP-6963                                     │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  方式 2: 使用钱包官方 SDK                                        │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  @metamask/sdk / @onekeyhq/sdk                           │   │
│  │  封装了: 深链接 + QR码 + Session + 跨平台                  │   │
│  │  底层仍然是 Wallet API                                    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  方式 3: 使用第三方库                                            │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  RainbowKit / Web3Modal / ConnectKit                     │   │
│  │     │                                                    │   │
│  │     ▼                                                    │   │
│  │  Wagmi (React Hooks 层)                                  │   │
│  │     │                                                    │   │
│  │     ▼                                                    │   │
│  │  Connectors:                                             │   │
│  │  - injected() ──> EIP-6963 发现所有钱包                   │   │
│  │  - metaMask() ──> 内部使用 @metamask/sdk                  │   │
│  │  - walletConnect() ──> WalletConnect 协议                 │   │
│  │     │                                                    │   │
│  │     ▼                                                    │   │
│  │  viem (底层 RPC 调用)                                     │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 4.3 如何选择？

| 场景 | 推荐方案 |
|------|----------|
| Web-only，只支持 OneKey | Wallet API (EIP-6963) |
| Web-only，支持多钱包 | RainbowKit / Wagmi |
| 需要移动端原生体验 | SDK |
| 需要 UI 组件 | RainbowKit / Web3Modal |
| 需要完全控制 | Wallet API |

### 5. MetaMask Quick Start 结构

```
Quick Start
├── Connect to MetaMask
│   ├── MetaMask SDK (跨平台连接)
│   └── Wallet API (直接调用扩展)
│
├── Create embedded wallets (内嵌钱包)
│   └── 各平台 SDK (React, Vue, JS, Node, Unity, Android, iOS...)
│
├── Create smart accounts (智能账户)
│
└── Extend and scale (扩展服务)
    ├── Services (Infura API)
    └── Snaps (扩展功能)
```

### 6. EIP-6963 在 MetaMask 文档中的位置

- 作为 Wallet API 的**首选推荐**连接方式
- 提供完整的 TypeScript 接口定义
- 提供 Vanilla TS 和 React TS 两个完整示例
- 同时推荐第三方库 (Wagmi, RainbowKit 等) 作为替代方案

---

## 软件层 (dApp 接入)

### 快速入门 (Quick Start)

#### 选择接入方式

| 接入方式 | 说明 | 适用场景 |
|----------|------|----------|
| **Wallet API** | 直接与 OneKey 扩展交互，无依赖 | Web dApp，简单场景 |
| **SDK** | 跨平台 SDK 封装 | 跨平台 dApp，移动端连接 |
| **第三方库** | Wagmi / RainbowKit / Web3Modal | 需要 UI 组件，多钱包支持 |

#### 5 分钟快速接入

##### 方式 1: Wallet API (最简单)

```html
<!-- index.html -->
<button id="connect">Connect OneKey</button>
<script>
document.getElementById('connect').onclick = async () => {
  // EIP-6963 方式发现钱包
  const providers = []
  window.addEventListener('eip6963:announceProvider', (e) => {
    providers.push(e.detail)
  })
  window.dispatchEvent(new Event('eip6963:requestProvider'))

  // 等待钱包广播
  await new Promise(r => setTimeout(r, 100))

  // 找到 OneKey
  const onekey = providers.find(p => p.info.rdns === 'so.onekey.app.wallet')
  if (!onekey) {
    alert('Please install OneKey')
    return
  }

  // 连接
  const accounts = await onekey.provider.request({
    method: 'eth_requestAccounts'
  })
  console.log('Connected:', accounts[0])
}
</script>
```

##### 方式 2: 使用 Wagmi (推荐)

```tsx
// App.tsx
import { WagmiProvider, createConfig, http, useAccount, useConnect } from 'wagmi'
import { mainnet } from 'wagmi/chains'
import { injected } from 'wagmi/connectors'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const config = createConfig({
  chains: [mainnet],
  connectors: [injected()],
  transports: { [mainnet.id]: http() }
})

function ConnectButton() {
  const { address, isConnected } = useAccount()
  const { connect, connectors } = useConnect()

  if (isConnected) return <p>Connected: {address}</p>

  return (
    <button onClick={() => connect({ connector: connectors[0] })}>
      Connect Wallet
    </button>
  )
}

export default function App() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={new QueryClient()}>
        <ConnectButton />
      </QueryClientProvider>
    </WagmiProvider>
  )
}
```

---

## 一、Wallet API

> 直接与 OneKey 浏览器扩展交互，无需额外依赖。
> 适合 Web-only 的 dApp 或需要精细控制的场景。

### 1. 连接钱包

#### 1.1 EIP-6963 (推荐)

> **为什么推荐 EIP-6963？**
>
> 传统 `window.ethereum` 注入方式的问题：
> - 多钱包互相覆盖，行为不可预测
> - 用户无法选择使用哪个钱包
>
> EIP-6963 的解决方案：
> - 钱包主动广播自己的存在
> - dApp 可发现所有已安装的钱包
> - 用户主动选择使用哪个钱包

##### 核心接口

```typescript
// 钱包信息
interface EIP6963ProviderInfo {
  uuid: string      // 唯一标识
  name: string      // 钱包名称
  icon: string      // 钱包图标 (data URI)
  rdns: string      // 反向域名标识
}

// 钱包详情 (包含 Provider)
interface EIP6963ProviderDetail {
  info: EIP6963ProviderInfo
  provider: EIP1193Provider
}

// 钱包广播事件
type EIP6963AnnounceProviderEvent = CustomEvent<EIP6963ProviderDetail>

// EIP-1193 Provider 接口
interface EIP1193Provider {
  request(args: { method: string; params?: unknown[] }): Promise<unknown>
  on(event: string, handler: (...args: any[]) => void): void
  removeListener(event: string, handler: (...args: any[]) => void): void
}
```

##### 完整实现示例 (Vanilla TypeScript)

```typescript
// types.ts
declare global {
  interface WindowEventMap {
    'eip6963:announceProvider': EIP6963AnnounceProviderEvent
  }
}

// wallet-discovery.ts
class WalletDiscovery {
  private providers: EIP6963ProviderDetail[] = []
  private listeners: ((providers: EIP6963ProviderDetail[]) => void)[] = []

  constructor() {
    window.addEventListener('eip6963:announceProvider', (event) => {
      // 避免重复添加
      if (this.providers.some(p => p.info.uuid === event.detail.info.uuid)) {
        return
      }
      this.providers.push(event.detail)
      this.notifyListeners()
    })
  }

  // 请求钱包广播
  discover() {
    window.dispatchEvent(new Event('eip6963:requestProvider'))
  }

  // 获取所有钱包
  getProviders() {
    return this.providers
  }

  // 获取 OneKey
  getOneKey() {
    return this.providers.find(p => p.info.rdns === 'so.onekey.app.wallet')
  }

  // 监听钱包变化
  subscribe(listener: (providers: EIP6963ProviderDetail[]) => void) {
    this.listeners.push(listener)
    return () => {
      this.listeners = this.listeners.filter(l => l !== listener)
    }
  }

  private notifyListeners() {
    this.listeners.forEach(l => l(this.providers))
  }
}

// 使用
const discovery = new WalletDiscovery()
discovery.discover()

// 连接 OneKey
async function connectOneKey() {
  const onekey = discovery.getOneKey()
  if (!onekey) {
    throw new Error('OneKey not installed')
  }

  const accounts = await onekey.provider.request({
    method: 'eth_requestAccounts'
  }) as string[]

  return accounts[0]
}
```

##### 完整实现示例 (React TypeScript)

```tsx
// hooks/useWalletProviders.ts
import { useSyncExternalStore, useCallback } from 'react'

interface ProviderStore {
  providers: EIP6963ProviderDetail[]
  subscribe: (callback: () => void) => () => void
  getSnapshot: () => EIP6963ProviderDetail[]
}

const createStore = (): ProviderStore => {
  let providers: EIP6963ProviderDetail[] = []
  let listeners: (() => void)[] = []

  window.addEventListener('eip6963:announceProvider', (event) => {
    if (providers.some(p => p.info.uuid === event.detail.info.uuid)) return
    providers = [...providers, event.detail]
    listeners.forEach(l => l())
  })

  window.dispatchEvent(new Event('eip6963:requestProvider'))

  return {
    providers,
    subscribe: (callback) => {
      listeners.push(callback)
      return () => {
        listeners = listeners.filter(l => l !== callback)
      }
    },
    getSnapshot: () => providers
  }
}

const store = createStore()

export function useWalletProviders() {
  return useSyncExternalStore(store.subscribe, store.getSnapshot)
}

// components/WalletList.tsx
export function WalletList() {
  const providers = useWalletProviders()
  const [account, setAccount] = useState<string>()
  const [selectedWallet, setSelectedWallet] = useState<EIP6963ProviderDetail>()

  const connect = useCallback(async (provider: EIP6963ProviderDetail) => {
    try {
      const accounts = await provider.provider.request({
        method: 'eth_requestAccounts'
      }) as string[]

      setAccount(accounts[0])
      setSelectedWallet(provider)
    } catch (error) {
      console.error('Connection failed:', error)
    }
  }, [])

  if (account && selectedWallet) {
    return (
      <div>
        <img src={selectedWallet.info.icon} alt={selectedWallet.info.name} />
        <p>{selectedWallet.info.name}</p>
        <p>{account.slice(0, 6)}...{account.slice(-4)}</p>
      </div>
    )
  }

  return (
    <div>
      <h2>Select Wallet</h2>
      {providers.length === 0 ? (
        <p>No wallets detected</p>
      ) : (
        providers.map(provider => (
          <button
            key={provider.info.uuid}
            onClick={() => connect(provider)}
          >
            <img src={provider.info.icon} alt={provider.info.name} />
            {provider.info.name}
          </button>
        ))
      )}
    </div>
  )
}
```

##### OneKey 的标识

```typescript
// 识别 OneKey 钱包
const isOneKey = (info: EIP6963ProviderInfo) => {
  return info.rdns === 'so.onekey.app.wallet'
}

// 优先连接 OneKey
const connectPreferOneKey = async (providers: EIP6963ProviderDetail[]) => {
  const onekey = providers.find(p => isOneKey(p.info))
  const provider = onekey || providers[0]

  if (!provider) throw new Error('No wallet found')

  return provider.provider.request({ method: 'eth_requestAccounts' })
}
```

#### 1.2 EIP-1193 (传统方式)

> 传统的 Provider 注入方式，仍被广泛使用，但存在多钱包冲突问题。

##### Provider 对象

```typescript
// 通用 Provider (可能被其他钱包覆盖)
window.ethereum

// OneKey 命名空间 (推荐，避免冲突)
window.$onekey.ethereum
```

##### 检测与连接

```typescript
// 检测 OneKey 是否安装
const isOneKeyInstalled = () => {
  return typeof window.$onekey !== 'undefined'
}

// 连接钱包
async function connect() {
  if (!isOneKeyInstalled()) {
    window.open('https://onekey.so/download', '_blank')
    return
  }

  const accounts = await window.$onekey.ethereum.request({
    method: 'eth_requestAccounts'
  })
  return accounts[0]
}
```

### 2. Provider API

#### 2.1 核心方法

```typescript
interface EIP1193Provider {
  // 发起 RPC 请求
  request(args: { method: string; params?: unknown[] }): Promise<unknown>

  // 事件监听
  on(event: string, handler: (...args: any[]) => void): void
  removeListener(event: string, handler: (...args: any[]) => void): void
}
```

#### 2.2 常用 RPC 方法

| 方法 | 说明 | 参数 |
|------|------|------|
| `eth_requestAccounts` | 请求连接 | 无 |
| `eth_accounts` | 获取已连接账户 | 无 |
| `eth_chainId` | 获取当前链 ID | 无 |
| `wallet_switchEthereumChain` | 切换网络 | `[{ chainId }]` |
| `wallet_addEthereumChain` | 添加网络 | `[{ chainId, chainName, ... }]` |
| `eth_sendTransaction` | 发送交易 | `[{ to, value, data, ... }]` |
| `eth_getBalance` | 查询余额 | `[address, blockTag]` |
| `personal_sign` | 个人签名 | `[message, address]` |
| `eth_signTypedData_v4` | EIP-712 签名 | `[address, typedData]` |

#### 2.3 事件监听

```typescript
const provider = window.$onekey.ethereum

// 账户切换
provider.on('accountsChanged', (accounts: string[]) => {
  if (accounts.length === 0) {
    // 用户断开连接
    console.log('Disconnected')
  } else {
    console.log('Account changed:', accounts[0])
  }
})

// 网络切换
provider.on('chainChanged', (chainId: string) => {
  // 建议刷新页面以避免状态不一致
  console.log('Chain changed:', parseInt(chainId, 16))
  window.location.reload()
})

// 连接状态
provider.on('connect', (info: { chainId: string }) => {
  console.log('Connected to chain:', info.chainId)
})

provider.on('disconnect', (error: { code: number; message: string }) => {
  console.log('Disconnected:', error.message)
})
```

### 3. 签名

#### 3.1 EIP-191: personal_sign

```typescript
const message = 'Hello, OneKey!'
const address = '0x...'

// 注意: personal_sign 的参数顺序是 [message, address]
const signature = await provider.request({
  method: 'personal_sign',
  params: [message, address]
})

// 验证签名 (使用 viem)
import { verifyMessage } from 'viem'

const isValid = await verifyMessage({
  address,
  message,
  signature
})
```

#### 3.2 EIP-712: signTypedData_v4

```typescript
const typedData = {
  types: {
    EIP712Domain: [
      { name: 'name', type: 'string' },
      { name: 'version', type: 'string' },
      { name: 'chainId', type: 'uint256' },
      { name: 'verifyingContract', type: 'address' }
    ],
    Mail: [
      { name: 'from', type: 'address' },
      { name: 'to', type: 'address' },
      { name: 'contents', type: 'string' }
    ]
  },
  primaryType: 'Mail',
  domain: {
    name: 'My dApp',
    version: '1',
    chainId: 1,
    verifyingContract: '0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC'
  },
  message: {
    from: '0xAlice...',
    to: '0xBob...',
    contents: 'Hello!'
  }
}

const signature = await provider.request({
  method: 'eth_signTypedData_v4',
  params: [address, JSON.stringify(typedData)]
})
```

#### 3.3 EIP-4361: Sign-In with Ethereum (SIWE)

```typescript
import { SiweMessage } from 'siwe'

// 1. 创建 SIWE 消息
const message = new SiweMessage({
  domain: window.location.host,
  address: userAddress,
  statement: 'Sign in to My dApp',
  uri: window.location.origin,
  version: '1',
  chainId: 1,
  nonce: generateNonce() // 从后端获取
})

// 2. 签名
const signature = await provider.request({
  method: 'personal_sign',
  params: [message.prepareMessage(), userAddress]
})

// 3. 发送到后端验证
const response = await fetch('/api/login', {
  method: 'POST',
  body: JSON.stringify({ message: message.prepareMessage(), signature })
})
```

### 4. 网络管理

#### 4.1 获取当前网络

```typescript
const chainId = await provider.request({ method: 'eth_chainId' })
console.log('Current chain:', parseInt(chainId, 16))
```

#### 4.2 切换网络

```typescript
try {
  await provider.request({
    method: 'wallet_switchEthereumChain',
    params: [{ chainId: '0x89' }] // Polygon
  })
} catch (error) {
  // 4902: 网络未添加
  if (error.code === 4902) {
    // 尝试添加网络
    await addNetwork()
  }
}
```

#### 4.3 添加自定义网络

```typescript
await provider.request({
  method: 'wallet_addEthereumChain',
  params: [{
    chainId: '0x89',
    chainName: 'Polygon Mainnet',
    nativeCurrency: {
      name: 'MATIC',
      symbol: 'MATIC',
      decimals: 18
    },
    rpcUrls: ['https://polygon-rpc.com'],
    blockExplorerUrls: ['https://polygonscan.com']
  }]
})
```

#### 4.4 常用链 ID

| 链 | Chain ID (Hex) | Chain ID (Dec) |
|----|----------------|----------------|
| Ethereum | 0x1 | 1 |
| Polygon | 0x89 | 137 |
| BSC | 0x38 | 56 |
| Arbitrum | 0xa4b1 | 42161 |
| Optimism | 0xa | 10 |
| Avalanche | 0xa86a | 43114 |
| Base | 0x2105 | 8453 |

### 5. 多链支持 (Wallet API)

#### 5.1 EVM 兼容链

所有 EVM 链共用同一 Provider 接口，通过 chainId 区分。

#### 5.2 Bitcoin

```typescript
const btcProvider = window.$onekey.btc

// 连接
const accounts = await btcProvider.requestAccounts()

// 获取公钥
const publicKey = await btcProvider.getPublicKey()

// 签名消息
const signature = await btcProvider.signMessage(message)

// 签名 PSBT
const signedPsbt = await btcProvider.signPsbt(psbtHex)
```

#### 5.3 Solana

```typescript
const solProvider = window.$onekey.solana

// 连接
const { publicKey } = await solProvider.connect()

// 签名消息
const signature = await solProvider.signMessage(
  new TextEncoder().encode(message)
)

// 签名交易
const signedTx = await solProvider.signTransaction(transaction)

// 签名并发送交易
const txId = await solProvider.signAndSendTransaction(transaction)
```

#### 5.4 其他链

| 链 | Provider 入口 | 主要方法 |
|----|--------------|----------|
| Cosmos | `window.$onekey.cosmos` | `getKey()`, `signAmino()`, `signDirect()` |
| Aptos | `window.$onekey.aptos` | `connect()`, `signTransaction()`, `signMessage()` |
| Sui | `window.$onekey.sui` | `connect()`, `signTransactionBlock()` |
| Tron | `window.$onekey.tron` | `request()` (类似 EIP-1193) |

---

## 二、SDK

> 跨平台 SDK，支持桌面和移动端连接。
> 适合需要移动端深链接、QR 码连接的 dApp。

### 1. 概述

#### 为什么使用 SDK？

| 场景 | Wallet API | SDK |
|------|------------|-----|
| 桌面浏览器有扩展 | ✓ 直接连接 | ✓ 直接连接 |
| 桌面浏览器无扩展 | ✗ 无法连接 | ✓ QR 码连接 |
| 移动浏览器 | ✗ 需要内置浏览器 | ✓ 深链接跳转 |
| React Native | ✗ 不支持 | ✓ 原生集成 |

#### SDK 的核心能力

- **跨平台支持** - 一次集成，覆盖桌面和移动端
- **移动端连接** - 深链接 + QR 码，无需内置浏览器
- **Session 管理** - 自动处理连接状态、暂停、恢复
- **加密通信** - ECIES 端到端加密

### 2. JavaScript SDK

#### 2.1 安装

```bash
npm install @onekeyhq/sdk
```

#### 2.2 初始化

```typescript
import { OneKeySDK } from '@onekeyhq/sdk'

const sdk = new OneKeySDK({
  // dApp 元数据，用于连接时展示
  dappMetadata: {
    name: 'My dApp',
    url: window.location.href,
    iconUrl: 'https://mydapp.com/icon.png'
  },
  // 可选: 检测到扩展时立即连接
  checkInstallationImmediately: false,
  // 可选: 只使用扩展，不显示 QR 码
  extensionOnly: false
})
```

#### 2.3 连接

```typescript
// 简单连接
const accounts = await sdk.connect()
console.log('Connected:', accounts[0])

// 连接并签名 (一步完成)
const result = await sdk.connectAndSign({
  msg: 'Sign in to My dApp'
})
```

#### 2.4 使用 Provider

```typescript
const provider = sdk.getProvider()

// 与 Wallet API 相同的调用方式
const balance = await provider.request({
  method: 'eth_getBalance',
  params: [address, 'latest']
})
```

#### 2.5 断开连接

```typescript
await sdk.terminate()
```

### 3. React SDK

```tsx
import { OneKeyProvider, useOneKey } from '@onekeyhq/sdk-react'

// 包裹应用
function App() {
  return (
    <OneKeyProvider
      sdkOptions={{
        dappMetadata: {
          name: 'My dApp',
          url: window.location.href
        }
      }}
    >
      <MyComponent />
    </OneKeyProvider>
  )
}

// 使用 Hook
function MyComponent() {
  const { sdk, account, chainId, connect, disconnect } = useOneKey()

  return (
    <div>
      {account ? (
        <>
          <p>Connected: {account}</p>
          <button onClick={disconnect}>Disconnect</button>
        </>
      ) : (
        <button onClick={connect}>Connect</button>
      )}
    </div>
  )
}
```

### 4. Vue SDK

(待补充)

### 5. 移动端 SDK

#### React Native

```tsx
import { useOneKey } from '@onekeyhq/sdk-react-native'

function App() {
  const { sdk, account, connect } = useOneKey()

  const handleConnect = async () => {
    try {
      await connect()
    } catch (error) {
      console.error('Connection failed:', error)
    }
  }

  return (
    <View>
      {account ? (
        <Text>Connected: {account}</Text>
      ) : (
        <Button title="Connect" onPress={handleConnect} />
      )}
    </View>
  )
}
```

#### 其他平台
- Flutter
- iOS (Swift)
- Android (Kotlin)

---

## 三、第三方库集成

> 使用成熟的 Web3 开发框架，获得更好的开发体验和 UI 组件。

### 1. 框架选型

| 框架 | 特点 | EIP-6963 支持 | 适用场景 |
|------|------|--------------|----------|
| Wagmi 2+ | React Hooks, 类型安全 | 原生支持 | React 项目首选 |
| RainbowKit | 精美 UI, 开箱即用 | 原生支持 | 需要现成 UI |
| Web3Modal (AppKit) | WalletConnect 官方 | 原生支持 | 需要移动端扫码 |
| Web3-Onboard | 多钱包管理 | 原生支持 | 需要深度定制 |
| ConnectKit | 简洁 UI | 原生支持 | 追求简洁 |

### 2. Wagmi + viem

#### 2.1 安装

```bash
npm install wagmi viem @tanstack/react-query
```

#### 2.2 配置

```typescript
import { createConfig, http } from 'wagmi'
import { mainnet, polygon, arbitrum } from 'wagmi/chains'
import { injected, walletConnect } from 'wagmi/connectors'

const config = createConfig({
  chains: [mainnet, polygon, arbitrum],
  connectors: [
    injected(), // 自动支持 EIP-6963，发现所有钱包
    walletConnect({ projectId: 'YOUR_PROJECT_ID' }) // 移动端扫码
  ],
  transports: {
    [mainnet.id]: http(),
    [polygon.id]: http(),
    [arbitrum.id]: http()
  }
})
```

#### 2.3 Provider 设置

```tsx
import { WagmiProvider } from 'wagmi'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

function App() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <YourApp />
      </QueryClientProvider>
    </WagmiProvider>
  )
}
```

#### 2.4 常用 Hooks

```tsx
import {
  useAccount,
  useConnect,
  useDisconnect,
  useBalance,
  useSendTransaction,
  useSignMessage,
  useSwitchChain
} from 'wagmi'

function WalletInfo() {
  // 账户信息
  const { address, isConnected, chain } = useAccount()

  // 连接
  const { connect, connectors, isPending } = useConnect()

  // 断开
  const { disconnect } = useDisconnect()

  // 余额
  const { data: balance } = useBalance({ address })

  // 切换链
  const { switchChain } = useSwitchChain()

  // 签名
  const { signMessage } = useSignMessage()

  // 发送交易
  const { sendTransaction } = useSendTransaction()

  return (
    <div>
      {isConnected ? (
        <>
          <p>Address: {address}</p>
          <p>Balance: {balance?.formatted} {balance?.symbol}</p>
          <p>Chain: {chain?.name}</p>
          <button onClick={() => signMessage({ message: 'Hello' })}>
            Sign Message
          </button>
          <button onClick={() => disconnect()}>Disconnect</button>
        </>
      ) : (
        connectors.map(connector => (
          <button
            key={connector.id}
            onClick={() => connect({ connector })}
            disabled={isPending}
          >
            {connector.name}
          </button>
        ))
      )}
    </div>
  )
}
```

### 3. RainbowKit

#### 3.1 安装

```bash
npm install @rainbow-me/rainbowkit wagmi viem @tanstack/react-query
```

#### 3.2 配置

```tsx
import '@rainbow-me/rainbowkit/styles.css'
import { getDefaultConfig, RainbowKitProvider } from '@rainbow-me/rainbowkit'
import { WagmiProvider } from 'wagmi'
import { mainnet, polygon, arbitrum } from 'wagmi/chains'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const config = getDefaultConfig({
  appName: 'My dApp',
  projectId: 'YOUR_WALLETCONNECT_PROJECT_ID',
  chains: [mainnet, polygon, arbitrum]
})

const queryClient = new QueryClient()

function App() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>
          <YourApp />
        </RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  )
}
```

#### 3.3 使用连接按钮

```tsx
import { ConnectButton } from '@rainbow-me/rainbowkit'

function Header() {
  return (
    <nav>
      <ConnectButton />
    </nav>
  )
}

// 自定义按钮
function CustomButton() {
  return (
    <ConnectButton.Custom>
      {({ account, chain, openConnectModal, openChainModal, mounted }) => {
        const connected = mounted && account && chain

        return (
          <div>
            {!connected ? (
              <button onClick={openConnectModal}>Connect Wallet</button>
            ) : (
              <>
                <button onClick={openChainModal}>{chain.name}</button>
                <span>{account.displayName}</span>
              </>
            )}
          </div>
        )
      }}
    </ConnectButton.Custom>
  )
}
```

### 4. Web3Modal (AppKit)

#### 4.1 安装

```bash
npm install @web3modal/wagmi wagmi viem @tanstack/react-query
```

#### 4.2 配置

```tsx
import { createWeb3Modal, defaultWagmiConfig } from '@web3modal/wagmi/react'
import { WagmiConfig } from 'wagmi'
import { mainnet, polygon } from 'wagmi/chains'

const projectId = 'YOUR_WALLETCONNECT_PROJECT_ID'

const metadata = {
  name: 'My dApp',
  description: 'My dApp description',
  url: 'https://mydapp.com',
  icons: ['https://mydapp.com/icon.png']
}

const chains = [mainnet, polygon]
const config = defaultWagmiConfig({ chains, projectId, metadata })

createWeb3Modal({ wagmiConfig: config, projectId, chains })

function App() {
  return (
    <WagmiConfig config={config}>
      <YourApp />
    </WagmiConfig>
  )
}
```

#### 4.3 使用

```tsx
import { useWeb3Modal } from '@web3modal/wagmi/react'

function ConnectButton() {
  const { open } = useWeb3Modal()

  return <button onClick={() => open()}>Connect</button>
}
```

### 5. 多链支持 (第三方库)

> 第三方库通常对 EVM 链有较好支持，非 EVM 链可能需要额外处理。

#### 5.1 EVM 兼容链

```typescript
// wagmi 配置多链
import { mainnet, polygon, arbitrum, optimism, base } from 'wagmi/chains'

const config = createConfig({
  chains: [mainnet, polygon, arbitrum, optimism, base],
  // ...
})

// 动态切换
const { switchChain } = useSwitchChain()
switchChain({ chainId: polygon.id })
```

#### 5.2 非 EVM 链

第三方库主要支持 EVM 链。对于非 EVM 链 (Bitcoin, Solana 等)，建议:

```tsx
// 混合方案: 第三方库 (EVM) + Wallet API (非 EVM)
function MultiChainApp() {
  // EVM 链使用 wagmi
  const { address: evmAddress } = useAccount()

  // 非 EVM 链使用原生 Provider
  const [btcAddress, setBtcAddress] = useState<string>()
  const [solAddress, setSolAddress] = useState<string>()

  const connectBitcoin = async () => {
    const accounts = await window.$onekey.btc.requestAccounts()
    setBtcAddress(accounts[0])
  }

  const connectSolana = async () => {
    const { publicKey } = await window.$onekey.solana.connect()
    setSolAddress(publicKey.toString())
  }

  return (
    <div>
      <p>EVM: {evmAddress}</p>
      <p>Bitcoin: {btcAddress}</p>
      <p>Solana: {solAddress}</p>
    </div>
  )
}
```

---

## 四、常见场景

### 1. 检测钱包安装状态

```typescript
// EIP-6963 方式 (推荐)
function detectOneKey(): Promise<boolean> {
  return new Promise((resolve) => {
    let found = false

    const handler = (event: EIP6963AnnounceProviderEvent) => {
      if (event.detail.info.rdns === 'so.onekey.app.wallet') {
        found = true
      }
    }

    window.addEventListener('eip6963:announceProvider', handler)
    window.dispatchEvent(new Event('eip6963:requestProvider'))

    // 等待 100ms 让钱包响应
    setTimeout(() => {
      window.removeEventListener('eip6963:announceProvider', handler)
      resolve(found)
    }, 100)
  })
}

// 传统方式
const hasOneKey = typeof window.$onekey !== 'undefined'
```

### 2. 自动重连

```typescript
// 检查是否已授权
async function checkConnection(provider: EIP1193Provider) {
  const accounts = await provider.request({ method: 'eth_accounts' })
  return accounts.length > 0 ? accounts[0] : null
}

// React Hook
function useAutoConnect() {
  const [account, setAccount] = useState<string | null>(null)

  useEffect(() => {
    const provider = window.$onekey?.ethereum
    if (!provider) return

    checkConnection(provider).then(setAccount)

    provider.on('accountsChanged', (accounts: string[]) => {
      setAccount(accounts[0] || null)
    })
  }, [])

  return account
}
```

### 3. 交易发送与确认

```typescript
import { createPublicClient, http } from 'viem'
import { mainnet } from 'viem/chains'

const publicClient = createPublicClient({
  chain: mainnet,
  transport: http()
})

async function sendAndWait(provider: EIP1193Provider, tx: any) {
  // 1. 发送交易
  const hash = await provider.request({
    method: 'eth_sendTransaction',
    params: [tx]
  }) as `0x${string}`

  // 2. 等待确认
  const receipt = await publicClient.waitForTransactionReceipt({ hash })

  return receipt
}

// 使用
const receipt = await sendAndWait(provider, {
  from: account,
  to: '0x...',
  value: '0x' + (0.01 * 1e18).toString(16) // 0.01 ETH
})

console.log('Transaction confirmed:', receipt.transactionHash)
```

### 4. 处理用户拒绝

```typescript
async function safeRequest(provider: EIP1193Provider, method: string, params?: any[]) {
  try {
    return await provider.request({ method, params })
  } catch (error: any) {
    if (error.code === 4001) {
      // 用户拒绝
      console.log('User rejected the request')
      return null
    }
    throw error
  }
}
```

---

## 五、API 参考

### 1. JSON-RPC 方法

#### 账户与连接

| 方法 | 说明 |
|------|------|
| `eth_requestAccounts` | 请求用户授权连接 |
| `eth_accounts` | 获取已授权的账户列表 |
| `eth_chainId` | 获取当前链 ID |

#### 交易

| 方法 | 说明 |
|------|------|
| `eth_sendTransaction` | 发送交易 |
| `eth_getTransactionReceipt` | 获取交易回执 |
| `eth_call` | 只读合约调用 |
| `eth_estimateGas` | 估算 Gas |

#### 签名

| 方法 | 说明 |
|------|------|
| `personal_sign` | 签名消息 (EIP-191) |
| `eth_signTypedData_v4` | 签名结构化数据 (EIP-712) |

#### 网络

| 方法 | 说明 |
|------|------|
| `wallet_switchEthereumChain` | 切换网络 |
| `wallet_addEthereumChain` | 添加自定义网络 |

### 2. 错误码

| 错误码 | 说明 | 处理建议 |
|--------|------|----------|
| 4001 | 用户拒绝请求 | 提示用户操作已取消 |
| 4100 | 未授权 | 重新请求连接 |
| 4200 | 不支持的方法 | 检查方法名是否正确 |
| 4900 | Provider 断开连接 | 重新初始化连接 |
| 4901 | 链断开连接 | 切换到可用网络 |
| 4902 | 网络未添加 | 调用 wallet_addEthereumChain |

---

## 六、迁移指南

### 1. 从 window.ethereum 迁移到 EIP-6963

```typescript
// Before: 传统方式
const provider = window.ethereum
const accounts = await provider.request({ method: 'eth_requestAccounts' })

// After: EIP-6963
const providers: EIP6963ProviderDetail[] = []

window.addEventListener('eip6963:announceProvider', (event) => {
  providers.push(event.detail)
})
window.dispatchEvent(new Event('eip6963:requestProvider'))

// 让用户选择钱包
const selectedProvider = providers[userChoice].provider
const accounts = await selectedProvider.request({ method: 'eth_requestAccounts' })
```

### 2. 从其他钱包迁移到 OneKey

OneKey 兼容 EIP-1193 和 EIP-6963 标准，迁移成本极低：

```typescript
// 只需修改检测逻辑
// Before
const isMetaMask = window.ethereum?.isMetaMask

// After
const isOneKey = window.$onekey !== undefined
// 或使用 EIP-6963
const isOneKey = providers.some(p => p.info.rdns === 'so.onekey.app.wallet')
```

---

## 硬件层 (OneKey 硬件钱包)

> 后续补充

### 1. 概述

### 2. 连接方式
- WebUSB
- Bluetooth
- OneKey Bridge

### 3. 设备通信

### 4. 固件与安全

---

## cross-inpage-provider 架构分析

### 1. 仓库概述

[cross-inpage-provider](https://github.com/onekeyhq/cross-inpage-provider) 是 OneKey 的核心 Provider 注入库，负责在浏览器扩展、桌面应用、移动端 WebView 等不同平台向网页注入 Provider。

### 2. 包结构

```
cross-inpage-provider/
├── packages/
│   ├── 核心基础设施
│   │   ├── core/           # 核心通信逻辑
│   │   ├── types/          # TypeScript 类型定义
│   │   ├── errors/         # 错误处理
│   │   └── events/         # 事件系统
│   │
│   ├── 平台适配器
│   │   ├── injected/       # 注入层抽象
│   │   ├── extension/      # 浏览器扩展适配
│   │   ├── desktop/        # 桌面应用适配
│   │   ├── native/         # 原生应用适配
│   │   └── webview/        # WebView 适配
│   │
│   └── 链专属 Provider (实现各链 RPC 接口)
│       ├── onekey-eth-provider/      # EVM (EIP-1193/6963)
│       ├── onekey-btc-provider/      # Bitcoin
│       ├── onekey-solana-provider/   # Solana
│       ├── onekey-cosmos-provider/   # Cosmos
│       ├── onekey-aptos-provider/    # Aptos
│       ├── onekey-sui-provider/      # Sui
│       ├── onekey-near-provider/     # NEAR
│       ├── onekey-nostr-provider/    # Nostr
│       ├── onekey-polkadot-provider/ # Polkadot
│       ├── onekey-cardano-provider/  # Cardano
│       ├── onekey-algo-provider/     # Algorand
│       ├── onekey-neo-provider/      # NEO
│       ├── onekey-alph-provider/     # Alephium
│       └── onekey-conflux-provider/  # Conflux
```

### 3. 架构图

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           OneKey Provider 架构                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         dApp 层 (开发者接入)                          │   │
│  │                                                                     │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────────┐ │   │
│  │  │ Wallet API  │  │    SDK      │  │      第三方库                │ │   │
│  │  │ (EIP-6963)  │  │ (跨平台)    │  │ (Wagmi/RainbowKit/Web3Modal)│ │   │
│  │  └──────┬──────┘  └──────┬──────┘  └────────────┬────────────────┘ │   │
│  │         │                │                      │                   │   │
│  │         └────────────────┼──────────────────────┘                   │   │
│  │                          ▼                                          │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                             │                                               │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    Provider 接口层 (EIP-1193)                        │   │
│  │                                                                     │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   │
│  │  │   ETH    │ │   BTC    │ │  Solana  │ │  Cosmos  │ │  Aptos   │  │   │
│  │  │ Provider │ │ Provider │ │ Provider │ │ Provider │ │ Provider │  │   │
│  │  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘  │   │
│  │       │            │            │            │            │         │   │
│  │       └────────────┴────────────┼────────────┴────────────┘         │   │
│  │                                 ▼                                    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                 cross-inpage-provider (注入层)                       │   │
│  │                                                                     │   │
│  │  ┌─────────────────────────────────────────────────────────────┐   │   │
│  │  │                     packages/injected                        │   │   │
│  │  │              (统一的注入接口和通信协议)                         │   │   │
│  │  └─────────────────────────────────────────────────────────────┘   │   │
│  │                               │                                     │   │
│  │       ┌──────────┬───────────┼───────────┬──────────┐              │   │
│  │       ▼          ▼           ▼           ▼          ▼              │   │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐      │   │
│  │  │extension│ │ desktop │ │ native  │ │ webview │ │  core   │      │   │
│  │  │(浏览器) │ │(桌面App)│ │(原生App)│ │(内置浏览)│ │(核心通信)│      │   │
│  │  └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘      │   │
│  │       │           │           │           │           │            │   │
│  └───────┴───────────┴───────────┴───────────┴───────────┴────────────┘   │
│                                    │                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      OneKey 钱包 (签名与密钥管理)                     │   │
│  │                                                                     │   │
│  │  ┌───────────────────┐  ┌───────────────────┐  ┌─────────────────┐ │   │
│  │  │   软件钱包         │  │   硬件钱包         │  │   密钥派生       │ │   │
│  │  │ (扩展/桌面/App)    │  │ (OneKey Touch等)  │  │ (BIP32/39/44)   │ │   │
│  │  └───────────────────┘  └───────────────────┘  └─────────────────┘ │   │
│  │                                                                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4. 与 MetaMask 的对比

| 维度 | MetaMask | OneKey |
|------|----------|--------|
| **Provider 注入库** | @metamask/providers | cross-inpage-provider |
| **官方 SDK** | @metamask/sdk (跨平台) | 暂无独立 SDK 包 |
| **EIP-6963 支持** | 是 | 是 |
| **多链支持** | EVM + Solana (原生) + 其他链 (Snaps) | EVM + Bitcoin + Solana + Cosmos 等 14+ 链 (原生) |
| **硬件钱包** | 外部集成 (Ledger/Trezor) | 原生集成 (OneKey 硬件) |
| **开源程度** | 部分开源 | 完全开源 |

> **2025年更新**: MetaMask 已于 2025年5月添加 Solana 原生支持 (Wallet Standard)，Bitcoin 原生支持计划于 2025年Q3 推出。其他非EVM链通过 Snaps 生态支持。

### 5. 文档层级映射

```
文档结构                          对应 cross-inpage-provider 包
──────────────────────────────────────────────────────────────────

一、Wallet API
├── 1. 连接钱包
│   ├── EIP-6963 ───────────► onekey-eth-provider (EIP-6963 实现)
│   └── EIP-1193 ───────────► packages/injected (注入逻辑)
│
├── 2. Provider API ────────► packages/core + types
│
├── 3. 签名 ────────────────► onekey-eth-provider (签名方法)
│
├── 4. 网络管理 ────────────► onekey-eth-provider (链管理)
│
└── 5. 多链支持
    ├── Bitcoin ────────────► onekey-btc-provider
    ├── Solana ─────────────► onekey-solana-provider
    ├── Cosmos ─────────────► onekey-cosmos-provider
    ├── Aptos ──────────────► onekey-aptos-provider
    └── ...

二、SDK (待开发)
└── 跨平台连接 ─────────────► packages/desktop + native + webview

三、第三方库集成
└── 通过 EIP-6963 ──────────► onekey-eth-provider (发现机制)
```

### 6. OneKey 与 MetaMask 的功能差异

| 功能 | MetaMask | OneKey | 说明 |
|------|----------|--------|------|
| **官方跨平台 SDK** | @metamask/sdk | 无 | OneKey 依赖第三方库或 WalletConnect |
| **移动端深链接** | 有 (SDK 内置) | 需要 WalletConnect | 无自建深链接方案 |
| **EIP-7702 智能账户** | 有 (Delegation Toolkit) | 无 | OneKey 暂不支持 |
| **Snaps 扩展** | 有 | 无 | MetaMask 独有生态 |
| **硬件钱包集成** | 外部设备 | 原生硬件 | OneKey 硬件原生集成 |

#### 多链支持详细对比 (2025年最新)

| 链 | MetaMask | OneKey | 说明 |
|---|---|---|---|
| **EVM 链** | 原生支持 | 原生支持 | 两者都完全支持 |
| **Solana** | 原生支持 (2025.5) | 原生支持 | MetaMask 使用 Wallet Standard |
| **Bitcoin** | 计划 2025 Q3 | 原生支持 | OneKey 已支持 |
| **Starknet** | Snap 支持 | 不支持 | MetaMask 通过 Snap |
| **Cosmos** | Snap 支持 | 原生支持 | OneKey 使用 Keplr 兼容接口 |
| **Aptos** | Snap 支持 | 原生支持 | OneKey 使用 Petra 兼容接口 |
| **Sui** | Snap 支持 | 原生支持 | |
| **NEAR** | 不支持 | 原生支持 | |
| **Polkadot** | Snap 支持 | 原生支持 | |
| **Cardano** | Snap 支持 | 原生支持 | |

> **核心差异**: MetaMask 通过 Snaps 生态扩展非EVM链，需要用户安装对应 Snap；OneKey 使用内置 Provider，开箱即用。

### 7. OneKey 文档设计建议

基于以上分析，OneKey dApp 文档应该：

1. **突出多链优势**: 14+ 链的原生 Provider 支持是核心差异化

2. **简化 SDK 章节**: 无官方 SDK，推荐使用第三方库 + WalletConnect

3. **强调 EIP-6963**: 作为首选连接方式，与 MetaMask 一致

4. **增加硬件章节**: 展示 OneKey 硬件钱包的原生集成优势

5. **保持 API 兼容**: Provider API 与 MetaMask 兼容，降低迁移成本

---

## OneKey dApp 文档最终架构设计

### 推荐目录结构

```
/connect-to-software (dApp 接入)
│
├── index.mdx                      # 概览与快速入门
│   ├── 选择接入方式决策树
│   ├── 5 分钟快速连接示例
│   └── 平台能力对比表
│
├── /wallet-api                    # 一、原生 Provider (Wallet API)
│   ├── index.mdx                  # 概览
│   ├── connect.mdx                # 连接钱包
│   │   ├── EIP-6963 (推荐)
│   │   └── EIP-1193 (传统)
│   ├── provider.mdx               # Provider 接口
│   ├── signing.mdx                # 签名
│   │   ├── personal_sign
│   │   ├── signTypedData_v4
│   │   └── SIWE
│   ├── transactions.mdx           # 交易
│   ├── networks.mdx               # 网络管理
│   └── events.mdx                 # 事件监听
│
├── /multi-chain                   # 二、多链支持 (OneKey 差异化)
│   ├── index.mdx                  # 多链概览与架构图
│   ├── ethereum.mdx               # EVM 链
│   ├── bitcoin.mdx                # Bitcoin
│   ├── solana.mdx                 # Solana
│   ├── cosmos.mdx                 # Cosmos
│   ├── aptos.mdx                  # Aptos
│   ├── sui.mdx                    # Sui
│   └── others.mdx                 # 其他链 (NEAR, Polkadot, Cardano...)
│
├── /frameworks                    # 三、框架集成
│   ├── index.mdx                  # 框架选型指南
│   ├── wagmi.mdx                  # Wagmi + viem
│   ├── rainbowkit.mdx             # RainbowKit
│   ├── web3modal.mdx              # Web3Modal (AppKit)
│   ├── web3-onboard.mdx           # Web3-Onboard
│   └── walletconnect.mdx          # WalletConnect (移动端连接)
│
├── /guides                        # 四、开发指南
│   ├── best-practices.mdx         # 最佳实践
│   ├── migration.mdx              # 迁移指南 (从 MetaMask / 其他钱包)
│   ├── troubleshooting.mdx        # 常见问题
│   └── security.mdx               # 安全建议
│
└── /reference                     # 五、API 参考
    ├── rpc-methods.mdx            # JSON-RPC 方法完整列表
    ├── error-codes.mdx            # 错误码
    └── typescript.mdx             # TypeScript 类型定义
```

### 与 MetaMask 文档结构对比

```
MetaMask 文档                      OneKey 文档 (建议)
─────────────────────────────────────────────────────────────────

Connect to MetaMask                dApp 接入
├── MetaMask SDK ─────────────────► [移除] 无官方 SDK
│   ├── JavaScript                  替代: 框架集成 + WalletConnect
│   ├── React
│   ├── Vue
│   └── Mobile SDKs
│
├── Wallet API ───────────────────► Wallet API (保留，核心)
│   ├── Connect (EIP-6963)          EIP-6963 首选
│   ├── Sign data                   签名章节
│   ├── Manage networks             网络管理
│   └── Third-party libraries       框架集成
│
├── Create embedded wallets ──────► [移除] 无此功能
│
├── Create smart accounts ────────► [移除] 无 EIP-7702 支持
│
└── Extend and scale ─────────────► [移除] 无 Snaps/Infura

                                   [新增] 多链支持
                                   ├── Bitcoin
                                   ├── Solana
                                   ├── Cosmos
                                   └── 10+ 其他链

                                   [新增] 硬件钱包集成
                                   └── (在 /hardware 章节)
```

### 核心页面内容规划

#### 1. 概览页 (index.mdx)

```markdown
# dApp 接入

OneKey 提供 EIP-1193/6963 兼容的 Provider，支持 14+ 条公链。

## 接入方式选择

| 场景 | 推荐方案 |
|------|----------|
| 仅需 EVM，快速集成 | 框架集成 (RainbowKit/Wagmi) |
| 多链 dApp | Wallet API + 各链 Provider |
| 精细控制 | 原生 Wallet API |
| 移动端扫码 | WalletConnect |

## 快速开始

[5 行代码连接 OneKey 示例]
```

#### 2. EIP-6963 连接页

```markdown
# 连接钱包 (EIP-6963)

推荐使用 EIP-6963 协议发现并连接 OneKey。

## 为什么选择 EIP-6963

- 多钱包共存，用户选择
- 无 window.ethereum 冲突
- 所有主流钱包支持

## TypeScript 完整示例

[Vanilla + React 两个版本]

## OneKey 标识

rdns: `so.onekey.app.wallet`
```

#### 3. 多链概览页

```markdown
# 多链支持

OneKey 原生支持 14+ 条公链，每条链有独立的 Provider。

## 架构

[ASCII 架构图: 展示 $onekey 命名空间下的各链 Provider]

## Provider 入口

| 链 | Provider 路径 | 接口标准 |
|----|--------------|----------|
| EVM | window.$onekey.ethereum | EIP-1193 |
| Bitcoin | window.$onekey.btc | Unisat 兼容 |
| Solana | window.$onekey.solana | Phantom 兼容 |
| ... | ... | ... |
```

### 与现有 OneKey 文档的整合

当前 OneKey 文档结构 (`/content/zh/connect-to-software/`):

```
connect-to-software/
├── _meta.js
├── index.mdx                # 保留，更新内容
├── /provider                # 重构为 /wallet-api
│   ├── eth.mdx
│   ├── btc.mdx
│   ├── solana.mdx
│   └── ...
├── /wallet-kits             # 重构为 /frameworks
│   ├── rainbowkit.mdx
│   ├── web3modal.mdx
│   └── ...
└── /guides                  # 保留
```

**建议改动:**

1. 将 `/provider` 拆分为 `/wallet-api` (连接/签名/交易) + `/multi-chain` (各链 API)
2. 将 `/wallet-kits` 重命名为 `/frameworks`，扩充内容
3. 增加 `/reference` 作为 API 完整参考

### 差异化内容策略

| MetaMask 有，OneKey 无 | 处理方式 |
|------------------------|----------|
| 官方 SDK | 在框架集成章节推荐 WalletConnect |
| EIP-7702 智能账户 | 不提及，后续支持再添加 |
| Snaps 扩展 | 不提及 |
| Infura 服务 | 推荐公共 RPC 或自建节点 |

| OneKey 有，MetaMask 无 | 如何突出 |
|------------------------|----------|
| 多链原生 Provider | 专门章节，架构图展示 |
| 硬件钱包原生集成 | 在硬件章节深入 |
| 完全开源 | 在概览提及，附 GitHub 链接 |

---

## 讨论记录

### MetaMask 分析要点

1. **分类依据**: 平台覆盖范围 + 连接方式封装程度
   - Wallet API: 直接与扩展交互，Web-only
   - SDK: 跨平台封装，支持移动端深链接

2. **EIP-6963 定位**: 作为 Wallet API 的首选推荐方案

3. **Quick Start 结构**: 按接入方式分类，每种方式提供多平台 SDK

4. **文档特点**:
   - 清晰的场景区分 (Web vs 跨平台)
   - 完整的代码示例 (Vanilla TS + React TS)
   - 第三方库推荐列表

5. **SDK 深链接/QR码/Session 机制**:
   - 解决移动端浏览器无法直接访问钱包 App 的问题
   - 使用 ECIES 加密 + Socket.io 建立安全通道
   - Session 管理处理后台暂停/恢复

6. **SDK vs 第三方库**:
   - SDK: 单一钱包的跨平台优化
   - 第三方库: 多钱包的统一抽象层
   - 两者可结合使用
