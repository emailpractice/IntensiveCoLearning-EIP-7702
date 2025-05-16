---
timezone: UTC+12
---

# Bruce Xu

1. 自我介绍：E 卫兵
2. 你认为你会完成本次残酷学习吗？：会
3. 你的联系方式（推荐 Telegram）：@brucexu_eth

## Notes

<!-- Content_START -->

## 2025.05.14

# https://eip.fun/eips/eip-7702

This EIP therefore focuses on adding short-term functionality improvements to EOAs which will allow UX improvements to permeate through the entire application stack.

三大用例（TODO 分别制作 Demo）：

- Batching：多个交易合并一笔
- Sponsorship：使用其他账号和 ERC-20 来为当前账号代付 gas
- Privilege de-escalation：授权 sub-keys 用于低安全性使用场景，这样可以减少签名的操作

# 2025.05.15

authorization_list = [[chain_id, address, nonce, y_parity, r, s], ...]

The authorization list is processed before the execution portion of the transaction begins, but after the sender's nonce is incremented.

EOA 的执行流程：

1. 发送 SET_CODE_TX_TYPE 0x04 + authorization_list 交易，包含授权的信息
2. EOA 对 authorization_list 签名 + 循环验证 pre-execution checks
3. 依次处理 `authorization_list` 中的每个授权条目：验证授权并将有效授权设置到对应 EOA 的代码区，使其指向一个委托合约地址（即设置 Delegation Indicator）。若同一 EOA 有多个授权，则以列表中最后一个有效授权为准。
4. `authorization_list` 处理完毕后，执行交易的主要操作（如 `data` 字段中定义的调用）。此时，被成功设置了 Delegation Indicator 的 EOA 将作为代理（proxy）执行其委托合约的代码。
5. 交易完成后，EOA 上设置的 Delegation Indicator 会持续存在，除非通过新的 EIP-7702 交易来更改或清除它。

Delegation Indicator：

- 设置到 EOA code 字段，告诉 EVM 用特殊的方式执行这个 code
  - 使用 0xef0100 开头，后面是 20 bytes 的以太坊地址，是一个智能合约，包含了当前 EOA 被调用时候，真正的执行逻辑
  - 存在 code 的 EOA，行为上像是代理合约
  - 当有交易跟 EOA 交互，EVM 会加载 indicator 指向的合约代码
  - 作为代理合约，被执行的合约代码使用 EOA 的 context，所以 msg.sender 和 msg.value 都是当前 EOA 的
- 通过 7702 设置了 Delegation Indicator 就会一直存在，除非被另一个 7702 tx 修改或者清理，不会自动消失，实现持久性

<!-- Content_END -->
