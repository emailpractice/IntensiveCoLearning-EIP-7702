---
timezone: UTC+8
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区


# Garden

1. Hi 我是 Garden
2. 盡力完成
3. TG: @gardenxu

## Notes

<!-- Content_START -->

### 2025.05.14

### EIP-7702：概念入門與背景脈絡

#### 🧠 為什麼需要帳戶抽象？Account Abstraction 的動機

以太坊原生帳戶分為：
- EOA（Externally Owned Account）：傳統錢包，只有私鑰，不能內建邏輯。
- 合約帳戶（Contract Account）：有程式邏輯，可自訂執行規則，但部署後不易修改。

帳戶抽象（Account Abstraction）希望打破這個二分法：
- 讓 EOA 也能支援自訂邏輯（如多簽、限額、社交恢復）。
- 支援批次交易、自訂驗簽邏輯、由 DApp 代付 Gas 等功能。

#### ⛓ EIP-4337 回顧與限制

EIP-4337（Account Abstraction via EntryPoint contract）為第一代帳戶抽象方案，其設計包含：
- **Bundler**：收集使用者操作（UserOperation）並統一送出。
- **EntryPoint** 合約：統一處理驗證與執行。
- 支援 `paymaster`（由他人代付 gas）。

但也存在限制：
- 需要獨立 mempool，不被所有客戶端支援。
- 執行流程複雜、學習曲線高。
- 不屬於 L1 協議層功能，執行效率與整合有限。

#### 🔓 EIP-7702：核心概念與創新

EIP-7702 是 Vitalik 主導的新提案，核心為：
- 為 EOA 提供 **動態邏輯委派** 的能力。
- EOA 的 `code` 欄位可暫時設定為 `0xef0100 || address`，即指向某個邏輯合約（Delegation contract）。
- 在交易結束後，自動清除 `code` 欄位，回到原始狀態。

特色如下：
- 無需 Bundler，直接支援在 L1 上的帳戶抽象。
- 無需在帳戶地址重新部署合約，即可為 EOA 附加執行邏輯。
- 適合批次操作、gas 贊助、動態簽名驗證。

#### 🔢 Type 4 新交易格式與比較

| 類型 | 名稱       | 特點                         | 對應 EIP    |
|------|------------|------------------------------|-------------|
| 0    | Legacy     | 最早期的交易格式             | 無          |
| 1    | EIP-2930   | 支援 `access list`           | Berlin      |
| 2    | EIP-1559   | 動態 gas 模型（Base Fee）    | London      |
| 4    | EIP-7702   | 動態附加 delegation 行為     | Pectra      |

Type 4 是 7702 引入的專屬格式，允許一次性附加 delegation code pointer 與 authorization list。

#### 🔗 delegation pointer 的格式與意義

EIP-7702 使用的 delegation pointer：
```
0xef0100 || address
```
- `0xef0100` 是 prefix，代表這是一筆 delegation。
- `address` 是被委託的邏輯合約位址。
- 整體構成帳戶 `code` 欄位的內容，讓 EVM 執行時判斷要用 delegatecall 呼叫此地址。

### 2025.05.15

<!-- Content_END -->
