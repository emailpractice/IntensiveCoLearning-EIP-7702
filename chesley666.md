---
timezone: UTC+8
---

> 请在上边的 timezone 添加你的当地时区(UTC)，这会有助于你的打卡状态的自动化更新，如果没有添加，默认为北京时间 UTC+8 时区

# chesley666

1. 自我介绍：web2测试工程师转web3
2. 你认为你会完成本次残酷学习吗？会
3. 你的联系方式（推荐 Telegram）：@rorozn_OvQ_

## Notes

<!-- Content_START -->

### 2025.05.14  
1. 解读[EIP7702]([https://eips.ethereum.org/EIPS/eip-7702])提案 [视频资料](https://www.youtube.com/watch?v=uZTeYfYM6fM)/[文字版](https://slowmist.medium.com/in-depth-discussion-on-eip-7702-and-best-practices-968b6f57c0d5)
- EOA: externally owned account外部拥有账户  
- EIP7702：  
钱包将指向链上的智能合约，功能上类似starknet账户    
引入一种新的交易类型（0x04），允许账户自行设置和委托代码。它不再将完整的合约代码直接存储在账户中，而是存储一个**委托指示符**，该指示符由一个特定的前缀加上一个地址 ( (0xef0100 || address))组成。这个指针指向实际智能合约代码在链上的位置。  
7702交易的destination不能为空：不能是创建合约的交易。  

- 用途：  
让EOA账户拥有合约账户那样灵活的能力
保持原有地址不变，兼容现有工具
L1层面支持多签、社交恢复、
gas替付等能力：可以使用代币或者第三方等结算gas
批量合约调用：比如把approve和swap通过合约打包成1比交易，可以减少gas
会话session：比如定期转账或者授权dapp定投等

- 数据结构：  
授权列表：与1559对比：增加委托列表，至少1条 [chain_id授权的链, address目标合约地址, nonce, y_parity签名, r, s]   
同一条链只有最后1条生效  
**chainid为0**的话，在所有支持7702的连上重放/授权，务必先确定每条链都是安全的  
通过tx==msg.sender来检查交易是否合法的合约，需要重新设计安全检查，因为有了7702之后，交易发起者不一定是实际用户  

- 与 EIP-4337 比较：  
EIP-7702 集成更精简，而 EIP-4337 提供更全面的账户抽象模型，两者可针对不同场景共存。7702无需中转合约调用，相比4337多次合约调用，bundler打包，消耗更少的gas。7702需要通过硬分叉，节点、rpc、钱包都需要升级才能支持，而4337目前已经支持。[参考](https://mirror.xyz/0x9FFC14AB8754E4De3b0C763F58564D60f935Ad6F/eiLgBj9iPFmy4s4bmjY2jvEW_7g8YxYMQaHvqm9Xw_o)

### 2025.05.15  
2. 安全相关  
- 存储冲突：修改授权合约时，要确保修改前后合约存储的slot内容对应一致，[slot相关介绍](https://mixbytes.io/blog/collisions-solidity-storage-layouts)，或者利用ERC7201，使用[存储命名空间](https://www.rareskills.io/post/erc-7201)来避免冲突
- 骗gas的爆破？【待实践】
- 老的链上合约，有些使用tx==msg.sender来检验交易是否合法，在7702升级之后需要升级合约，因为存在7702的账户在和这种老合约交互的时候，msg.sender并不一定是真实用户的地址
3. 链上数据浏览
- [okx的代理合约](https://etherscan.io/address/0x80296FF8D1ED46f8e3C7992664D13B833504c2Bb)
```solidity
/**
* @dev Modifier to make a function callable by the account itself or EOA address under 7702
*/

modifier onlySelf() {
    if (msg.sender != address(this)) revert Errors.NotFromSelf();
    _;
}
```
- [设置7702的EOA](https://holesky.etherscan.io/tx/0x29252bf527155a29fc0df3a2eb7f5259564f5ee7a15792ba4e2ca59318080182) 对比 [取消设置的EOA](https://holesky.etherscan.io/tx/0xd410d2d2a2ad19dc82a19435faa9c19279fa5b96985988daad5d40d1a8ee2269#authorizationlist)

### 2025.05.16

### 2025.05.17

### 2025.05.18

<!-- Content_END -->
