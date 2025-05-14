---
timezone: UTC+2
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区


# 你的名字

1. 自我介绍：我是 Zion，目前是一名合约开发者，熟悉 EVM 与 Solana 智能合约开发，基于跨链协议 LayerZero 开发了跨链 Token 与 Dex 合约。
2. 你认为你会完成本次残酷学习吗？I wish I can
3. 你的联系方式（推荐 Telegram）X: zlog_in

## Notes

<!-- Content_START -->

### 2025.05.14

#### EIP-7702 速览

* EIP-7702 目的是将 EOA 账号托管给合约账号，通过引入新的交易类型 `0x04` 实现授权托管与取消托管
* `0x04` 交易结构： `[chain_id, nonce, max_priority_fee_per_gas, max_fee_per_gas, gas_limit, destination, value, data, access_list, authorization_list,v, r, s]`
* 其中：`authorization_list = [[chain_id, address, nonce, v, r, s], ...]`
* `authorization_list` 是一组 7702 签名数据，通过 `v,r,s` 获取 EOA Signer 地址，并将 `signer.code` 设置为 `0xef0100+address`, `address`  为目标合约地址，如果为〇地址，则为取消授权
* `0x04` 交易其实是在 `0x03` (EIP-1559)基础上，挂载了一个额外的字段 `authorization_list` ，此字段内容将实现一组授权托管/取消托管操作；外部交易数据依然为一笔正常`0x03`交易，即可以为转账，或合约调用。`0x03` 部分与 `authorization_list` 可彼此独立
* 更详细内容可参见[此资料](https://docs.google.com/presentation/d/1CGHSzlRl-d8nxzPtAYF6Aq5wkCORvxg3SFs96TICAPg/edit?usp=sharing)
<!-- Content_END -->