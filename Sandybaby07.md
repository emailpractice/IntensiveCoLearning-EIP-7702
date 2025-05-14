---
timezone: UTC+8
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区


# 你的名字

1. 自我介绍 Hihi 我是Sandy
2. 你认为你会完成本次残酷学习吗？ 應該會：）
3. Telegram :sandyyy0707

## Notes

<!-- Content_START -->

### 2025.05.14

#### DESIGNATING A SMART CONTRACT TO AN EOA
role: EOA/Sequencer/Node

<img width="692" alt="image" src="https://github.com/user-attachments/assets/b846c750-ac2e-4b62-961f-dc60b7d7d9e8" />

```odyssey_sendTransaction```

odyssey_sendTransaction request that executes an EIP-7702 Transaction to designate a contract to the EOA (via authorizationList), as well as executing a function on it (via data):

```
curl 'https://odyssey.ithaca.xyz/' --data '{
  "jsonrpc":"2.0",
  "id":0,
  "method":"odyssey_sendTransaction",
  "params":[{
    "authorizationList":[{
      "address":"0x35202a6E6317F3CC3a177EeEE562D3BcDA4a6FcC",
      "chainId":"0xde9fb",
      "nonce":"0x0",
      "r":"0xc9e930f2051c331b994ca1ec5d398379923dc1aae3b1aaaccf4242e872b8ce79",
      "s":"0x182672f89807a836962b0f29ce566f30b1588bd8b6bd4f9a8e24b3675ed11c3e",
      "yParity":"0x0"
    }],
    "data":"0xdeadbeef00000000000000000000000000000000000000000000000000000000cafebabe",
    "to":"0xFC7b76a8cA893f00976a14559D08b77aa4e4Bf2e"
  }]
}'
```
#### EXECUTING A CALL TO A DELEGATED EOA
<img width="649" alt="image" src="https://github.com/user-attachments/assets/b0da9d47-b12f-49bd-ad71-61dbefa5e091" />

odyssey_sendTransaction request that executes an EIP-1559 Transaction to execute a function on a delegated EOA (via data):
```
curl 'https://odyssey.ithaca.xyz/' --data '{
  "jsonrpc":"2.0",
  "id":0,
  "method":"odyssey_sendTransaction",
  "params":[{
    "data":"0xdeadbeef00000000000000000000000000000000000000000000000000000000cafebabe",
    "to":"0xFC7b76a8cA893f00976a14559D08b77aa4e4Bf2e"
  }]
}'
```
#### SEQUENCER
Sequencer 應僅處理符合以下條件的交易：

- 為 EIP-7702 類型，且將智慧合約指定給一個 EOA（外部擁有帳戶），或為 EIP-1559 類型且指定至 EOA；

- 不超過 Sequencer 定義的每筆交易 gas 上限

- 不包含 value 欄位（或其值設為 0）

- 不包含 nonce 欄位（由 Sequencer 管理）

- 不包含 from 欄位（由 Sequencer 管理）

### 2025.05.15

<!-- Content_END -->
