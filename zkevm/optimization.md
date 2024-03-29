# 优化 / Optimization

## 1. 替换 merkle tree

将 L1 geth 原本的 hexary Merkle Patricia Trie (MPT) 换成了 compressed binary trie，用以减少电路复杂性和 witness 的大小。

## 2. 替换 hash

SHA3-256 (`keccak`) 在 Layer 1 EVM 中成本低（通过 precompile），但是在电路中证明成本很高。所以 scroll 选择了将一些 `keccak` hash 替换为 zkp-friendly 的 `Poseidon` hash。

具体来讲，Scroll 的 zkEVM 方案会替换掉：

+ Transaction hash
+ Receipt hash
+ Code hash
    * 同时提供了 poseidon codehash 和 keccack codehash
+ Merkle Patricia Trie's hash

不会替换掉：

+ `SHA3` opcode
    * 这个不应该替换，否则语义、定义不一致
+ `CREATE`/`CREATE2` 计算出来的合约地址
    * 因为有些开发者需要直接计算出这个地址（比如 uniswap fork）所以为了兼容性应该和 Layer1 的逻辑保持一致（使用 `keccak256`）

### 导致的后果

由于没什么人会在合约中直接计算 Transaction hash（无法计算）、Receipt hash（无法计算）；而 `CREATE`、`CREATE2` 出来的 contract addresses 又因为保留了 `keccak256` 和原来一致，所以对用户写 dapp 没什么影响。

可能有影响的是，Merkle Patricia Trie 的改变会影响到 blockheader 的计算。比如 https://github.com/lidofinance/curve-merkle-oracle 就在合约中直接计算了 blockheader。但是 Lido 也说自己会废弃掉这个合约（以后找 chainlink 拿 blockheader）。（并且直接在合约中计算 blockheader 本来也不是一种好方式，比如假如以太坊升级为 verkle tree，lido 的这个合约也会被影响、作废。）

## 3. FRI

## 4. 移除零知识

zk-Rollups 用的其实是 zkp 里面 succinctness 的特性对数据进行压缩，zero-knowledge 反而并不是必要的特性。于是我们可以移除 zero-knowledge 来提升效率。
