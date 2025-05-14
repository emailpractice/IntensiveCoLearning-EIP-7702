---
timezone: UTC+8
---


# fffuuuming
1. 自我介绍:
我目前為台灣大學資工碩一的學生，於大四時接觸到以太坊和智慧合約，對此相當有興趣並開始自學。碩班的研究主題也聚焦在smart contract, defi protocol 相關的 security issue，比較熟的主要有 price manipulation attack, collision in proxy contract 的 auto detection。
由於太晚認識到 DeFiHackLab這個社群，導致之前的殘酷共學沒參加到，但我也有自己把 WTF solidity 上的教學看過。這次看到有新的共學便決定要參加，在此也許願 Solidity 跟 CTF 的殘酷共學有機會能再開
2. 你认为你会完成本次残酷学习吗？
會的，我在今年ㄧ、二月時即有大概看過eip-7702並與eip-4337, eip-3074比較，相信這次能快速上手
3. 你的联系方式（推荐 Telegram）
https://t.me/fffuuuming
## Notes

<!-- Content_START -->

### 2025.07.11
### 2025-05-14
#### Core idea of EIP-7702
- **What** : Account Abstraction, allow EOAs to have code, which enable EOAs for batch operations, implementing native multisig, or alternative signature schemes
- **How** : Introduces a new transaction type `SET_CODE_TX_TYPE (0x04)` that allows accounts to set and delegate code on themselves
    - **delegation designator** : a specific prefix, which is followed by an address and stored in the **account's code** instead of the full contract code
        - (```0xef0100``` || ```address```)
        -  Indicates where the actual smart contract code resides on-chain.
    - **authorization list** : ```[chain_id, address, nonce, y_parity, r, s]```
#### Some notes
- Multiple wallets can point to the same delegation designator contract at the same time.
- Can **redelegate** from one delegation designator contract to another, but be aware of **collision**
- The owner of the EOA can **clear the code by delegating to address(0)**. This will restore your EOA back to normal.
#### Security Risk & Vulnerabilities
1. Delegate contract lacks proper access controls
2. Initialization challenges
    - **Constructors** : The delegation transaction doesn’t include ```initcode```,  so the ```constructor``` of the delegation designator contract does not execute in the context of the EOA -> use **initialization pattern**
    - **Front running and (re)initialization**
        - ensure that the ```initialize``` function has proper access controls and cannot be re-initialized
        - delegate and call the ```initialize``` function in the same transaction, otherwise it may be frontran
3. Storage collisions
Delegating code does not clear existing storage
    ```solidity!
    //SPDX-License-Identifier: MIT
    pragma solidity 0.8.20;

    contract ContractA{
        bool thisIsABool = true;
    }

    contract ContractB{
        uint thisIsAUint;
    }
    ```
    - migrate from ```ContractA``` to ```ContractB```
    - Ignore accountting for the old storage data
    - Storage collision : ```uint``` in the second contract will be interpreted as a ```bool```

### 2025.07.12

<!-- Content_END -->
