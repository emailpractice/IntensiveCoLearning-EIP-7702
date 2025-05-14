---
timezone: UTC+8
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区


# 你的名字

1. 自我介绍
Kevin
2. 你认为你会完成本次残酷学习吗？
会，而且想对安全方面进行更加深度的挖掘。
3. 你的联系方式（推荐 Telegram）
@Kev1nWeb3

## Notes

<!-- Content_START -->

### 2025.05.14

#### Day1 研究EIP-7702的过去：从账户抽象到原生合约钱包的演化
在以太坊中，传统的 EOA（外部拥有账户）功能有限且风险较高，因此，如何对 EOA 账户进行抽象和拓展，一直是许多过去提案试图解决的重要目标。
- EIP-4337
- EIP-3074
- EIP-5003

只有在了解先前这些提案的基础上，我们才能对 EIP-7702 有一个更加完整和深入的认识。
##### EIP-4337
账户抽象（Account Abstraction）
通过高层基础设施实现账户抽象，避免更改共识层协议。主要组件包括：
- 智能合约钱包：链上部署的合约，管理用户账户权限和资源。
- Bundler：EOA钱包，代表用户验证和执行UserOperation交易。
- Entry Point Contract：链上全局合约，Bundler通过它执行UserOperation，充当Bundler与智能合约钱包的中介。
- Paymaster：实现Gas抽象，支持用ERC20代币支付Gas或无Gas交易。
- Wallet Factory：用于创建智能合约钱包的合约。
- Signature Aggregator：优化签名验证，降低交易成本。
整体流程：
1. 用户通过前端构造UserOperation以及签名
2. Bundler收集UserOperation，并验证签名的有效性，将其打包成批量交易（Paymaster介入，可以用ERC20代币支付Gas或无Gas交易； Signature Aggregator用于优化签名验证，降低交易成本）
3. Entry Point Contract收到Bundler的批量交易，调用智能合约钱包执行UserOperation，交易上链。
目前的一些数据（2025/5/14）
ETH：
61,115 Accounts, 277,345 UserOps, 278,291 Transactions, 400.31 ETH gas, 125.8517 ETH revenue(Bundler)
##### EIP-3074
在EVM中引入了两条新指令`AUTH`和`AUTHCALL`，允许外部拥有账户（EOA）将其控制权委托给智能合约，从而实现批量交易、Gas 赞助、交易到期设置和脚本化操作等功能。这增强了 EOA 的灵活性和用户体验，与智能合约钱包的部分功能类似，但无需更改账户类型。
##### EIP-5003
引入了一个新的操作码`AUTHUSURP`
EOA先通过EIP-3074的机制授权某个智能合约，然后`AUTHUSURP`允许将代码直接部署到这个EOA的地址上

### 2025.05.15

<!-- Content_END -->
