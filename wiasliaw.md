---
timezone: UTC+8
---


# wiasliaw

1. 自我介紹：技術上的 E 衛兵
2. 會完成殘酷學習嗎：會
3. tg@wiasliaw

## Notes

<!-- Content_START -->

### 2025.05.14

#### How it works

- 7702 透過 eip-2718 引入一個新的交易類型，允許 EOA 指定一個合約作為其程式碼。這樣使得 EOA 可以像是合約執行 evm bytecode 同時能保有發起交易的能力。

#### set code transaction

- set code transaction 的 type 為 0x04，主要多出一個 authorization_list 欄位
- 執行 set code transaction
    1. 先驗證 authorization_list 並處理 set code
    2. 再來執行原本的 transaction
- 交易內容如下：
    
    ```tsx
    const eip_7702_transaction = bytes.concat(
      // transaction type
    	"0x04",
    	// transaction payload
    	rlp([
    		chain_id,
    		nonce,
    		max_priority_fee_per_gas, // eip-1559
    		max_fee_per_gas, gas_limit, // eip-1559
    		to,
    		value,
    		data,
    		access_list, // eip-2930
    		authorization_list, // eip-7702
    		signature_y_parity,
    		signature_r,
    		signature_s
    	])
    );
    ```
    

#### authorization_list

- 要將一個 EOA 升級成 smart contract account 需要 EOA 簽名 authorization 的資料
    
    ```python
    authorization_list = [
    	[chain_id, address, nonce, y_parity, r, s],
    	...
    ]
    ```
    
- 對 authorization 的驗證流程如下
    1. 驗證 chain ID 是否為 0 或為當前鏈的 ID。這表示支援 cross-chain replay
    2. 驗證 nonce 是否小於 `2**64 - 1`。
    3. 驗證簽名 `authority = ecrecover(msg, y_parity, r, s)`
    4. 將 authority 加入 accessed_addresses，如 EIP-2929 所定義
    5. 驗證 `authority` 的代碼是否為空或已被委派
    6. 驗證 `authority` 的 nonce 是否等於 `nonce`
    7. 若 `authority` 非空，則向全域退款計數器增加 `PER_EMPTY_ACCOUNT_COST - PER_AUTH_BASE_COST` 的 gas
    8. 將 `authority` 的代碼設為 `0xfe0100 || address`
        - 如果 address 為 zero address 則清除 authority 的 code region，並將其 code hash 設置成 empty code hash
    9. 將 `authority` 的 nonce 增加 1
- 如果驗證中任一步驟失敗的話，則跳過當前的 authorization element，並開始處理下一個 authorization element
- 如果後續的 transaction revert，authorization 對 EOA 的寫入並不會 revert

#### delegation indicator

- 寫入到 `authority` 的資訊不是可以直接執行的 evm bytecode，而是一個指示器，用以指示 evm 要以不同的方式處理 code region
- `*CALL` 都是受影響的 evm opcode，當一讀到指示器時，會從 `address` 讀取其 code region
- 另外受影響的 opcode 還有 `CODESIZE` 和 `CODECOPY`，同樣也會從 `address` 讀取其 code region
- 但是 `EXTCODESIZE` `EXTCODECOPY` `EXTCODEHASH`則不受影響

#### Reference

- https://eips.ethereum.org/EIPS/eip-7702
- https://x.com/blainemalone/status/1893428082653962306

<!-- Content_END -->