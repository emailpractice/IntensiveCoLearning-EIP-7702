---
timezone: UTC+8
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区


# 你的名字

1. 自我介绍   seashell，在工作之餘學習了 smart contract audit 兩個月，目前嘗試了兩次審計比賽跟一次 codehawk 的 first flight
2. 你认为你会完成本次残酷学习吗？ 會
3. 你的联系方式（推荐 Telegram） https://t.me/emailpractice

## Notes

<!-- Content_START -->

### 2025.07.11

### 2025.05.14

- What is EIP-7702 ?
EIP-7702 turns every account into a smart contract.
Alone, that’s useless — no user wants to write Solidity.
But with more and more "plugin" being deployed, wallets can really start to be customized :

✅ Set transfer limits to prevent hacks
✅ Pay gas with USDT or any token
✅ Batch txs, automation, allowlists...
________________________________________
- one of EIP-7702's problem 
EIP-7702 lets EOAs temporarily act like smart accounts by attaching custom contract code during a single transaction.

Under the hood, these work via DELEGATECALL.

That means: 
→ Your wallet storage is being directly modified by external contracts (plugins). 
→ Multiple plugins = multiple contracts sharing the same storage space.

This is one of the biggest blockers to EIP-7702 adoption.

Solution
 a. ERC-7201: it assigns unique storage slots to modules, avoiding collisions by design.

________________________________________
- EIP-7702 is compatible with older implementation of customize transaction ( ERC-4337 )

compatible means less pain of adopting newer EIP. that can greatly promote the usage of 7702. 

前情提要，什麼是 4337? 
同樣都要讓用戶能自訂交易邏輯。 4337 是去工廠去生出符合規範的智能合約，然後智能合約自然而然就能自訂交易邏輯 ; 有點像是EVM本身不予許自訂交易，所以要靠著一個額外的4337智能合約去當中介。 就是 4337 體系送出的交易不是讓 EVM 去驗證的，而是讓一個 bundler 去處理，總之就是盡量繞開EVM。 7702 才是真的去讓EVM接受客製交易
7702 不是靠著智能合約，而是讓 EOA自己可以在transaction的時候delegate call別人合約的程式碼，進而控制自己的交易邏輯； 
在4337的情境下，用戶為了客製交易需求，不僅需要新部屬一個智能合約，還要把 EOA 的資產轉過去智能合約。 而7702就可以方便的使用原本的帳戶來達成客製交易


而 4337 能與 7702 相容的原因是
EIP-7702 要求 EOA 在transaction的時候填寫的資料規範，與 EIP-4337 的工廠所要求的交易函數格式吻合。 所以EOA所送的交易，也能被 4337 的 bundler 處理。 

### 2025.07.12

<!-- Content_END -->
