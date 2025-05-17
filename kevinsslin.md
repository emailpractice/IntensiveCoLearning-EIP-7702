---
timezone: UTC+8
---
# Kevin Lin

1. 自我介绍：我是 Kevin，來自台灣，喜歡學習 DeFi 並分享
2. 你认为你会完成本次残酷学习吗？：會的會的
3. 你的联系方式（推荐 Telegram）：[@kevinsslin](https://t.me/kevinsslin)

## Notes

<!-- Content_START -->

### 2025.05.14

see below

### 2025.05.15

# EIP-7702 Proposal Note

## One-Sentence Summary
EIP-7702 enables Externally Owned Accounts (EOAs) to set their own code via a special transaction, allowing them to act like smart contract wallets by delegating execution to a contract address.

## Motivation: Three Core Features
- **Batching**: Perform multiple operations (e.g., ERC-20 approve + transfer) in a single atomic transaction.
- **Sponsorship**: Allow a third party (like a DApp or relayer) to pay gas fees on behalf of a user.
- **Privilege De-escalation**: Enable sub-keys with limited permissions (e.g., only spend a specific token, set daily limits, or restrict to certain apps).

## EIP-2718 "Set Code Transaction" Structure
- **TransactionType**: `SET_CODE_TX_TYPE` (`0x04`)
- **TransactionPayload**:
  ```solidity
  rlp([
    chain_id,
    nonce,
    max_priority_fee_per_gas,
    max_fee_per_gas,
    gas_limit,
    destination,
    value,
    data,
    access_list,
    authorization_list,
    signature_y_parity,
    signature_r,
    signature_s
  ])
  ```
  - **authorization_list**:
    ```solidity
    [
      [chain_id, address, nonce, y_parity, r, s],
      ...
    ]
    ```
    - Each tuple is a signature authorizing the EOA (authority) to delegate to a contract (address).
    - `authority = ecrecover(msg, y_parity, r, s)`
    - `msg = keccak(MAGIC || rlp([chain_id, address, nonce]))`
  - The outer transaction fields follow the same semantics as EIP-4844.
  - The transaction signature (`signature_y_parity`, `signature_r`, `signature_s`) is over `keccak256(SET_CODE_TX_TYPE || TransactionPayload)`.
- **ReceiptPayload**:
  ```solidity
  rlp([status, cumulative_transaction_gas_used, logs_bloom, logs])
  ```

## Delegation Indicator
- Uses the banned opcode `0xef` (from EIP-3541), so the EVM treats the code as a delegation pointer, not regular code.
- Format: `0xef0100 || address`
- When called, the EVM loads and executes the code at `address` in the context of the EOA (authority).

## Misc
- If transaction execution fails (e.g., revert), the delegation indicator is not rolled back — the delegation remains.
- To change or remove delegation, send a new EIP-2718 transaction (set to a new contract or to the zero address to revert to a normal EOA).
- During delegated execution, `CODESIZE` and `CODECOPY` behave differently from `EXTCODESIZE` and `EXTCODECOPY` on the authority.
  - For example, when executing a delegated account:
    - `EXTCODESIZE` returns 23 (the size of `0xef0100 || address`)
    - `CODESIZE` returns the size of the code residing at the delegated address

## Breaking Invariants
- `tx.origin == msg.sender` is only true in the topmost execution frame.
  - This breaks some legacy patterns:
    - Ensuring `msg.sender` is an EOA (no longer reliable)
    - Protecting against atomic sandwich attacks (bad practice anyway)
    - Preventing reentrancy (rarely used for this purpose)

## Security Considerations
- **Storage Management**: Delegated contracts should avoid storage collisions (e.g., use ERC-7201 Storage Namespaces).
- **Replay Protection**: Delegated contracts should implement their own nonces and signature checks.

## References
- [EIP-7702: Set Code for EOAs](https://eips.ethereum.org/EIPS/eip-7702)
- [EIP-2718: Typed Transaction Envelope](https://eips.ethereum.org/EIPS/eip-2718)
- [EIP-4844: Shard Blob Transactions](https://eips.ethereum.org/EIPS/eip-4844)
- [EIP-3541: Reject New Contracts Starting with the 0xEF Byte](https://eips.ethereum.org/EIPS/eip-3541)
- [ERC-7201: Namespaced Storage Layout](https://eips.ethereum.org/EIPS/eip-7201)

### 2025.05.16

<!-- Content_END -->


